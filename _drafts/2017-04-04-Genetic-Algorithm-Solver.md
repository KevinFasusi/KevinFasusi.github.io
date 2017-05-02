---
layout: posts
excerpt_separator: <!--more -->
title: Sesnse Checking Evolutionary Algorithm Solver For Exponential Smoothing Forecasts
comments: False
---

{% include header.html %}

The first forecasting techniques implemented in supplychainpy were, simple and holt's trend corrected exponential smoothing. I wanted to get some sense of whether the optimisation using the genetic algorithm (GA) solver, was working well. <!--more --> 

The goal of the GA solver was to find the best smoothing constant, that generated the minimum sum of standard errors. There is a risk that when searching for a minimum value with a GA algorithm results in a local minimum.

For the simple exponential forecast, 2000 results were plotted using random values for the alpha between 0 and 1. The optimisation solver flag was set to `false`. The default setting for the GA are `initial_population = 100,  recombination_type='single_point', mutation_probability = 0.2`.

{% highlight Python %}

from mpl_toolkits.mplot3d import Axes3D
import multiprocessing as mp
import matplotlib.pyplot as plt
from matplotlib import rcParams, cm
import seaborn as sb
import random 
import numpy as np
from supplychainpy.model_demand import simple_exponential_smoothing_forecast
from supplychainpy.model_demand import holts_trend_corrected_exponential_smoothing_forecast
rcParams['figure.figsize'] = 6,5
cores = int(mp.cpu_count()) - 1

ses_alpha = []
ses_standard_error = []
with mp.Pool(processes =cores) as pool:
    ses_forecast = [pool.apply_async(simple_exponential_smoothing_forecast, args = (orders, round(random.uniform(0,1),4), False)) for i in range(0,100)]
    ses_results = [forecast.get() for forecast in ses_forecast]
    ses_alpha = [results.get('optimal_alpha') for results in ses_results]
    ses_standard_error = [results.get('standard_error') for results in ses_results]
fig, ax = plt.subplots()
ax.scatter(ses_alpha, ses_standard_error)
ax.set_xlabel('alpha')
ax.set_ylabel('standard error')

{% endhighlight %}

The plot below shows the result for the random alpha values.

![ses forecast]({{base}}/assets/ses_standard_err_alpha.png "solution space")


The simple exponential smoothing forecast was run again 2000 times, this time using the optimisation solver. The resulting scatter graph shows the results group around a minimum value.

![ses forecast]({{base}}/assets/ses_standard_err_alpha2.png "solution space")


The solver seems to be finding locating a value that is close to the minimum.

A similar process was followed for the Random alpha and gamma values were generated for holt's trend corrected exponential smoothing


To begin the I generated some forecasts using the supplychainpy library:


The results from the excel solver and the implementation in the supplychainpy library were similar. Searching for the minimum value can make it possible that the solution is a local minima and not the global minima. So for a quick sanity check, I ran the solver for a random number of alpha value ( 0< alpha < 1) and plotted the resulting standard error. The code was run inside a Jupyter notebook (there is a link to the notebok at the bottom of the page).

The python implementation is fast and coverges on a solution in (using the %%timeit function in jupyter nootbook).

{% highlight Python linenos %}
import multiprocessing as mp
import random
import numpy as np
import matplotlib.pyplot as plt

from matplotlib import rcParams, cm
from mpl_toolkits.mplot3d import Axes3D

from supplychainpy.model_demand import simple_exponential_smoothing_forecast
from supplychainpy.model_demand import holts_trend_corrected_exponential_smoothing_forecast
rcParams['figure.figsize'] =6,5
cores = int(mp.cpu_count())
cores -= 1

orders = [165, 171, 147, 143, 164, 160, 152, 150, 159, 169, 173, 203, 169, 166, 162, 147, 188, 161, 162,
          169, 185, 188, 200, 229, 189, 218, 185, 199, 210, 193, 211, 208, 216, 218, 264, 304]

htces_alphas = []
htces_gammas = []
htces_standard_error = []

with mp.Pool(processes=cores) as pool:
        htces_forecast = [pool.apply_async(holts_trend_corrected_exponential_smoothing_forecast, args =(orders, round(random.uniform(0,1),4), round(random.uniform(0,1),4), False)) for i in range(0,3000)]
        #print(htces_forecast)
        htces_results = [forecast.get() for forecast in htces_forecast]
        #print(htces_results)
        htces_alphas = [results.get('optimal_alpha', results.get('alpha')) for results in htces_results]
        #print(htces_alphas)
        htces_gammas = [results.get('optimal_gamma', results.get('gamma')) for results in htces_results]
        #print(htces_gammas)
        htces_standard_error = [results.get('standard_error', results.get('standard_error')) for results in htces_results]
        #print(htces_standard_error)
fig = plt.figure()
ax = Axes3D(fig)
ax.set_xlabel('alpha')
ax.set_ylabel('gamma')
ax.set_zlabel('SSE')

ax.scatter(htces_alphas, htces_gammas, htces_standard_error)
{% endhighlight %}

I checked to see if the solutions are good enough i.e. not always finding a local minima. After plotting 3000 results the solution space looked like the chart below:

![Holt's Trend Corrected Solution Space]({{base}}/assets/htces_random_alpha_and_gamma_no_optimisation.png "solution space")

After plotting several random values constants, I ran it again and this time set the optimise flag in the library to True (true by default so just removed the False value for the parameter) the optimised results show a clustering around a the lowest standard error between 20.9 an 20.1 this would indicate that the genetic algorithm is working okay.

![Holt's Trend Corrected Exponential Smoothing Alpha, Gamma, standard error 3D Plot ]({{base}}/assets/3D_scatter_plot_holts_trend_corrected_es.png "3D Plot")


The Jupyter Notebook used can be found [here]()


