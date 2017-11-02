---
layout: posts
excerpt_separator: <!--more -->
title: Profiling Evolutionary Algorithm Solver
comments: False
---
The first forecasting techniques implemented in supplychainpy were, "simple" and "holt's trend corrected" exponential smoothing. I wanted to get a sense of whether the optimisation using the genetic algorithm (GA) solver, was working. <!--more --> 

The goal of the GA solver is to find a "good enough" smoothing constant, resulting in the smallest value possible for the sum of standard errors. When searching for a minimum value, there is a risk that the result is a local minimum.

To explore whether, the current solver is finding an acceptable or "good enough" constant, I ran the `simple_exponential_smoothing_forecast()` 50 times using random alpha values. The random values for the alpha, where between zero and one with the optimisation flag set to `optimise=false`. The default setting for the GA are `initial_population=100,  recombination_type='single_point', mutation_probability=0.2` (currently these are fixed in the library within the population class [here](https://github.com/KevinFasusi/supplychainpy/blob/master/supplychainpy/demand/_evo_algo.pyx) and are not exposed to the public API).

{% highlight Python %}

import multiprocessing as mp
import matplotlib.pyplot as plt
from matplotlib import rcParams, cm
import random 
from supplychainpy.model_demand import simple_exponential_smoothing_forecast
rcParams['figure.figsize'] = 6,5
cores = int(mp.cpu_count()) - 1
orders = [165, 171, 147, 143, 164, 160, 152, 150, 159, 169, 173, 203, 169, 166, 162, 147, 188, 161, 162,
          169, 185, 188, 200, 229, 189, 218, 185, 199, 210, 193, 211, 208, 216, 218, 264, 304]
ses_alpha = []
ses_standard_error = []
maximum = 50
rnd_nums = [round(random.uniform(0,1),4) for i in range(0, maximum)]
with mp.Pool(processes =cores) as pool:
    ses_forecast = [pool.apply_async(simple_exponential_smoothing_forecast, args = (orders, rnd_nums[i],False)) for i in range(0,maximum)]
    ses_results = [forecast.get() for forecast in ses_forecast]
    ses_alpha = [results.get('optimal_alpha') for results in ses_results]
    ses_standard_error = [results.get('standard_error') for results in ses_results]
fig, ax = plt.subplots()
ax.scatter(ses_alpha, ses_standard_error)
ax.set_xlabel('alpha')
ax.set_ylabel('standard error')

{% endhighlight %}

The plot below shows the result for the random alpha values.

![ses forecast]({{base}}/assets/ses_rnd.png "solution space")

Running the example 50 times is not enough to get an adequate view of the possible results obtained without using the solver. Unfortunately, the current implementation for `simple_exponential_smoothing` ([here](https://github.com/KevinFasusi/supplychainpy/blob/master/supplychainpy/demand/_forecast_demand.py)) uses a generator to 'yield' the results. The [public API](https://github.com/KevinFasusi/supplychainpy/blob/master/supplychainpy/model_demand.py) in the module (`from supplychainpy.model_demand import simple_exponential_smoothing_forecast`), constantly calls the function that relies on the generator. In an attempt to increase the speed of the computation, a processing pool from the `multiprocessing` module in the Python standard library is used. Unfortunately, the use of a 'pool' causes the process sometimes to hang and remain uncompleted. After using the logging function, it seems that the generator function is called an ungodly amount of times.

I have tried implementing this as a batch job but have been unsuccessful. I guess I have to rewrite the forecast implementation and remove the generators. At least, I hope that is the solution. Honestly, this problem has been giving me the middle finger for a while. 

The simple exponential smoothing forecast was run again (50 times), this time using the optimisation solver. The resulting scatter graph shows the results group around a minimum value.

![ses forecast]({{base}}/assets/ses_standard_err_alpha2.png "solution space")


The solver seems to be finding a value that is close to the minimum. A similar process was followed for random alpha and gamma values using `holts_trend_corrected_exponential_smoothing_forecast()`.

{% highlight Python linenos %}
import multiprocessing as mp
import random
import matplotlib.pyplot as plt

from matplotlib import rcParams, cm
from mpl_toolkits.mplot3d import Axes3D

from supplychainpy.model_demand import holts_trend_corrected_exponential_smoothing_forecast

rcParams['figure.figsize'] = 6,5
cores = int(mp.cpu_count()) - 1
orders = [165, 171, 147, 143, 164, 160, 152, 150, 159, 169, 173, 203, 169, 166, 162, 147, 188, 161, 162,
          169, 185, 188, 200, 229, 189, 218, 185, 199, 210, 193, 211, 208, 216, 218, 264, 304]
htces_alphas = []
htces_gammas = []
htces_standard_error = []

with mp.Pool(processes=cores) as pool:
        htces_forecast = [pool.apply_async(holts_trend_corrected_exponential_smoothing_forecast, args =(orders, round(random.uniform(0,1),4), round(random.uniform(0,1),4), False)) for i in range(0,2000)]
        htces_results = [forecast.get() for forecast in htces_forecast]
        htces_alphas = [results.get('optimal_alpha', results.get('alpha')) for results in htces_results]
        htces_gammas = [results.get('optimal_gamma', results.get('gamma')) for results in htces_results]
        htces_standard_error = [results.get('standard_error', results.get('standard_error')) for results in htces_results]
fig = plt.figure()
ax = Axes3D(fig)
ax.set_xlabel('alpha')
ax.set_ylabel('gamma')
ax.set_zlabel('SSE')

ax.scatter(htces_alphas, htces_gammas, htces_standard_error)
{% endhighlight %}

The random values for the constants generate a spread of results, without any clear indication of "good enough" solutions. 

![Holt's Trend Corrected Solution Space]({{base}}/assets/htces_random_alpha_and_gamma_no_optimisation.png "solution space")

Running the solver again, with the optimise flag set to `optimise=True`, shows clustering around a 'good enough' standard error between 20.9 and 20.1. The results would indicate that the genetic algorithm is working. I will explore how well it is working in another post.

![Holt's Trend Corrected Exponential Smoothing Alpha, Gamma, standard error 3D Plot ]({{base}}/assets/3D_scatter_plot_holts_trend_corrected_es.png "3D Plot") 