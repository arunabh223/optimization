## Problem statement:
Optimizing the picking process at the dark stores. Specifically, how we can leverage batch picking of multiple items 
across orders and be more efficient during peak times.

## Base picker assignment algorithm:
1. Pickers: Free pickers are selected based on a round-robin basis. Then assigned to orders.
2. But single order assignment to pickers leads to significant delays at few stress slots.
3. This leads to a backlog
4. Solution would be order batching
5. The intuition of order batching for pickers is that the pickers would pick similar items at once and 
   save time in travelling within the store.

## Proposed picker assignment algorithm:

Base Algorithm: The picker gets assigned to the order which was placed first, goes to the juice section, 
grabs the juice, comes back to the billing counter and packs the first order. Afterwards, the picker gets 
assigned to the second order, again goes back to the juice section, grabs the juice and again comes back 
to the billing counter and packs the second order.

Proposed Algorithm: The picker gets assigned to both the orders and is shown that he/she has to pick two 
juice bottles, he/she goes to the juice section, grabs both of them, comes back to the billing desk, packs 
the order, which was placed first and then packs the second placed order.

## Mathematical model:

### Input Data:
Inputs to the batching algorithm:
Type of items in the order
Number of items in the order
Bill value of the order
Weight of the order
Predicted time to mark ready.

Active orders which have no picker assigned to them are permuted and combined to form batches with up to k 
orders (we considered k = 2) to be assigned to a single picker. The K-order batches are checked for their 
validity if their weight, bill, and total number of items are below the respective thresholds.

## Set:
B = the set of all batches,
O = the set of all active orders in the dark store.
J = is the set of all pickers in the dark store.
E = set of edges between a given batch i and a picker j

## Parameters:
TTPᵢⱼ = Time to pack iᵗʰ batch by jᵗʰ picker
TTFᵢⱼ = Predicted Time to jᵗʰ picker getting free and start working on iᵗʰ batch
DCᵢⱼ = Delay cost if iᵗʰ batch is assigned now to jᵗʰ picker = func(expected_order_ready_time, predicted_order_ready_time)

## Decision Variables:
bᵢₒ = indicates if order o is in batch i
Xᵢⱼ = indicates if jᵗʰ picker is assigned to iᵗʰ batch in the current cron
Zᵢⱼ= indicates if jᵗʰ picker is not assigned to iᵗʰ batch in the current cron

## Objective Function:
The batches permuted with every picker in the dark store are sent to the optimisation algorithm along with the following 
objective. The future cost is needed to compare the consequences of delaying one batch over another.

![Objective function](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lDm6OJo6JcZH1rG8VeHZRA.png)

Where hat(^) indicates all these terms in the next cron (i.e. decision cycle)

## Constraints:
1. An order should either be assigned in the current cron or the next cron

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*kFYeSPoS5ibs_2S5bEAzQA.png)

2. Every picker can be assigned to only one batch.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*FzjvSZGY1nLIHAco_ho9ZQ.png)

In the objective function, TTP is used to account for Similarity. The calculation of TTP (the first time) accounts for the similarity as time picking for multiple items in the same category is lesser than travelling to other category aisles, identifying and picking the items. The TTP will be different for different batches, if any batch will have similar items it will have lower TTP compared to addition of individual orders and if the batch has very different items (hot/cold) it will have same TTP time as addition of TTP times of individual orders with higher delay cost, making it an inefficient choice.
The time to free (TTF) accounts for picker status, i.e. if he/she is free or the time in which he/she will be free to pick the next order. The delay cost (DC) is to keep the delays beyond their promised order ready time in check.

## Model Settings:
> The limit on the number of orders to be batched is 2. This is done to ensure less confusion for the staff at the dark stores and human errors don’t increase while packing the orders.

In the 2 order batching, if the picker picks the total number of items correctly but packs extra or misses any item in the 1st order, while packing the 2nd order he will have a missing or extra item which will help rectify the mistake if done in a timely manner.
If we increase the number of orders, the above won’t be possible till we reach the last order of the batch.

- The orders are only batched when the free pickers available are less than the number of unassigned orders.
- Validate the batches of the orders.
- The total number of items and the total weight of items in both orders need to be less than the respective upper bound.
- A similarity score is calculated for a pair of orders based on the categories of items and the closeness of their racks in the dark store such that it reduces the time and is checked if it’s above a certain threshold.

## Simulation:
To validate the above order batching algorithm, a discrete-time simpy-based simulation was run.
Input data is real-time orders data, picker data and shelving information for a dark store-date combination.
A cron is run where pickers are checked if free and unassigned batches are assigned till the end time.



