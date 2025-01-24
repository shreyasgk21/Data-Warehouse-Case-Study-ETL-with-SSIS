# Data Warehouse Case Study ETL with Approach:

So, in this case study I want to first plan my way of approach. The very first step is to understand the problem and plan how I will tackle it. Next, I will setup the necessary tables and the schema for the data warehouse. Then, I would design the staging the facts and the dimensions. Setting up the core layer for the dimensions and fact tables would be next in the process. Finally, I would setup a job and test the entire ETL flow. In this case study, I will be using sales data from a company. The tools I will be using for this case study are:

•	SQL Server Management Studio 19

•	SQL Server Integration Services

•	Excel

I had a fact_sales table that needed to be extracted and transformed into a data warehouse which I hosted on SQL Server on premesis. It contained the following columns.

| transaction_id | transactional_date | product_id | customer_id | payment | credit_card | loyalty_card	| cost | quantity | price |
| -------------- |:------------------:| ----------:| -----------:| -------:| -----------:| ------------:| ----:| --------:| -----:|
