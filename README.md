# Sales-Logs-analysis
---
**1. Project Overview**
---
   The project includes analytics about two important aspects of a retail business which are sales and logistics. While sales performance is studied based on realized revenue and profit, the logistics competency of the company is evaluated mainly based on dispatch date. The analysis of both aspects starts with overall trend and data aggregation to generalize the health of the two. This is then followed by a more in-depth study where some possible causes and scenarios behind problems and trends found earlier are brainstormed, some tests will be run to support the assumptions made, and those risks or red flags identified along the way will be highlighted.
   
**Analytical flow:**

General picture of sales and logistics performance ➔ Assumptions made ➔ Tests run to prove the possibility of assumptions made ➔ Conclusion from the tests and red flags raised if any

---
**2. Objectives of The Project**
---
   - To find out the trend of revenue over the period of 3 years from 2015 to 2017
   - To identify changes in revenue breakdown by product to explain changes in overal revenue trends
   - To find out possible reasons behind decreased demand
   - To identify risks with the existing revenue distribution using pareto principle
   - To identify the type of customers contributing greatly to the business
   - To find out possible reasons behind the revenue share of each market
   - To find out possible reasons behind the increased profit and identify products with largest margin
   - To run tests to find out possible reasons behind the significant loss and identify areas where loss can be alleviated
   - To run tests to verify if factors such as customers' state, products, and departments are relevant with the delay in shipment date
---
**3. Dataset Overview**
---
   The dataset records all the transactions of a retail firm from January 2015 to January 2018 with 180,519 rows and 53 columns. Each row corresponding with one transaction covers info such as revenue, discount amount, profit & loss, payment type, delivery status, order status, customer details, product name & price, shipment date, expected and actual shipping window, shipping mode, and destination of the products.
   
**Source:** [DataCo SMART SUPPLY CHAIN FOR BIG DATA ANALYSIS](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)

---
**4. Data Cleaning**
---
   Action taken:
   - Find missing values
   - Find duplicate rows
   - Logic validation.
     
   Results obtained:
   - 8 rows lacking customers' last name, 3 rows lacking customers' zip code, 155,679 rows lacking orders' zip code and all the rows lacking product description ➔ no data curation is required because these fields are not necessary during the data analysis
   - No duplicate rows were found.
   - No shipment dates are before order dates and the relationship between actual and scheduled shipping days strictly aligns with the assigned delivery status:
        + Late delivery: Actual days > Scheduled days
        + Shipping on time: Actual days = Scheduled days
        + Advance shipping: Actual days < Scheduled days
---
**5. Scope of data used**
---
   **Timeframe:** Restricted to 2015–2017 because transactions for these years are fully recorded. Data for 2018 only covers January, which would introduce seasonal noise and skew time-series trend analysis.
   **Order Status Selection:**
      - For Realized Revenue & Profit Analysis: Filtered to include only Complete and Closed orders, ensuring transactions have completed their full lifecycle and revenue was genuinely collected.
      - For Logistics Analysis: Included all order statuses except Cancelled. Delivery performance is evaluated based on warehouse dispatch timing rather than final delivery completion.
   **Key Fields Utilized:** Days for shipping (real), Days for shipment (scheduled), Benefit per order, Sales per customer, Delivery Status, Late_delivery_risk, Customer Id, Customer Segment, Customer State, Department Name, Market, order date (DateOrders), Order Item Discount, Order Item Discount Rate, Order Item Id, Order Item Product Price, Order Item Profit Ratio, Order Item Quantity, Sales, Order Item Total, Order Profit Per Order, Order Status, Product Price, shipping date (DateOrders), Shipping Mode, Product Name
   
---
**6. Data Understanding & Column Logic**
---
   **Pricing Consistency:** Order Item Product Price and Product Price are identical and used interchangeably to denote the unit price of a product.
   **Granularity of Quantity:** Order Item Quantity represents the number of units within a specific line item/transaction, not the total quantity of an entire order.
   **Gross Revenue (Sales):** Represents pre-discount revenue at the line-item level. Sales = Order Item Product Price x Order item quantity
   **Discount Mechanics:** Order Item Discount Rate is the percentage discount applied per transaction while Order Item Discount is the total amount discounted from Sales of one transaction. Order Item Discount = Order Item Discount Rate x Sales
   **Net Revenue (Order Item Total / Sales per customer):** Both columns are identical and represent post-discount net revenue per transaction: Order Item Total = Sales - Order Item Discount
   **Profitability Structure:** Order Item Profit Ratio is the profit margin generated from net revenue. Order Profit Per Order and Benefit per order are identical and represent the amount of profit or loss generated from a line item's net revenue.
   **Shipping Date Interpretation:** shipping date (DateOrders) is treated as the warehouse dispatch date rather than the final customer receipt date. Scheduled shipping days remain fixed per shipping mode regardless of geographic distance, indicating this field functions more as an internal operational benchmark rather than a strict carrier transit promise.
   **Risk Flag:** Late_delivery_risk is a binary indicator where 1 denotes late delivery and 0 denotes timely delivery.
