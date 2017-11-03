---
layout: posts
excerpt_separator: <!--more -->
title: AWS Lambda and Supplychainpy (there was an attempt)
comments: False
---

I recently attempted to use supplychainpy from an AWS lambda function, and it did not go well. The problem was not the library itself or the process required to create a python deployment package, with all the environment dependencies, and uploading to AWS. <!--more -->I document some of the process below. This post is not a tutorial but I suppose it might be useful as confirmation of how not to go serverless.

## Building the Deployment Package

I used Anaconda on an Ubuntu Linux machine to build the virtual environment for deploying the package. AWS offers Python 3.6 and 2.7 for building lambda functions. Using the conda command, I built an environment for python 3.6:

`$ conda create -n suchpy_lambda python=3.6`

After creating and activating the new environment, I installed the dependencies (Cython for my project and Pillow and boto3 for AWS):
    
`$ pip install Pillow boto3 Cython`

I then installed 'supplychainpy' (0.0.5rc0):

`$ pip install supplychainpy==0.0.5rc0`
  
To package all the dependencies I navigated to the anaconda site-packages for the environment created earlier:

`$ cd ~/anaconda/envs/suchpy_lambda/lib/python3.6/site-packages`

I zipped the dependencies found in the virtual environments site-packages and moved the compressed file to a project directory:

`$ zip -r9 ~/Projects/python/supplychainpylambda/scmlambda.zip *`

After creating `scmlambda.zip` and moving it to a project directory. I created a `scmlambda.py` file and added the following function:

{% highlight Python %}

from __future__ import print_function
import boto3
import json as js
import os
import sys
from decimal import Decimal
from supplychainpy import model_inventory

def handler(event, context):
    data_set = event['data_set']
    data_set = dict(js.loads(data_set))
    z_value = Decimal(event['z_value'])
    lead_time = Decimal(event['lead_time'])
    holding_cost_percentge = Decimal(event['holding_cost_percentage'])
    retail_price = Decimal(event['retail_price'])
    unit_cost = Decimal(event['unit_cost'])
    reorder_cost = Decimal(event['reorder_cost'])
    quantity_on_hand = Decimal(event['quantity_on_hand'])
    backlog = Decimal(event['backlog'])
    sku_id = event['sku_id']
    currency = event['currency']
    uncertain_demand = model_inventory.analyse_orders(data_set=data_set,
                                                      sku_id= sku_id,
                                                      lead_time= lead_time,
                                                      reorder_cost= reorder_cost,
                                                      z_value = z_value,
                                                      retail_price= retail_price,
                                                      unit_cost= unit_cost,
                                                      currency=currency,
                                                      quantity_on_hand=quantity_on_hand
                                                     )
   return uncertain_demand
  
if __name__ == "__name__":
	handler()
	    
{% endhighlight %}

This function from the supplychainpy library provides a summary of basic descriptive statistics and inventory KPI's. The aim was to use an API Gateway as a trigger and send values to the function using JSON. As I explain below, I didn't even get the chance to see if this specific function would work.

Now the newly created python file is added to the previously created zip file:

`zip -g scmlambda.zip scmlambda.py`

At this point, all seemed to be going well. I had read elsewhere that you might want to do all of this on an AWS server instance. So I may have faced problems even if I had succeeded in uploading the deployment package.

To upload the Python deployment package, I selected an AWS blueprint, specifically the `hello-world-python3`.

![AWS blueprint]({{base}}/assets/aws_blueprints.png "AWS blueprints")

Once you have chosen a blueprint, named it and selected a role, you choose a code entry type. I uploaded from an S3 instance, which requires copying the link to the location of the file over to the Lambda function. 

It was at this stage I realised my package was too big to upload directly as the maximum is 10mb, so you are required to upload to an S3 bucket. 

I received the error message that read:

 *The Code tab failed to save. Reason: Unzipped size must be smaller than 262144000 bytes*:

![AWS Storage]({{base}}/assets/aws_storage.png "AWS Storage Limit")

I believe my failure underscores the point of a lambda function and how my attempt flies in the face of that logic. Bundling all the functionality of into a library that the function can use is not a sensible choice. It would mean each lambda would have a version of the library etc. 

I am still experimenting with AWS lambdas and the library but my initial attempt to use it lump sum in a single function was not a success. It would make more sense to separate all the library functionality across different lambda functions. I might give that a try when I have the chance.

