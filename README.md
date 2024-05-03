# Sales Analysis - AtliQ Hardware
## Problem Statement

The Sales director of Atliq Hardware is facing challenges in tracking sales in a dynamically growing market. The sales director does not have a direct access to track down the performance of the business. Atliq has regional managers in north, south and central regions to monitor business in their respective region. These regional managers responsibility includes communicating the performance of the business to the sales director.

The sales directors receives updates on monthly basis which is incomplete and inadeqaute in terms of numbers. The Sales director is not able to take data-driven decisions as the sales in most regions are declining and has become a huge reason for concern.

Following are the specific problems that the sales director is facing:

- He is unable to track sales in real time. He only receives sales data on a monthly basis, which is too slow to make timely decisions.
- He does not have a clear understanding of how his sales are performing in different regions or in different product categories.
- He is unable to identify trends in his sales data. This makes it difficult for him to predict future sales and make informed decisions about marketing and pricing.

## Solution

The sales director hopes that by building a Power BI dashboard, he will be able to address these problems and improve sales for Atliq Hardware. The dashboard will provide him with real-time sales data, insights into sales performance by region and product category, and trends in his sales data. This information will help make better decisions about marketing, pricing, and product development.

The sales director hopes to view the following specific things in the dashboard which will help him take data-driven decisions:

Real-time sales data :
This will allow him to track sales performance and make timely decisions about marketing, pricing, and product development.

Sales performance by region and product category :
This will give him a clear understanding of how his sales are performing in different areas and for different products.

Trends in sales data :
This will help him predict future sales and make informed decisions about marketing and pricing.

## Data Gathering and Analysis

- The data was initially stored in a database in MySQL text file format

- This MySQL database was imported to MySQL workbench to do further data analysis.
  

#### Below are the name of the tables and its description that the Atliq Sales database contains:

  1) Customers :
  Contains the information regarding the clients involved in business

  2) Date :
  Contains information related to dates on which business was done

  3) Products :
  Contains information regarding the products the company supplies

  4) Markets :
  Contains information regarding the locations where business is done

  5) Transactions : 
  Contains information about the business done with the client

## Extract, Transform, Load and Data Modelling

1) Import the date from MySQL database to Power BI by establishing connection between the two.

2) Transforming the data in Power Query Editor:

- Removed Garbage entries 0 and -1 from sales_amount coloumn in sales_transactions table
  
- Filtered the data to Indian cities only by excluding the businesses from New York and Paris since no records were found for transactions done outside India.

- Created a new column 'norm_sales_amt' for sales amount which has values only in 'INR'. For sales_amount entries in currency 'USD' , the sales_amount will be multiplied with 76 as per the currency conversion.

### Formula to create norm_sales_amt column

``` dax
= Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then [sales_amount]*75 else [sales_amount], type any)
```
  
3) The relationship with the 4 child tables which are customers, markets, products, date was established with the fact table transactions which holds the references (foreign key) from the primary keys of child tables. The relationship being one to many.

4) Applied the final changes and loaded the transformed table to Power BI for data vizualization.

## Creating Base measures

- Table named Base measure was created to store the new measures which are as follows:

1)  Revenue:
  ```dax
    SUM('sales transactions'[norm_sales_amt])
```

2) Total Quantity:
```dax
 SUM('sales transactions'[sales_qty])
```

3) Total Profit Margin :
```dax
SUM('sales transactions'[profit_margin])
```

4) Profit Margin % :
```dax
 DIVIDE([Total Profit Margin],[Revenue],0)
```

5) Profit Margin Contribution % :
```dax
DIVIDE([Total Profit Margin],CALCULATE([Total Profit Margin],all('sales products'),ALL('sales customers'),all('sales markets')))
```
   
6) Revenue Contribution % :
```dax
 DIVIDE([Revenue],CALCULATE([Revenue],all('sales products'),ALL('sales customers'),all('sales markets')))
```

7) Revenue LY :
```dax
CALCULATE([Revenue],SAMEPERIODLASTYEAR('sales date'[date]))
```


## Insights from the final report

- Atliq mainly does business with two types of customers, those in Brick & mortar category and others in E-Commerce.

- The revenue trend fluctuates for 2017-18 but the major decline in business is observed during year end of 2019 and by the june of 2020.

- Delhi NCR, Mumbai, Ahmedabad remains on top over the span of 4 years in terms of revenue genrated.

- Electricalsara stores remains the top customer to generate largest revenue over the 4 years with over 413 million rupees.

- Bengaluru remains at bottom in terms of revenue, gross profit margin over the years.

- Delhi NCR makes up 50% profit contribution to Atliq's overall profit.

- The revenue contribution from north zone is obvious due the profit contribution percentage by Delhi NCR


## Suggestions to consider for improving the business:

- Engage with high-revenue customers like "Electricalsara stores", "Electricalslytical", "Excel stores" to strengthen customer relationships.

- Leverage the strong profit contribution from Delhi NCR to inform decision-making.

- Gather feedback from customers to identify areas for improvement.

- Examining cost of goods sold at various levels of productions
   

 

  

