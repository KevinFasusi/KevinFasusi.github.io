---
layout: posts
excerpt_separator: <!--more -->
title: Monte Carlo Simulation Process
comments: False
---

Building a simulation requires an abstraction of the processes involved, for the construction of an appropriate model. A description of the process is a useful starting point.<!--more --> Below is a flow diagram illustrating the sequence of activities that describe the simulation process for the discrete event inventory simulation in the supplychainpy library:

![sim_process pic]({{base}}/assets/sim_process.jpg "simulation process")


## Describing the Process

Building a simulation requires an abstraction of the processes involved, for the construction of an appropriate model. Therefore, a description of the process is a useful starting point.<!--more --> Below is a flow diagram illustrating the sequence of activities that describe the simulation process for the discrete event inventory simulation for supplychainpy.

The process shown above starts by checking which period is currently being simulated. If the process is for the first period, then the 'opening stock' is assumed to be the amount calculated for the reorder level. This assumption makes it highly likely that a purchase order is raised in the first period. For all other periods, the 'opening stock' will be the 'closing stock' for the previous cycle. Demand is randomised based on the probability distribution of the historical demand profile for the SKU under simulation. Any deliveries due, get added to the inventory. The resulting stock is decremented by back orders and new order. The quantity on hand and reorder level are compared. If the amount on hand is less than the reorder level, then a purchase order is raised. Calculating the backlog and the closing stock for the next period are the final steps in the process.


## Using the Supplychianpy Library

The supplychainpy feature for running a Monte Carlo Simulation uses the demand profile from the data source (the .csv used to generate the report) to produce a randomised order. For example, the raw data for SKU **KR202-209** looks like this:

```
 Sku, jan, feb, mar, apr, may, jun, jul, aug, sep, oct, nov, dec, unit cost, leadtime, retail price, quantity on hand, backlog
KR202-209, 1509, 1855, 2665, 1841, 1231, 2598, 1988, 1988, 2927, 2707, 731, 2598, 1001, 2, 5000, 1003, 10
```

The features do not currently have a function for running the simulation for a single SKU, so in the example below a small sample file bundled in the library is used:

```python
from supplychainpy import simulate
from supplychainpy.model_inventory import analyse_orders_abcxyz_from_file
from supplychainpy.sample_data.config import ABS_FILE_PATH

orders_analysis = analyse_orders_abcxyz_from_file(file_path=ABS_FILE_PATH['COMPLETE_CSV_SM'],
                                                  z_value=Decimal(1.28),
                                                  reorder_cost=Decimal(5000),
                                                  file_type="csv",
                                                  length=12
                                                  )

sim = simulate.run_monte_carlo(orders_analysis=orders_analysis,
                               runs=100,
                               period_length=12
                              )
```

The 100 runs take approximately 2s (on an i7 laptop with 8g ram under heavy load). The new implementation makes use of parallel processing and cython which has resulted in a significant improvement over the previous implementation written in pure Python and not using the 'Multiprocessing' library.  

The **runs** keyword argument for the `simulate.run_monte_carlo()` is set to '100', so the simulation will run 100 times. Each series will generate a unique demand profile and subsequent transactions as described in the process above. The `simulate.run_monte_carlo()` function returns a list for each transaction. Below is an example of a set of results (1 run) for SKU **KR202-209** over a 12 month period:

