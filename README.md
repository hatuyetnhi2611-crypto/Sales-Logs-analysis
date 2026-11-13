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
   **Pricing consistency:** Order Item Product Price and Product Price are identical and used interchangeably to denote the unit price of a product.
   
   **Granularity of quantity:** Order Item Quantity represents the number of units within a specific line item/transaction, not the total quantity of an entire order.
   
   **Gross revenue (Sales):** Represents pre-discount revenue at the line-item level. Sales = Order Item Product Price x Order item quantity.
   
   **Discount mechanics:** Order Item Discount Rate is the percentage discount applied per transaction while Order Item Discount is the total amount discounted from Sales of one transaction. Order Item Discount = Order Item Discount Rate x Sales.
   
   **Net revenue (Order Item Total / Sales per customer):** Both columns are identical and represent post-discount net revenue per transaction: Order Item Total = Sales - Order Item Discount.
   
   **Profitability structure:** Order Item Profit Ratio is the profit margin generated from net revenue. Order Profit Per Order and Benefit per order are identical and represent the amount of profit or loss generated from a line item's net revenue.
   
   **Shipping date interpretation:** shipping date (DateOrders) is treated as the warehouse dispatch date rather than the final customer receipt date. Scheduled shipping days remain fixed per shipping mode regardless of geographic distance, indicating this field functions more as an internal operational benchmark rather than a strict carrier transit promise.
   
   **Risk flag:** Late_delivery_risk is a binary indicator where 1 denotes late delivery and 0 denotes timely delivery.

---
**7. Sales performance analysis**
---
   **7.1 Revenue situation**
   
   **7.1.1 Overall trend**

| order_year | total_order_item_total |
| ---------- | ---------------------- |
| 2015       | 4,857,736.22       |
| 2016       | 4,873,670.71       |
| 2017       | 4,610,967.15       |  
   - Realized revenue experienced a marginal increase of less than 0.5% from 2015 to 2016, followed by a notable contraction of 5.4% the following year.

   **7.1.2 Product range dynamics**

   | order_year | distinct_product_count |
| ---------- | ---------------------- |
| 2015       | 54                     |
| 2016       | 54                     |
| 2017       | 118                    |

   | stt | Product Name                                  | comment           |
