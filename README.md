# Customer-Lifetime-Value
Monetary value of transactions/purchases made by a customer with a business over his entire lifetime(time period till a customer purchases with particular Business before moving to competitors)

<h2>Data Schema</h2>
Data is divided in multiple datasets for better understanding and organization. Refering with the following data schema when working with it is helpfull:

![image](https://user-images.githubusercontent.com/26667491/141727281-8f6f8b48-002a-4570-9775-3755f5e0a865.png)

----
<h3>Prerequisite:</h3>

* Prob. Den. Function
* Poisson Distribution
* Gamma Distribution
* P value
* Beta Distribution

----

<h3><center>CLV = ((Average Sales * Purchase Frequency) / Churn) * Profit Margin</center></h3>
Here: <br>

* `Average Sales` = `Total Sales` / `Total Number of Orders`
* `Purchase Frequency` = `Total number of Orders` / `Total Unique Customers`
* `Retention Rate` = `Total numer of orders > 1` / `Total Unique Customers`
* `Churn` = `1` / `Retention Rate`
* `Profit Margin` = `Based on business context`
    * Assume Profit Margin say any number%

-----
**`What I am trying to do!!!`**:
* To predict future sales and monetary values of customers by using historical transactional data from the business


Some frameworks/models to handle such complexity with ease are
>1. **`Historical Approach`**: <br>
    1.1. `Aggregate Model`—calculating the CLV by using `average revenue per customer based on past transactions`.This method gives us a single value for the CLV. <br>
    1.2. `Cohort Model`—grouping customers into different cohorts based on `transaction date`, etc. and `calculate average revenue per cohort`. This method gives CLV value for each cohort.


>2. **`Predictive Approach`**:<br>
    2.1. `Machine Learning Model`—using `regression techniques` to fit on past data to predict CLV.<br>
    2.2. `Probabilistic Model`—it tries to `fit a probability distribution to data and estimates future count of transactions and monetary value for each transaction`<br>

-----
<h3><center>Historical Approach</center></h3><br>

* `Aggregate Model` => considers all customer as one group

* `Cohort Model` => makes diff. groups out of all customer
    * main assumption => customers within a cohort spend similarly or in general behave similarly
    * most common way `to group customers into cohorts` is by start date of a customer, typically by month
    *  best choice will depend on `customer acquisition rate`, `seasonality of business`, and `whether additional customer information can be used`

---
---
<h3><center>Predictive Approach</center></h3><br> 

* `Probabilistic Model`
    * This class of models tries to fit a probability distribution to data and then use that information to estimate other parameters of CLV equation (such as number of future transactions, future monetary value, etc)
    * There are various probabilistic models out there that can be used to predict future CLV
    * Not all variables in the CLV equation can be predicted using a single model
    * `Usually:`
        * `Transaction variables(Purchase freq & Churn)` and `Monetary variables(Avg order value)` are modeled separately
        
* `Predicting Future Transaction and Churn`(Purchase Freq & Churn)
    * `Pareto`/NBD Model
        * one of the most used methods in CLV calculations
    * `BG`/NBD Model => `Beta Geometric`/Negative Binomial Distribution
        * one of the most commonly used probabilistic models for predicting CLV
        * alternative to Pareto/NBD model
    * `MBG`/NBD Model
    * `BG`/BB Model
    
* `Predicting Future Monetary Values`(Avg Order Values)
    * `Gamma-Gammma Model` => adds monetary aspect of customer transaction 


<h4><center>`BG/NBD model has few assumptions`:</center></h4>

1. When a user is active, number of transactions in a `time-t` is described by Poisson distribution with rate `lambda`

2. Heterogeneity in transaction across users (difference in purchasing behavior across users) has `Gamma distribution` with `shape parameter-r` and `scale parameter-a`

3. Users may become inactive after any transaction with `probability-p` and their dropout point is distributed between purchases with Geometric distribution

4. Heterogeneity in dropout probability has `Beta distribution` with the two shape parameters `alpha and beta`

5. Transaction rate and dropout probability vary independently across users

--------

**[Lifetimes Package](https://lifetimes.readthedocs.io/en/latest/):**
This package is primarily built to aid customer lifetime value calculations, predicting customer churn, etc. It has all the major models and utility functions that are needed for CLV calculations

import lifetimes #importing lifetime package


`First`, we need to `create a summary table` from `transaction data`
    * summary table is nothing but an RFM table 
        * (RFM — Recency, Frequency and Monetary value)
Helps in finding:
* `frequency` — the number of repeat purchases (more than 1 purchases)
* `recency` — the time between the first and the last transaction
* `T` — the time between the first purchase and the end of the transaction period (last date of the time frame considered for the analysis)
* `monetary_value` — it is the mean of a given customers sales value