```
[{'demand': '1905', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 31', 'period': '1', 'opening_stock': '4069', 'index': '1', 'po_quantity': '2892', 'delivery': '0', 'revenue': '2166338', 'sku_id': 'KR202-209', 'po_received': '', 'closing_stock': '2164', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '2164'}]
[{'demand': '1953', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 41', 'period': '2', 'opening_stock': '2164', 'index': '1', 'po_quantity': '4845', 'delivery': '0', 'revenue': '211110', 'sku_id': 'KR202-209', 'po_received': '', 'closing_stock': '211', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '211'}]
[{'demand': '2984', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 51', 'period': '3', 'opening_stock': '211', 'index': '1', 'po_quantity': '4937', 'delivery': '2892', 'revenue': '118928', 'sku_id': 'KR202-209', 'po_received': 'PO 30', 'closing_stock': '119', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '119'}]
[{'demand': '1865', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 61', 'period': '4', 'opening_stock': '119', 'index': '1', 'po_quantity': '1957', 'delivery': '4845', 'revenue': '3101842', 'sku_id': 'KR202-209', 'po_received': 'PO 40', 'closing_stock': '3099', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '3099'}]
[{'demand': '3501', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 71', 'period': '5', 'opening_stock': '3099', 'index': '1', 'po_quantity': '521', 'delivery': '4937', 'revenue': '4539073', 'sku_id': 'KR202-209', 'po_received': 'PO 50', 'closing_stock': '4535', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '4535'}]
[{'demand': '1685', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 81', 'period': '6', 'opening_stock': '4535', 'index': '1', 'po_quantity': '250', 'delivery': '1957', 'revenue': '4811111', 'sku_id': 'KR202-209', 'po_received': 'PO 60', 'closing_stock': '4806', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '4806'}]
[{'demand': '1367', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 91', 'period': '7', 'opening_stock': '4806', 'index': '1', 'po_quantity': '1096', 'delivery': '521', 'revenue': '3964091', 'sku_id': 'KR202-209', 'po_received': 'PO 70', 'closing_stock': '3960', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '3960'}]
[{'demand': '2359', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 101', 'period': '8', 'opening_stock': '3960', 'index': '1', 'po_quantity': '3205', 'delivery': '250', 'revenue': '1853134', 'sku_id': 'KR202-209', 'po_received': 'PO 80', 'closing_stock': '1851', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '1851'}]
[{'demand': '3195', 'previous_backlog': '-248', 'shortage_cost': '0', 'po_raised': 'PO 111', 'period': '9', 'opening_stock': '1851', 'index': '1', 'po_quantity': '4808', 'delivery': '1096', 'revenue': '0', 'sku_id': 'KR202-209', 'po_received': 'PO 90', 'closing_stock': '0', 'backlog': '-248', 'shortage_units': '0', 'quantity_sold': '0'}]
[{'demand': '1798', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 121', 'period': '10', 'opening_stock': '0', 'index': '1', 'po_quantity': '3649', 'delivery': '3205', 'revenue': '1408257', 'sku_id': 'KR202-209', 'po_received': 'PO 100', 'closing_stock': '1407', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '1407'}]
[{'demand': '2606', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': 'PO 131', 'period': '11', 'opening_stock': '1407', 'index': '1', 'po_quantity': '1448', 'delivery': '4808', 'revenue': '3612025', 'sku_id': 'KR202-209', 'po_received': 'PO 110', 'closing_stock': '3608', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '3608'}]
[{'demand': '1933', 'previous_backlog': '0', 'shortage_cost': '0', 'po_raised': '', 'period': '12', 'opening_stock': '3608', 'index': '1', 'po_quantity': '0', 'delivery': '3649', 'revenue': '5329768', 'sku_id': 'KR202-209', 'po_received': 'PO 120', 'closing_stock': '5324', 'backlog': '0', 'shortage_units': '0', 'quantity_sold': '5324'}]

```


## Summarising The Simulation

Summarising the results from the multiple runs offers an overview of the performance of the SKU within the system. From the summary, it is possible to make many operational inferences.


```python
from supplychainpy.simulate import summarise_frame

frame_summary = simulate.summarise_frame(sim_window)

for item in frame_summary:
	if item.get('sku_id')=='KR202-209':
		print(item)
```

output (table):

<table>
	<tr>
		<th> SKU ID </th>
		<td> KR202-209 </td>
	</tr>
	<tr>
		<th> Service Level (%) </th>
		<td> 93.95 </td>
	</tr>
	<tr>
		<th> Quantity Sold (STDEV.) </th>
		<td> 1811  </td>
	</tr>
	<tr>
		<th> Quantity Sold (AVG) </th>
		<td> 3096  </td>
	</tr>
	<tr>
		<th> Quantity Sold (MIN) </th>
		<td> 26</td>
	</tr>
	<tr>
		<th> Quantity Sold (MAX) </th>
		<td> 8710 </td>
	</tr>
	<tr>
		<th> Opening Stock (MAX.) </th>
		<td> 8710  </td>
	</tr>
	<tr>
		<th> Opening Stock (MIN.) </th>
		<td> 0  </td>
	</tr>
	<tr>
		<th> Opening Stock (VAR.) </th>
		<td> 1756  </td>
	</tr>
	<tr>
		<th> Closing Stock (AVG) </th>
		<td> 2929  </td>
	</tr>
	<tr>
		<th> Closing Stock (STDEV.) </th>
		<td> 1811  </td>
	</tr>
		<tr>
		<th> Closing Stock (MIN) </th>
		<td>  0  </td>
	</tr>
		</tr>
	<tr>
		<th> Closing Stock (MAX) </th>
		<td> 7081  </td>
	</tr>
	<tr>
		<th> Backlog (AVG) </th>
		<td> 39  </td>
	</tr>
	<tr>
		<th> Backlog (STDEV.) </th>
		<td> 198 </td>
	</tr>
	<tr>
		<th> Backlog (MIN) </th>
		<td> 39 </td>
	</tr>	
	<tr>
		<th> Shortage (AVG) </th>
		<td> 0 </td>
	</tr>
</table>


## The Reporting View

I am currently adding the functionality for running the simulation from the reporting view and searching the simulation in the reports. I am also making improvements to the process model and expanding the basic model to be more flexible for describing various real systems. Below is a screenshot of the current state of the simulation view in the reporting:
 
![sim pic]({{base}}/assets/sim.png "sim")

As of writing this post, the view hogs the main thread when required to compute 1000 runs or more. Currently, the only front end functionality available in the simulation view is the ability to specify the number of runs. Release 0.0.5 should see the feature complete simulation view implemented in the library.
 