| --- | --------------------------------------------- | ----------------- |
| 1   | adidas Brazuca 2014 Official Match Ball       | in 2017           |
| 2   | Adult dog supplies                            | in 2017           |
| 3   | Baby sweater                                  | in 2017           |
| 4   | Bag Boy M330 Push Cart                        | in 2017           |
| 5   | Bowflex SelectTech 1090 Dumbbells             | in 2017           |
| 6   | Brooks Women's Ghost 6 Running Shoe           | in 2017           |
| 7   | Bushnell Pro X7 Jolt Slope Rangefinder        | in 2017           |
| 8   | CDs of rock                                   | in 2017           |
| 9   | Children's heaters                            | in 2017           |
| 10  | Cleveland Golf Collegiate My Custom Wedge 588 | in 2017           |
| 11  | Cleveland Golf Women's 588 RTX CB Satin Chrom | in 2017           |
| 12  | Dell Laptop                                   | in 2017           |
| 13  | Diamondback Boys' Insight 24 Performance Hybr | in 2017           |
| 14  | Diamondback Girls' Clarity 24 Hybrid Bike 201 | in 2017           |
| 15  | DVDs                                          | in 2017           |
| 16  | Elevation Training Mask 2.0                   | in 2017           |
| 17  | Fighting video games                          | in 2017           |
| 18  | First aid kit                                 | in 2017           |
| 19  | Fitbit The One Wireless Activity & Sleep Trac | in 2017           |
| 20  | Garmin Approach S3 Golf GPS Watch             | in 2017           |
| 21  | Garmin Approach S4 Golf GPS Watch             | in 2017           |
| 22  | Garmin Forerunner 910XT GPS Watch             | in 2017           |
| 23  | GolfBuddy VT3 GPS Watch                       | in 2017           |
| 24  | GoPro HERO3+ Black Edition Camera             | in 2017           |
| 25  | Industrial consumer electronics               | in 2017           |
| 26  | insta-bed Neverflat Air Mattress              | in 2017           |
| 27  | Lawn mower                                    | in 2017           |
| 28  | LIJA Women's Argyle Golf Polo                 | in 2017           |
| 29  | LIJA Women's Button Golf Dress                | in 2017           |
| 30  | LIJA Women's Mid-Length Panel Golf Shorts     | in 2017           |
| 31  | MDGolf Pittsburgh Penguins Putter             | in 2017           |
| 32  | Men's gala suit                               | in 2017           |
| 33  | Merrell Men's All Out Flash Trail Running Sho | in 2017           |
| 34  | Merrell Women's Grassbow Sport Hiking Shoe    | in 2017           |
| 35  | Merrell Women's Grassbow Sport Waterproof Hik | in 2017           |
| 36  | Merrell Women's Siren Mid Waterproof Hiking B | in 2017           |
| 37  | Mio ALPHA Heart Rate Monitor/Sport Watch      | in 2017           |
| 38  | Nike Kids' Grade School KD VI Basketball Shoe | in 2017           |
| 39  | Nike Men's Fingertrap Max Training Shoe       | in 2017           |
| 40  | Nike Men's Free TR 5.0 TB Training Shoe       | in 2017           |
| 41  | Nike Men's Kobe IX Elite Low Basketball Shoe  | in 2017           |
| 42  | Nike Women's Free 5.0 TR FIT PRT 4 Training S | in 2017           |
| 43  | Ogio Race Golf Shoes                          | in 2017           |
| 44  | Pelican Maverick 100X Kayak                   | in 2017           |
| 45  | Polar FT4 Heart Rate Monitor                  | in 2017           |
| 46  | Polar Loop Activity Tracker                   | in 2017           |
| 47  | Porcelain crafts                              | in 2017           |
| 48  | Rock music                                    | in 2017           |
| 49  | Smart watch                                   | in 2017           |
| 50  | SOLE E25 Elliptical                           | in 2017           |
| 51  | SOLE E35 Elliptical                           | in 2017           |
| 52  | Sports Books                                  | in 2017           |
| 53  | Stiga Master Series ST3100 Competition Indoor | in 2017           |
| 54  | Summer dresses                                | in 2017           |
| 55  | TaylorMade 2014 Purelite Stand Bag            | in 2017           |
| 56  | TaylorMade White Smoke IN-12 Putter           | in 2017           |
| 57  | TaylorMade Women's RBZ SL Rescue              | in 2017           |
| 58  | The North Face Women's Recon Backpack         | in 2017           |
| 59  | Titleist Club Glove Travel Cover              | in 2017           |
| 60  | Titleist Small Wheeled Travel Cover           | in 2017           |
| 61  | Total Gym 1400                                | in 2017           |
| 62  | Toys                                          | in 2017           |
| 63  | Web Camera                                    | in 2017           |
| 64  | Yakima DoubleDown Ace Hitch Mount 4-Bike Rack | in 2017           |
| 65  | adidas Kids' F5 Messi FG Soccer Cleat         | from 2015 to 2017 |
| 66  | adidas Men's F10 Messi TRX FG Soccer Cleat    | from 2015 to 2017 |
| 67  | adidas Men's Germany Black Crest Away Tee     | from 2015 to 2017 |
| 68  | adidas Youth Germany Black/Red Away Match Soc | from 2015 to 2017 |
| 69  | Bag Boy Beverage Holder                       | from 2015 to 2017 |
| 70  | Bridgestone e6 Straight Distance NFL Carolina | from 2015 to 2017 |
| 71  | Bridgestone e6 Straight Distance NFL San Dieg | from 2015 to 2017 |
| 72  | Bridgestone e6 Straight Distance NFL Tennesse | from 2015 to 2017 |
| 73  | Clicgear 8.0 Shoe Brush                       | from 2015 to 2017 |
| 74  | Clicgear Rovic Cooler Bag                     | from 2015 to 2017 |
| 75  | Columbia Men's PFG Anchor Tough T-Shirt       | from 2015 to 2017 |
| 76  | Diamondback Women's Serene Classic Comfort Bi | from 2015 to 2017 |
| 77  | ENO Atlas Hammock Straps                      | from 2015 to 2017 |
| 78  | Field & Stream Sportsman 16 Gun Fire Safe     | from 2015 to 2017 |
| 79  | Glove It Imperial Golf Towel                  | from 2015 to 2017 |
| 80  | Glove It Urban Brick Golf Towel               | from 2015 to 2017 |
| 81  | Glove It Women's Imperial Golf Glove          | from 2015 to 2017 |
| 82  | Glove It Women's Mod Oval 3-Zip Carry All Gol | from 2015 to 2017 |
| 83  | Glove It Women's Mod Oval Golf Glove          | from 2015 to 2017 |
| 84  | Hirzl Men's Hybrid Golf Glove                 | from 2015 to 2017 |
| 85  | Hirzl Women's Hybrid Golf Glove               | from 2015 to 2017 |
| 86  | Hirzl Women's Soffft Flex Golf Glove          | from 2015 to 2017 |
| 87  | LIJA Women's Eyelet Sleeveless Golf Polo      | from 2015 to 2017 |
| 88  | Nike Dri-FIT Crew Sock 6 Pack                 | from 2015 to 2017 |
| 89  | Nike Men's CJ Elite 2 TD Football Cleat       | from 2015 to 2017 |
| 90  | Nike Men's Comfort 2 Slide                    | from 2015 to 2017 |
| 91  | Nike Men's Deutschland Weltmeister Winners Bl | from 2015 to 2017 |
| 92  | Nike Men's Dri-FIT Victory Golf Polo          | from 2015 to 2017 |
| 93  | Nike Men's Free 5.0+ Running Shoe             | from 2015 to 2017 |
| 94  | Nike Women's Legend V-Neck T-Shirt            | from 2015 to 2017 |
| 95  | Nike Women's Tempo Shorts                     | from 2015 to 2017 |
| 96  | O'Brien Men's Neoprene Life Vest              | from 2015 to 2017 |
| 97  | Pelican Sunstream 100 Kayak                   | from 2015 to 2017 |
| 98  | Perfect Fitness Perfect Rip Deck              | from 2015 to 2017 |
| 99  | Team Golf New England Patriots Putter Grip    | from 2015 to 2017 |
| 100 | Team Golf Pittsburgh Steelers Putter Grip     | from 2015 to 2017 |
| 101 | Team Golf San Francisco Giants Putter Grip    | from 2015 to 2017 |
| 102 | Team Golf St. Louis Cardinals Putter Grip     | from 2015 to 2017 |
| 103 | Team Golf Tennessee Volunteers Putter Grip    | from 2015 to 2017 |
| 104 | Team Golf Texas Longhorns Putter Grip         | from 2015 to 2017 |
| 105 | Titleist Pro V1 High Numbers Personalized Gol | from 2015 to 2017 |
| 106 | Titleist Pro V1x Golf Balls                   | from 2015 to 2017 |
| 107 | Titleist Pro V1x High Numbers Golf Balls      | from 2015 to 2017 |
| 108 | Titleist Pro V1x High Numbers Personalized Go | from 2015 to 2017 |
| 109 | Top Flite Women's 2014 XL Hybrid              | from 2015 to 2017 |
| 110 | TYR Boys' Team Digi Jammer                    | from 2015 to 2017 |
| 111 | Under Armour Girls' Toddler Spine Surge Runni | from 2015 to 2017 |
| 112 | Under Armour Hustle Storm Medium Duffle Bag   | from 2015 to 2017 |
| 113 | Under Armour Kids' Mercenary Slide            | from 2015 to 2017 |
| 114 | Under Armour Men's Compression EV SL Slide    | from 2015 to 2017 |
| 115 | Under Armour Men's Tech II T-Shirt            | from 2015 to 2017 |
| 116 | Under Armour Women's Ignite PIP VI Slide      | from 2015 to 2017 |
| 117 | Under Armour Women's Ignite Slide             | from 2015 to 2017 |
| 118 | Under Armour Women's Micro G Skulpt Running S | from 2015 to 2017 |
   
   - The active product catalog remained static at 54 items between 2015 and 2016, before expanding significantly to 118 products in 2017.

   **7.1.3 Demand shift analysis**

   - Declining demand for legacy products:
     
      + Almost all long-existing products witnessed a significant decrease in 2017. For leading products, their revenue dropped by 20–30%. However, the revenue generated by newly introduced products was substantial enough to offset the majority of this core revenue loss—mitigating the overall net revenue decline to just 5%.
