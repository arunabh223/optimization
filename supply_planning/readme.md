## Target

Your target is to balance supply and demand to ensure the best service level at the lowest cost.

## Network diagram
![flow](/Users/arunabhbora/Downloads/code/optimization/supply_planning/img/image.png)

## Problem statement

As a Supply Planning manager of a mid-size manufacturing company, you received feedback that the distribution costs are too high. Based on the analysis of the Transportation Manager, this is mainly due to the stock allocation rules.
Sometimes, your customers are not shipped from the closest distribution centre, which impacts your freight costs.

## Distribution network

- 2 plants producing products with infinite capacity.  
_Note: We’ll see later how we can improve this assumption easily_
- 2 distribution centres that receive finished goods from the two plants and deliver them to the final customers.  
_Note: We will consider that these warehouses operate X-Docking to avoid considering the concept of stock capacity in our model_
- 200 stores (delivery points)

## Link to article
https://towardsdatascience.com/supply-planning-using-linear-programming-with-python-bff2401bf270

## Objective

Reduce the transportation costs, including inbound and outbound shipments.

IC: Inbound cost
OC: Outbound cost
I: Inbound Qty
O: Outboud Qty

Minimise IxIC + OxOC

## Next Steps
The model presented here can be easily improved by adding operational constraints:
- Production Costs in Plants ($/Case)
- Maximal X-Docking Capacity in Distribution Centers (Cartons)
But also by improving the cost structure by adding
- Fixed/Variable Costs Structures in Distribution Centers ($)
- Fixed + Variable Transportation Costs Structure y = (Ax +b)  

The only limit you will find is the linearity of the constraints and the objective functions.


