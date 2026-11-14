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

| Product Name                                  | total_2016 | total_2017 | growth_pct_vs_2016 | pct_of_total_2017 |
| --------------------------------------------- | ---------- | ---------- | ------------------ | ----------------- |
| Field & Stream Sportsman 16 Gun Fire Safe     | 1003937.71 | 736298.08  | -26.66             | 15.9684           |
| Perfect Fitness Perfect Rip Deck              | 639404.24  | 464273.15  | -27.39             | 10.0689           |
| Diamondback Women's Serene Classic Comfort Bi | 575566.96  | 444849.87  | -22.71             | 9.6476            |
| Nike Men's Free 5.0+ Running Shoe             | 520656.64  | 398933.59  | -23.38             | 8.6518            |
| Nike Men's Dri-FIT Victory Golf Polo          | 457595.00  | 336432.00  | -26.48             | 7.2963            |
| Pelican Sunstream 100 Kayak                   | 439243.57  | 334677.40  | -23.81             | 7.2583            |
| Nike Men's CJ Elite 2 TD Football Cleat       | 425289.79  | 305706.48  | -28.12             | 6.6300            |
| O'Brien Men's Neoprene Life Vest              | 416028.29  | 298142.43  | -28.34             | 6.4659            |
| Dell Laptop                                   | 0.00       | 279142.50  | null               | 6.0539            |
| Under Armour Girls' Toddler Spine Surge Runni | 181157.07  | 131413.16  | -27.46             | 2.8500            |
| Web Camera                                    | 0.00       | 103386.08  | null               | 2.2422            |
| Porcelain crafts                              | 0.00       | 91472.37   | null               | 1.9838            |
| Children's heaters                            | 0.00       | 88935.98   | null               | 1.9288            |
| Lawn mower                                    | 0.00       | 79018.86   | null               | 1.7137            |
| Industrial consumer electronics               | 0.00       | 45941.82   | null               | 0.9964            |
| DVDs                                          | 0.00       | 32446.91   | null               | 0.7037            |
| First aid kit                                 | 0.00       | 29920.85   | null               | 0.6489            |
| Rock music                                    | 0.00       | 28727.48   | null               | 0.6230            |
| Summer dresses                                | 0.00       | 21299.34   | null               | 0.4619            |
| SOLE E35 Elliptical                           | 0.00       | 19449.89   | null               | 0.4218            |
| Smart watch                                   | 0.00       | 18807.98   | null               | 0.4079            |
| Men's gala suit                               | 0.00       | 14157.55   | null               | 0.3070            |
| LIJA Women's Mid-Length Panel Golf Shorts     | 0.00       | 9379.00    | null               | 0.2034            |
| Adult dog supplies                            | 0.00       | 9020.68    | null               | 0.1956            |
| TaylorMade White Smoke IN-12 Putter           | 0.00       | 8940.58    | null               | 0.1939            |
| Elevation Training Mask 2.0                   | 0.00       | 8406.54    | null               | 0.1823            |
| Nike Women's Free 5.0 TR FIT PRT 4 Training S | 0.00       | 7701.32    | null               | 0.1670            |
| Brooks Women's Ghost 6 Running Shoe           | 0.00       | 7250.50    | null               | 0.1572            |
| Nike Men's Free TR 5.0 TB Training Shoe       | 0.00       | 7114.75    | null               | 0.1543            |
| Garmin Forerunner 910XT GPS Watch             | 0.00       | 7069.80    | null               | 0.1533            |
| Polar FT4 Heart Rate Monitor                  | 0.00       | 7069.14    | null               | 0.1533            |
| Garmin Approach S3 Golf GPS Watch             | 0.00       | 6745.95    | null               | 0.1463            |
| GoPro HERO3+ Black Edition Camera             | 0.00       | 6565.82    | null               | 0.1424            |
| The North Face Women's Recon Backpack         | 0.00       | 6482.03    | null               | 0.1406            |
| LIJA Women's Argyle Golf Polo                 | 0.00       | 6119.60    | null               | 0.1327            |
| Nike Kids' Grade School KD VI Basketball Shoe | 0.00       | 6114.88    | null               | 0.1326            |
| Merrell Women's Grassbow Sport Hiking Shoe    | 0.00       | 6081.37    | null               | 0.1319            |
| Fitbit The One Wireless Activity & Sleep Trac | 0.00       | 5736.15    | null               | 0.1244            |
| Bag Boy M330 Push Cart                        | 0.00       | 5409.69    | null               | 0.1173            |
| Mio ALPHA Heart Rate Monitor/Sport Watch      | 0.00       | 5368.03    | null               | 0.1164            |
| TaylorMade 2014 Purelite Stand Bag            | 0.00       | 4862.40    | null               | 0.1055            |
| Sports Books                                  | 0.00       | 4736.86    | null               | 0.1027            |
| Garmin Approach S4 Golf GPS Watch             | 0.00       | 4693.33    | null               | 0.1018            |
| Pelican Maverick 100X Kayak                   | 0.00       | 4625.10    | null               | 0.1003            |
| MDGolf Pittsburgh Penguins Putter             | 0.00       | 4573.82    | null               | 0.0992            |
| Titleist Small Wheeled Travel Cover           | 0.00       | 4532.30    | null               | 0.0983            |
| Yakima DoubleDown Ace Hitch Mount 4-Bike Rack | 0.00       | 4528.45    | null               | 0.0982            |
| Baby sweater                                  | 0.00       | 4329.44    | null               | 0.0939            |
| Total Gym 1400                                | 0.00       | 4309.34    | null               | 0.0935            |
| Titleist Club Glove Travel Cover              | 0.00       | 4253.84    | null               | 0.0923            |
| adidas Brazuca 2014 Official Match Ball       | 0.00       | 4013.32    | null               | 0.0870            |
| GolfBuddy VT3 GPS Watch                       | 0.00       | 3962.78    | null               | 0.0859            |
| adidas Youth Germany Black/Red Away Match Soc | 11140.50   | 3924.55    | -64.77             | 0.0851            |
| Ogio Race Golf Shoes                          | 0.00       | 3906.34    | null               | 0.0847            |
| Merrell Women's Siren Mid Waterproof Hiking B | 0.00       | 3674.41    | null               | 0.0797            |
| Cleveland Golf Collegiate My Custom Wedge 588 | 0.00       | 3655.90    | null               | 0.0793            |
| Merrell Men's All Out Flash Trail Running Sho | 0.00       | 3635.68    | null               | 0.0788            |
| LIJA Women's Button Golf Dress                | 0.00       | 3510.54    | null               | 0.0761            |
| Fighting video games                          | 0.00       | 3497.43    | null               | 0.0759            |
| Nike Men's Kobe IX Elite Low Basketball Shoe  | 0.00       | 3388.81    | null               | 0.0735            |
| LIJA Women's Eyelet Sleeveless Golf Polo      | 9438.66    | 3382.93    | -64.16             | 0.0734            |
| TaylorMade Women's RBZ SL Rescue              | 0.00       | 3359.56    | null               | 0.0729            |
| Merrell Women's Grassbow Sport Waterproof Hik | 0.00       | 3180.18    | null               | 0.0690            |
| Bowflex SelectTech 1090 Dumbbells             | 0.00       | 3131.94    | null               | 0.0679            |
| Cleveland Golf Women's 588 RTX CB Satin Chrom | 0.00       | 3120.91    | null               | 0.0677            |
| Titleist Pro V1x High Numbers Personalized Go | 7715.81    | 3034.64    | -60.67             | 0.0658            |
| Titleist Pro V1x Golf Balls                   | 6987.08    | 3031.26    | -56.62             | 0.0657            |
| adidas Men's F10 Messi TRX FG Soccer Cleat    | 9852.73    | 2935.29    | -70.21             | 0.0637            |
| Diamondback Boys' Insight 24 Performance Hybr | 0.00       | 2920.39    | null               | 0.0633            |
| SOLE E25 Elliptical                           | 0.00       | 2869.97    | null               | 0.0622            |
| Nike Men's Fingertrap Max Training Shoe       | 0.00       | 2790.39    | null               | 0.0605            |
| Diamondback Girls' Clarity 24 Hybrid Bike 201 | 0.00       | 2738.90    | null               | 0.0594            |
| Polar Loop Activity Tracker                   | 0.00       | 2732.25    | null               | 0.0593            |
| Nike Men's Comfort 2 Slide                    | 7107.25    | 2482.54    | -65.07             | 0.0538            |
| insta-bed Neverflat Air Mattress              | 0.00       | 2475.56    | null               | 0.0537            |
| Titleist Pro V1x High Numbers Golf Balls      | 5988.88    | 2447.72    | -59.13             | 0.0531            |
| Titleist Pro V1 High Numbers Personalized Gol | 6967.39    | 2275.59    | -67.34             | 0.0494            |
| Under Armour Women's Micro G Skulpt Running S | 8347.18    | 2205.41    | -73.58             | 0.0478            |
| Stiga Master Series ST3100 Competition Indoor | 0.00       | 2130.08    | null               | 0.0462            |
| Under Armour Men's Compression EV SL Slide    | 5208.47    | 1915.22    | -63.23             | 0.0415            |
| Nike Women's Tempo Shorts                     | 3770.70    | 1895.40    | -49.73             | 0.0411            |
| adidas Kids' F5 Messi FG Soccer Cleat         | 5545.53    | 1856.38    | -66.52             | 0.0403            |
| Bridgestone e6 Straight Distance NFL San Dieg | 4927.69    | 1767.58    | -64.13             | 0.0383            |
| Columbia Men's PFG Anchor Tough T-Shirt       | 5466.30    | 1757.10    | -67.86             | 0.0381            |
| Under Armour Hustle Storm Medium Duffle Bag   | 4494.27    | 1697.35    | -62.23             | 0.0368            |
| Bushnell Pro X7 Jolt Slope Rangefinder        | 0.00       | 1682.97    | null               | 0.0365            |
| ENO Atlas Hammock Straps                      | 5604.95    | 1555.13    | -72.25             | 0.0337            |
| Clicgear Rovic Cooler Bag                     | 5352.02    | 1535.41    | -71.31             | 0.0333            |
| TYR Boys' Team Digi Jammer                    | 5176.07    | 1472.42    | -71.55             | 0.0319            |
| Under Armour Men's Tech II T-Shirt            | 3784.31    | 1433.03    | -62.13             | 0.0311            |
| Team Golf San Francisco Giants Putter Grip    | 3413.46    | 1409.30    | -58.71             | 0.0306            |
| Glove It Women's Imperial Golf Glove          | 2327.41    | 1402.69    | -39.73             | 0.0304            |
| Bridgestone e6 Straight Distance NFL Tennesse | 4342.59    | 1382.77    | -68.16             | 0.0300            |
| Team Golf New England Patriots Putter Grip    | 4256.67    | 1335.56    | -68.62             | 0.0290            |
| CDs of rock                                   | 0.00       | 1267.72    | null               | 0.0275            |
| Nike Men's Deutschland Weltmeister Winners Bl | 6209.70    | 1249.05    | -79.89             | 0.0271            |
| Under Armour Kids' Mercenary Slide            | 3090.77    | 1181.45    | -61.77             | 0.0256            |
| Toys                                          | 0.00       | 1165.44    | null               | 0.0253            |
| Bag Boy Beverage Holder                       | 3675.13    | 1073.93    | -70.78             | 0.0233            |
| Team Golf Texas Longhorns Putter Grip         | 2484.75    | 1069.31    | -56.97             | 0.0232            |
| Under Armour Women's Ignite Slide             | 4684.24    | 1045.74    | -77.68             | 0.0227            |
| Nike Dri-FIT Crew Sock 6 Pack                 | 2849.77    | 1004.74    | -64.74             | 0.0218            |
| Glove It Women's Mod Oval 3-Zip Carry All Gol | 3616.45    | 982.30     | -72.84             | 0.0213            |
| Under Armour Women's Ignite PIP VI Slide      | 5011.02    | 925.61     | -81.53             | 0.0201            |
| Top Flite Women's 2014 XL Hybrid              | 3496.30    | 885.33     | -74.68             | 0.0192            |
| Team Golf Tennessee Volunteers Putter Grip    | 4518.02    | 876.52     | -80.60             | 0.0190            |
| Glove It Urban Brick Golf Towel               | 2414.87    | 861.62     | -64.32             | 0.0187            |
| Team Golf St. Louis Cardinals Putter Grip     | 4568.64    | 782.93     | -82.86             | 0.0170            |
| Glove It Women's Mod Oval Golf Glove          | 2614.87    | 780.30     | -70.16             | 0.0169            |
| Hirzl Women's Soffft Flex Golf Glove          | 2620.58    | 779.58     | -70.25             | 0.0169            |
| Bridgestone e6 Straight Distance NFL Carolina | 5562.40    | 726.00     | -86.95             | 0.0157            |
| Team Golf Pittsburgh Steelers Putter Grip     | 3825.95    | 718.95     | -81.21             | 0.0156            |
| adidas Men's Germany Black Crest Away Tee     | 3174.01    | 698.50     | -77.99             | 0.0151            |
| Hirzl Men's Hybrid Golf Glove                 | 2544.66    | 682.86     | -73.16             | 0.0148            |
| Nike Women's Legend V-Neck T-Shirt            | 3798.64    | 633.00     | -83.34             | 0.0137            |
| Hirzl Women's Hybrid Golf Glove               | 2416.06    | 620.87     | -74.30             | 0.0135            |
| Glove It Imperial Golf Towel                  | 3133.50    | 606.80     | -80.64             | 0.0132            |
| Clicgear 8.0 Shoe Brush                       | 1265.20    | 380.55     | -69.92             | 0.0083            |

| legacy_revenue_diff | new_products_total_2017 | legacy_diff_pct_of_total_2016 | new_products_pct_of_total_2016 |
| ------------------- | ----------------------- | ----------------------------- | ------------------------------ |
| -1356243.35         | 1093539.79              | -27.83                        | 22.44                          |

Almost all long-existing products witnessed a significant decrease in revenue from 2016 to 2017. For leading products, their revenue dropped by 20–30%. However, the revenue generated by newly introduced products (1,093,539.79) was substantial enough to offset the majority of this core revenue loss (1,356,243.35)—mitigating the overall net revenue decline to just 5%.
