---
layout: posts
excerpt_separator: <!--more -->
title: Thoughts on Supplychainpy Release 0.0.4
comments: False
---

For a few months now I have been working on a project for Supply Chain reporting and analysis. It has come a little way but is far from feature complete. Currently, the project is on planning release 0.0.3 and 0.0.4 is just around the corner. <!--more --> 

### Issues

The main problems that are holding up the release, and even after release will be in a less than ideal state are:

- The sequential computation of the forecasts using evolutionary algorithms for optimised alpha and gamma values.
- The view for the SKU level details has become a sprawling kludge of HTML and Jinja. 

### Sprawling HTML and Jinja

For small inventory profiles < 500 SKUs the implementation of the forecasting is not too prohibitive. However, in more real-world scenarios it needs to deal with inventory profiles that are bigger by orders of magnitude. One issue here is that the forecasting problem is an embarrassingly parallel problem. The current implementation is sequential, parallelising this code should speed it up considerably. I love Python; I do, but gaining speed using parallel processing is not as easy as other programming languages I have used (looking at you vb.net, for all the bad press you get, boy you let me do some powerful things effortlessly).

The kludge of HTML and Jinja is easy to resolve by using the macro function inside Jinja, so that is what I will be doing going forward. 

### Charts

Other general issues, at this point, are the charting and choice of front-end framework. Some of the charts are in d3.js, but that library sure has a steep learning curve. So for speed's sake, I switched some to Flotr library early on in the development process. The bar charts and pie charts on the *Dashboard* are d3.js as shown below:

![d3 charts]({{base}}/assets/d3.jpg "d3 charts")

The bar chart and line chart on the *SKU breakdown* page are Flotr:

![Flotr bar chart]({{base}}/assets/bar_chart.jpg "flotr bar chart")

![Flotr line chart]({{base}}/assets/line_chart.jpg "flotr line chart")
 
I plan to stick with pure d3.js, but I am a mere mortal and d3.js is written by the gods for the gods. 

### Choosing a Front-end Framework

I have been deliberating over which framework to use in the front-end. Initially, I opted for vanilla ECMAScript 6 and “a bag of Jquery” (shout out to the guys over at the 'Linux Action Show' for that one). For rapid development and getting the MVP down, this made a lot of sense. However, going forward I would like to make the JavaScript for the reporting side of the project just as legible and standardised as I attempt to make the Python back-end. For maintenance and minimising technical debt, this is a must. 

React.js seemed like the clear winner, regarding popularity, documentation and ease of adoption. After a brief tutorial and playing around with it, I did enjoy it but recently another framework has caught my attention. Vue.js is discussed in the many forums I frequent, so I popped over to their website and read the documentation and boom. It was “you’re alright” at first sight. I read the documentation, and it felt comfortable. I went through nodding my head and smiling through the code snippets. So I think I’m going to give vue.js a shot. I hope vue.js and Jinja plays nicely with each other. Here’s to a cleaner code base and a shiny front-end for the reporting.

