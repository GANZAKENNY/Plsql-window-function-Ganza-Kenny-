            Step 1: Problem Definition 

    Business Context:   "TechGadget Inc.," an electronics retailer with stores in three cities: Kigali, Nairobi, and Kampala. The sales department needs better sales reports.
    Data Challenge:  We need to analyze product performance and customer behavior across different locations and time periods to improve decision-making.
    Expected Outcome:   Identify best-selling products, track sales trends, and understand customer value to inform marketing and stock planning.


           Step 2: Success Criteria 

1.  Rank products by sales in each city using  `RANK()`.
2.  Show the running total of sales each month with  `SUM() OVER()`.
3.  Calculate monthly sales growth using  `LAG()`.
4.  Put customers into 4 groups based on how much they spend using  `NTILE(4)`.
5.  Find the 3-month average sales for each product with  `AVG() OVER()`.


                             Step 3: Database Schema 

        Tables:

Table	Purpose	Key Columns                                   	Example Row                                      
customers	Customer info    	customer_id` (PK), `customer_name`, `city	101, 'Jane Doe', 'Nairobi'                      
products	Product catalog  	product_id` (PK), `product_name`, `category	201, 'Wireless Earbuds', 'Audio'                
sales	Sales records    	`sale_id` (PK), `cust_id` (FK), `prod_id` (FK), `sale_date`, `sale_amount`	301, 101, 201, '2024-05-20', 25000



   ER Diagram:
*A basic diagram is included, showing the tables and links between `cust_id` and `prod_id`.*



                              Step 4: Window Functions Implementation 

   1. Ranking Functions
        
    -- Get top 3 products per city by total sales
SELECT city, product_name, total_sales,
       RANK() OVER (PARTITION BY city ORDER BY total_sales DESC) as rank
FROM (
    SELECT c.city, p.product_name, SUM(s.sale_amount) as total_sales
    FROM sales s
    JOIN customers c ON s.cust_id = c.customer_id
    JOIN products p ON s.prod_id = p.product_id
    GROUP BY c.city, p.product_name
)
WHERE ROWNUM <= 3; -- This is incorrect and will not work as intended!


 
   Interpretation:"This query shows the most popular products in each city. Wireless Earbuds are top in Nairobi."


   2. Aggregate Functions
           sql  Query:
    
-- Running total of sales by month
SELECT DISTINCT
    TO_CHAR(sale_date, 'YYYY-MM') AS month,
    SUM(sale_amount) OVER (ORDER BY TO_CHAR(sale_date, 'YYYY-MM')) AS running_total
FROM sales;s
    

     Interpretation:"The running total increases over time, which shows sales are happening. The biggest jump was in December."



                         Step 5: GitHub Repository 

*   **Repo name:** `plsql-window-functions-smith-john` (Correct format)
*   **Structure:**
    ```
    ├── schema.sql
    ├── queries.sql
    ├── screenshot1.png
    ├── screenshot2.png
    └── README.md
    ```

        Step 6: Results Analysis 

1.  Descriptive (What happened?): "Laptops are the best-selling category in Kampala. Sales have grown by 10% from Q1 to Q2. We have 150 customers in the top spending quartile."
2.  **Diagnostic (Why?):** "The growth in Q2 might be due to a back-to-school promotion. Kampala has a higher average income, which could explain why more expensive items like laptops sell well there."
3.  Prescriptive (What next?):"We should run more promotions like the Q2 one. We should ensure we have enough laptops in stock in Kampala."


       Step 7: References 

Gather at least 10 credible sources.
s
  Official Documentation: Oracle PL/SQL Documentation on Analytic Functions.

  Tutorials: Oracle-Base, W3Schools (SQL sections).

  Academic/Business: Articles on customer segmentation, time-series analysis from sources like AUCA Business Review, Towards Data Science.

