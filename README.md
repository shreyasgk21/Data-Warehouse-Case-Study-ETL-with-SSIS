# Data Warehouse Case Study ETL with SSIS:

So, in this case study I want to first plan my way of approach. The very first step is to understand the problem and plan how I will tackle it. Next, I will setup the necessary tables and the schema for the data warehouse. Then, I would design the staging the facts and the dimensions. Setting up the core layer for the dimensions and fact tables would be next in the process. Finally, I would setup a job and test the entire ETL flow. In this case study, I will be using sales data from a company. The tools I will be using for this case study are:

•	SQL Server Management Studio 19

•	SQL Server Integration Services

•	Excel

I had a fact_sales table that needed to be extracted and transformed into a data warehouse which I hosted on SQL Server on premesis. It contained the following columns.

| transaction_id | transactional_date | product_id | customer_id | payment | credit_card | loyalty_card	| cost | quantity | price |
| -------------- |:------------------:| ----------:| -----------:| -------:| -----------:| ------------:| ----:| --------:| -----:|


*Designing the Data Warehouse:*

I used a star schema for simplicity and performance:

1.Fact Table: FactSales

    Includes: transaction_id, transactional_date, product_id, customer_id, payment, credit_card, loyalty_card, cost, quantity, price
    
2.Dimension Tables:

    DimProduct: Contains details about products (product_id, product_name, category).
    
    DimCustomer: Contains details about customers (customer_id, name, location).
    
    DimDate: Contains details about dates (date_id, day, month, year, quarter).
   
    DimPayment: Contains payment methods (payment_id, payment_type, credit_card_provider).

*Creating the Data Warehouse in SQL Server:*

Using the following SQL queries I was able to create the required schema for the tables:

  DimProduct Table:
  
    CREATE TABLE DimProduct (
        product_id INT PRIMARY KEY,
        product_name VARCHAR(255),
        category VARCHAR(255)
     );
     
DimCustomer Table:
 
    CREATE TABLE DimCustomer (
        customer_id INT PRIMARY KEY,
        customer_name VARCHAR(255),
        location VARCHAR(255)
    );
    
DimDate Table:
   
    CREATE TABLE DimDate (
        date_id INT PRIMARY KEY,
        date DATE,
        day INT,
        month INT,
        year INT,
        quarter INT
    );
    
DimPayment Table:

    CREATE TABLE DimPayment (
        payment_id INT PRIMARY KEY,
        payment_type VARCHAR(50),
        credit_card_provider VARCHAR(50)
    );
    
FactSales Table:

    CREATE TABLE FactSales (
        transaction_id INT PRIMARY KEY,
        transactional_date DATE,
        product_id INT,
        customer_id INT,
        payment_id INT,
        cost DECIMAL(10, 2),
        quantity INT,
        price DECIMAL(10, 2),
        FOREIGN KEY (product_id) REFERENCES DimProduct(product_id),
        FOREIGN KEY (customer_id) REFERENCES DimCustomer(customer_id),
        FOREIGN KEY (payment_id) REFERENCES DimPayment(payment_id)
    );

*ETL with SSIS:*

I used SQL Server Integration Services to perform ETL operations I loaded the FactSales table and then performed required ETL operations to obtain the other Dimension Tables.

For DimProduct Table:
I mapped the FactSales table to the necessary columns in the DimProduct Table and ran the package.

![image](https://github.com/user-attachments/assets/bd645cc3-dc12-4a8d-bb9c-98b355b34626)

For DimCustomer Table:
For DimCustomer, I aggreagated the data from the customer_id column of the FactSales Table by using a group by operation

![image](https://github.com/user-attachments/assets/194e97bf-4755-4251-8939-63ebc15d0a39)

For DimDate Table:
For DimDate I used a derived column tranformation in order to get the date value from the FactSales Table
I used the following expression in order to get the desired output:
            
            LEFT((DT_WSTR,20)transactional_date,10)

![image](https://github.com/user-attachments/assets/fa1a77f6-e237-4454-bbe2-77d234d7efd7)

For DimPayment Table:
I mapped the FactSales table to the necessary columns in the DimPayment Table and ran the package.

![image](https://github.com/user-attachments/assets/c361b25d-f51a-4723-8583-8ebad3e4d6e0)


*Concclusion*
From this case study, I was able to design and create a efficient data warehouse using a star schema to boost performance. I was able to carry out ETL operations to get the required Dimension Tables. With Jobs this would be an effective solution for an organisation to run on a regular basis.




