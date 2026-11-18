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

   - Investigation of the cause behind decreased demand of long-existing products.

| qty_2016 | qty_2017 | qty_growth_pct |
| -------- | -------- | -------------- |
| 60694    | 41264    | -32.01         |

Given that unit price remained unchanged throughout the given period, a decline of 32.01% in revenue from 2016 to 2017 was caused by the reduction in the amount purchased of all 54 long-existing products.

| Product Name                                  | qty_2016 | qty_2017 | qty_difference | qty_growth_pct |
| --------------------------------------------- | -------- | -------- | -------------- | -------------- |
| Perfect Fitness Perfect Rip Deck              | 11873    | 8604     | -3269          | -27.53         |
| Nike Men's Dri-FIT Victory Golf Polo          | 10195    | 7479     | -2716          | -26.64         |
| O'Brien Men's Neoprene Life Vest              | 9245     | 6627     | -2618          | -28.32         |
| Under Armour Girls' Toddler Spine Surge Runni | 5050     | 3661     | -1389          | -27.50         |
| Nike Men's Free 5.0+ Running Shoe             | 5803     | 4456     | -1347          | -23.21         |
| Nike Men's CJ Elite 2 TD Football Cleat       | 3641     | 2622     | -1019          | -27.99         |
| Field & Stream Sportsman 16 Gun Fire Safe     | 2795     | 2047     | -748           | -26.76         |
| Pelican Sunstream 100 Kayak                   | 2444     | 1861     | -583           | -23.85         |
| Diamondback Women's Serene Classic Comfort Bi | 2141     | 1649     | -492           | -22.98         |
| Nike Men's Deutschland Weltmeister Winners Bl | 230      | 45       | -185           | -80.43         |
| Glove It Imperial Golf Towel                  | 216      | 42       | -174           | -80.56         |
| Team Golf St. Louis Cardinals Putter Grip     | 205      | 34       | -171           | -83.41         |
| Bridgestone e6 Straight Distance NFL Carolina | 192      | 26       | -166           | -86.46         |
| Team Golf Tennessee Volunteers Putter Grip    | 201      | 40       | -161           | -80.10         |
| ENO Atlas Hammock Straps                      | 209      | 57       | -152           | -72.73         |
| Top Flite Women's 2014 XL Hybrid              | 195      | 49       | -146           | -74.87         |
| Under Armour Women's Ignite PIP VI Slide      | 174      | 33       | -141           | -81.03         |
| Team Golf Pittsburgh Steelers Putter Grip     | 172      | 31       | -141           | -81.98         |
| Nike Women's Legend V-Neck T-Shirt            | 166      | 28       | -138           | -83.13         |
| Hirzl Men's Hybrid Golf Glove                 | 188      | 50       | -138           | -73.40         |
| Glove It Women's Mod Oval 3-Zip Carry All Gol | 185      | 50       | -135           | -72.97         |
| Columbia Men's PFG Anchor Tough T-Shirt       | 199      | 65       | -134           | -67.34         |
| adidas Men's F10 Messi TRX FG Soccer Cleat    | 186      | 53       | -133           | -71.51         |
| Hirzl Women's Hybrid Golf Glove               | 179      | 47       | -132           | -73.74         |
| Team Golf New England Patriots Putter Grip    | 192      | 61       | -131           | -68.23         |
| Under Armour Women's Ignite Slide             | 163      | 37       | -126           | -77.30         |
| Under Armour Women's Micro G Skulpt Running S | 170      | 45       | -125           | -73.53         |
| adidas Youth Germany Black/Red Away Match Soc | 179      | 60       | -119           | -66.48         |
| Bag Boy Beverage Holder                       | 164      | 47       | -117           | -71.34         |
| adidas Kids' F5 Messi FG Soccer Cleat         | 175      | 59       | -116           | -66.29         |
| Hirzl Women's Soffft Flex Golf Glove          | 163      | 47       | -116           | -71.17         |
| Nike Men's Comfort 2 Slide                    | 177      | 62       | -115           | -64.97         |
| Bridgestone e6 Straight Distance NFL San Dieg | 172      | 61       | -111           | -64.53         |
| adidas Men's Germany Black Crest Away Tee     | 141      | 31       | -110           | -78.01         |
| Glove It Urban Brick Golf Towel               | 169      | 62       | -107           | -63.31         |
| Clicgear Rovic Cooler Bag                     | 149      | 43       | -106           | -71.14         |
| Under Armour Men's Tech II T-Shirt            | 167      | 62       | -105           | -62.87         |
| Glove It Women's Mod Oval Golf Glove          | 147      | 43       | -104           | -70.75         |
| Bridgestone e6 Straight Distance NFL Tennesse | 151      | 48       | -103           | -68.21         |
| TYR Boys' Team Digi Jammer                    | 143      | 40       | -103           | -72.03         |
| LIJA Women's Eyelet Sleeveless Golf Polo      | 161      | 59       | -102           | -63.35         |
| Titleist Pro V1x High Numbers Personalized Go | 165      | 64       | -101           | -61.21         |
| Titleist Pro V1 High Numbers Personalized Gol | 149      | 51       | -98            | -65.77         |
| Clicgear 8.0 Shoe Brush                       | 140      | 44       | -96            | -68.57         |
| Nike Dri-FIT Crew Sock 6 Pack                 | 143      | 48       | -95            | -66.43         |
| Titleist Pro V1x Golf Balls                   | 161      | 69       | -92            | -57.14         |
| Under Armour Hustle Storm Medium Duffle Bag   | 143      | 53       | -90            | -62.94         |
| Titleist Pro V1x High Numbers Golf Balls      | 140      | 57       | -83            | -59.29         |
| Team Golf San Francisco Giants Putter Grip    | 148      | 65       | -83            | -56.08         |
| Under Armour Men's Compression EV SL Slide    | 129      | 49       | -80            | -62.02         |
| Under Armour Kids' Mercenary Slide            | 124      | 46       | -78            | -62.90         |
| Nike Women's Tempo Shorts                     | 143      | 69       | -74            | -51.75         |
| Team Golf Texas Longhorns Putter Grip         | 113      | 48       | -65            | -57.52         |
| Glove It Women's Imperial Golf Glove          | 129      | 78       | -51            | -39.53         |

The percentage of quantity decline of those items with highest revenue ranged from 22.98% to 28.32%

| distinct_cust_2016 | distinct_cust_2017 | qty_per_customer_2016 | qty_per_customer_2017 |
| ------------------ | ------------------ | --------------------- | --------------------- |
| 6599               | 5219               | 9.20                  | 7.91                  |

Both quantity per customer and the number of customers decreased from 2016 to 2017. While the average quantity bought by one customer went down by 14%, the number of customers saw a decrease of nearly 21%. This shows that the decline in the number of customers accounts for a larger share of the fall in total quantity than the decline in quantity per customer. 

| new_cust_2016 | returning_cust_2016 | new_cust_2017 | returning_cust_2017 |
| ------------- | ------------------- | ------------- | ------------------- |
| 3136          | 3463                | 1181          | 4038                |

The number of active customers fell from 6,599 in 2016 to 5,219 in 2017. New customers fell by 62% (3,136 to 1,181) while returning customers rose by 17% (3,463 to 4,038), so the decline comes entirely from fewer new customers. “New” means customers with no order in any earlier year. This suggests that the company should look into ways to make the long-existing products reach a wider range of customers so their  sold volume & revenue can be improved.

   - High revenue concentration

| Product Name                                  | total_2015 | total_2016 | total_2017 | cum_pct_2015 | cum_pct_2016 | cum_pct_2017 |
| --------------------------------------------- | ---------- | ---------- | ---------- | ------------ | ------------ | ------------ |
| Field & Stream Sportsman 16 Gun Fire Safe     | 974313.23  | 1003937.71 | 736298.08  | 20.06        | 20.60        | 15.97        |
| Perfect Fitness Perfect Rip Deck              | 630928.92  | 639404.24  | 464273.15  | 33.05        | 33.72        | 26.04        |
| Diamondback Women's Serene Classic Comfort Bi | 596965.90  | 575566.96  | 444849.87  | 45.33        | 45.53        | 35.68        |
| Nike Men's Free 5.0+ Running Shoe             | 540546.58  | 520656.64  | 398933.59  | 56.46        | 56.21        | 44.34        |
| Nike Men's Dri-FIT Victory Golf Polo          | 452596.50  | 457595.00  | 336432.00  | 65.78        | 65.60        | 51.63        |
| Pelican Sunstream 100 Kayak                   | 428127.18  | 439243.57  | 334677.40  | 74.59        | 74.61        | 58.89        |
| Nike Men's CJ Elite 2 TD Football Cleat       | 417866.14  | 425289.79  | 305706.48  | 83.19        | 83.34        | 65.52        |
| O'Brien Men's Neoprene Life Vest              | 417287.63  | 416028.29  | 298142.43  | 91.78        | 91.88        | 71.99        |
| Dell Laptop                                   | 0.00       | 0.00       | 279142.50  | 100.00       | 100.00       | 78.04        |
| Under Armour Girls' Toddler Spine Surge Runni | 182708.85  | 181157.07  | 131413.16  | 95.55        | 95.59        | 80.89        |
| Web Camera                                    | 0.00       | 0.00       | 103386.08  | 100.00       | 100.00       | 83.13        |
| Porcelain crafts                              | 0.00       | 0.00       | 91472.37   | 100.00       | 100.00       | 85.12        |
| Children's heaters                            | 0.00       | 0.00       | 88935.98   | 100.00       | 100.00       | 87.05        |
| Lawn mower                                    | 0.00       | 0.00       | 79018.86   | 100.00       | 100.00       | 88.76        |
| Industrial consumer electronics               | 0.00       | 0.00       | 45941.82   | 100.00       | 100.00       | 89.76        |
| DVDs                                          | 0.00       | 0.00       | 32446.91   | 100.00       | 100.00       | 90.46        |
| First aid kit                                 | 0.00       | 0.00       | 29920.85   | 100.00       | 100.00       | 91.11        |
| Rock music                                    | 0.00       | 0.00       | 28727.48   | 100.00       | 100.00       | 91.73        |
| Summer dresses                                | 0.00       | 0.00       | 21299.34   | 100.00       | 100.00       | 92.19        |
| SOLE E35 Elliptical                           | 0.00       | 0.00       | 19449.89   | 100.00       | 100.00       | 92.62        |
| Smart watch                                   | 0.00       | 0.00       | 18807.98   | 100.00       | 100.00       | 93.02        |
| Men's gala suit                               | 0.00       | 0.00       | 14157.55   | 100.00       | 100.00       | 93.33        |
| LIJA Women's Mid-Length Panel Golf Shorts     | 0.00       | 0.00       | 9379.00    | 100.00       | 100.00       | 93.53        |
| Adult dog supplies                            | 0.00       | 0.00       | 9020.68    | 100.00       | 100.00       | 93.73        |
| TaylorMade White Smoke IN-12 Putter           | 0.00       | 0.00       | 8940.58    | 100.00       | 100.00       | 93.92        |
| Elevation Training Mask 2.0                   | 0.00       | 0.00       | 8406.54    | 100.00       | 100.00       | 94.11        |
| Nike Women's Free 5.0 TR FIT PRT 4 Training S | 0.00       | 0.00       | 7701.32    | 100.00       | 100.00       | 94.27        |
| Brooks Women's Ghost 6 Running Shoe           | 0.00       | 0.00       | 7250.50    | 100.00       | 100.00       | 94.43        |
| Nike Men's Free TR 5.0 TB Training Shoe       | 0.00       | 0.00       | 7114.75    | 100.00       | 100.00       | 94.58        |
| Garmin Forerunner 910XT GPS Watch             | 0.00       | 0.00       | 7069.80    | 100.00       | 100.00       | 94.74        |
| Polar FT4 Heart Rate Monitor                  | 0.00       | 0.00       | 7069.14    | 100.00       | 100.00       | 94.89        |
| Garmin Approach S3 Golf GPS Watch             | 0.00       | 0.00       | 6745.95    | 100.00       | 100.00       | 95.04        |
| GoPro HERO3+ Black Edition Camera             | 0.00       | 0.00       | 6565.82    | 100.00       | 100.00       | 95.18        |
| The North Face Women's Recon Backpack         | 0.00       | 0.00       | 6482.03    | 100.00       | 100.00       | 95.32        |
| LIJA Women's Argyle Golf Polo                 | 0.00       | 0.00       | 6119.60    | 100.00       | 100.00       | 95.45        |
| Nike Kids' Grade School KD VI Basketball Shoe | 0.00       | 0.00       | 6114.88    | 100.00       | 100.00       | 95.59        |
| Merrell Women's Grassbow Sport Hiking Shoe    | 0.00       | 0.00       | 6081.37    | 100.00       | 100.00       | 95.72        |
| Fitbit The One Wireless Activity & Sleep Trac | 0.00       | 0.00       | 5736.15    | 100.00       | 100.00       | 95.84        |
| Bag Boy M330 Push Cart                        | 0.00       | 0.00       | 5409.69    | 100.00       | 100.00       | 95.96        |
| Mio ALPHA Heart Rate Monitor/Sport Watch      | 0.00       | 0.00       | 5368.03    | 100.00       | 100.00       | 96.08        |
| TaylorMade 2014 Purelite Stand Bag            | 0.00       | 0.00       | 4862.40    | 100.00       | 100.00       | 96.18        |
| Sports Books                                  | 0.00       | 0.00       | 4736.86    | 100.00       | 100.00       | 96.28        |
| Garmin Approach S4 Golf GPS Watch             | 0.00       | 0.00       | 4693.33    | 100.00       | 100.00       | 96.39        |
| Pelican Maverick 100X Kayak                   | 0.00       | 0.00       | 4625.10    | 100.00       | 100.00       | 96.49        |
| MDGolf Pittsburgh Penguins Putter             | 0.00       | 0.00       | 4573.82    | 100.00       | 100.00       | 96.58        |
| Titleist Small Wheeled Travel Cover           | 0.00       | 0.00       | 4532.30    | 100.00       | 100.00       | 96.68        |
| Yakima DoubleDown Ace Hitch Mount 4-Bike Rack | 0.00       | 0.00       | 4528.45    | 100.00       | 100.00       | 96.78        |
| Baby sweater                                  | 0.00       | 0.00       | 4329.44    | 100.00       | 100.00       | 96.88        |
| Total Gym 1400                                | 0.00       | 0.00       | 4309.34    | 100.00       | 100.00       | 96.97        |
| Titleist Club Glove Travel Cover              | 0.00       | 0.00       | 4253.84    | 100.00       | 100.00       | 97.06        |
| adidas Brazuca 2014 Official Match Ball       | 0.00       | 0.00       | 4013.32    | 100.00       | 100.00       | 97.15        |
| GolfBuddy VT3 GPS Watch                       | 0.00       | 0.00       | 3962.78    | 100.00       | 100.00       | 97.23        |
| adidas Youth Germany Black/Red Away Match Soc | 9654.75    | 11140.50   | 3924.55    | 95.99        | 95.82        | 97.32        |
| Ogio Race Golf Shoes                          | 0.00       | 0.00       | 3906.34    | 100.00       | 100.00       | 97.40        |
| Merrell Women's Siren Mid Waterproof Hiking B | 0.00       | 0.00       | 3674.41    | 100.00       | 100.00       | 97.48        |
| Cleveland Golf Collegiate My Custom Wedge 588 | 0.00       | 0.00       | 3655.90    | 100.00       | 100.00       | 97.56        |
| Merrell Men's All Out Flash Trail Running Sho | 0.00       | 0.00       | 3635.68    | 100.00       | 100.00       | 97.64        |
| LIJA Women's Button Golf Dress                | 0.00       | 0.00       | 3510.54    | 100.00       | 100.00       | 97.72        |
| Fighting video games                          | 0.00       | 0.00       | 3497.43    | 100.00       | 100.00       | 97.79        |
| Nike Men's Kobe IX Elite Low Basketball Shoe  | 0.00       | 0.00       | 3388.81    | 100.00       | 100.00       | 97.87        |
| LIJA Women's Eyelet Sleeveless Golf Polo      | 12149.15   | 9438.66    | 3382.93    | 95.80        | 96.22        | 97.94        |
| TaylorMade Women's RBZ SL Rescue              | 0.00       | 0.00       | 3359.56    | 100.00       | 100.00       | 98.01        |
| Merrell Women's Grassbow Sport Waterproof Hik | 0.00       | 0.00       | 3180.18    | 100.00       | 100.00       | 98.08        |
| Bowflex SelectTech 1090 Dumbbells             | 0.00       | 0.00       | 3131.94    | 100.00       | 100.00       | 98.15        |
| Cleveland Golf Women's 588 RTX CB Satin Chrom | 0.00       | 0.00       | 3120.91    | 100.00       | 100.00       | 98.22        |
| Titleist Pro V1x High Numbers Personalized Go | 9417.93    | 7715.81    | 3034.64    | 96.19        | 96.55        | 98.28        |
| Titleist Pro V1x Golf Balls                   | 6777.60    | 6987.08    | 3031.26    | 97.09        | 96.84        | 98.35        |
| adidas Men's F10 Messi TRX FG Soccer Cleat    | 8138.20    | 9852.73    | 2935.29    | 96.36        | 96.02        | 98.41        |
| Diamondback Boys' Insight 24 Performance Hybr | 0.00       | 0.00       | 2920.39    | 100.00       | 100.00       | 98.48        |
| SOLE E25 Elliptical                           | 0.00       | 0.00       | 2869.97    | 100.00       | 100.00       | 98.54        |
| Nike Men's Fingertrap Max Training Shoe       | 0.00       | 0.00       | 2790.39    | 100.00       | 100.00       | 98.60        |
| Diamondback Girls' Clarity 24 Hybrid Bike 201 | 0.00       | 0.00       | 2738.90    | 100.00       | 100.00       | 98.66        |
| Polar Loop Activity Tracker                   | 0.00       | 0.00       | 2732.25    | 100.00       | 100.00       | 98.72        |
| Nike Men's Comfort 2 Slide                    | 6940.58    | 7107.25    | 2482.54    | 96.81        | 96.69        | 98.77        |
| insta-bed Neverflat Air Mattress              | 0.00       | 0.00       | 2475.56    | 100.00       | 100.00       | 98.83        |
| Titleist Pro V1x High Numbers Golf Balls      | 7185.30    | 5988.88    | 2447.72    | 96.67        | 97.23        | 98.88        |
| Titleist Pro V1 High Numbers Personalized Gol | 7901.12    | 6967.39    | 2275.59    | 96.52        | 96.98        | 98.93        |
| Under Armour Women's Micro G Skulpt Running S | 6306.14    | 8347.18    | 2205.41    | 97.36        | 96.39        | 98.98        |
| Stiga Master Series ST3100 Competition Indoor | 0.00       | 0.00       | 2130.08    | 100.00       | 100.00       | 99.02        |
| Under Armour Men's Compression EV SL Slide    | 6908.16    | 5208.47    | 1915.22    | 96.95        | 97.90        | 99.06        |
| Nike Women's Tempo Shorts                     | 4950.30    | 3770.70    | 1895.40    | 98.11        | 99.07        | 99.10        |
| adidas Kids' F5 Messi FG Soccer Cleat         | 4080.15    | 5545.53    | 1856.38    | 98.73        | 97.57        | 99.14        |
| Bridgestone e6 Straight Distance NFL San Dieg | 4543.86    | 4927.69    | 1767.58    | 98.30        | 98.21        | 99.18        |
| Columbia Men's PFG Anchor Tough T-Shirt       | 4149.60    | 5466.30    | 1757.10    | 98.47        | 97.68        | 99.22        |
| Under Armour Hustle Storm Medium Duffle Bag   | 5328.06    | 4494.27    | 1697.35    | 97.69        | 98.59        | 99.26        |
| Bushnell Pro X7 Jolt Slope Rangefinder        | 0.00       | 0.00       | 1682.97    | 100.00       | 100.00       | 99.29        |
| ENO Atlas Hammock Straps                      | 5653.38    | 5604.95    | 1555.13    | 97.47        | 97.34        | 99.33        |
| Clicgear Rovic Cooler Bag                     | 5291.66    | 5352.02    | 1535.41    | 97.80        | 97.79        | 99.36        |
| TYR Boys' Team Digi Jammer                    | 6659.89    | 5176.07    | 1472.42    | 97.23        | 98.01        | 99.39        |
| Under Armour Men's Tech II T-Shirt            | 3134.49    | 3784.31    | 1433.03    | 99.67        | 99.00        | 99.42        |
| Team Golf San Francisco Giants Putter Grip    | 3939.50    | 3413.46    | 1409.30    | 98.89        | 99.37        | 99.46        |
| Glove It Women's Imperial Golf Glove          | 3829.84    | 2327.41    | 1402.69    | 98.97        | 99.97        | 99.49        |
| Bridgestone e6 Straight Distance NFL Tennesse | 2492.63    | 4342.59    | 1382.77    | 99.78        | 98.68        | 99.52        |
| Team Golf New England Patriots Putter Grip    | 3404.84    | 4256.67    | 1335.56    | 99.40        | 98.76        | 99.54        |
| CDs of rock                                   | 0.00       | 0.00       | 1267.72    | 100.00       | 100.00       | 99.57        |
| Nike Men's Deutschland Weltmeister Winners Bl | 4174.95    | 6209.70    | 1249.05    | 98.39        | 97.11        | 99.60        |
| Under Armour Kids' Mercenary Slide            | 3656.45    | 3090.77    | 1181.45    | 99.04        | 99.56        | 99.62        |
| Toys                                          | 0.00       | 0.00       | 1165.44    | 100.00       | 100.00       | 99.65        |
| Bag Boy Beverage Holder                       | 3430.75    | 3675.13    | 1073.93    | 99.33        | 99.15        | 99.67        |
| Team Golf Texas Longhorns Putter Grip         | 4091.09    | 2484.75    | 1069.31    | 98.64        | 99.83        | 99.70        |
| Under Armour Women's Ignite Slide             | 5329.45    | 4684.24    | 1045.74    | 97.58        | 98.31        | 99.72        |
| Nike Dri-FIT Crew Sock 6 Pack                 | 3291.09    | 2849.77    | 1004.74    | 99.47        | 99.62        | 99.74        |
| Glove It Women's Mod Oval 3-Zip Carry All Gol | 3579.20    | 3616.45    | 982.30     | 99.19        | 99.22        | 99.76        |
| Under Armour Women's Ignite PIP VI Slide      | 5032.47    | 5011.02    | 925.61     | 98.01        | 98.11        | 99.78        |
| Top Flite Women's 2014 XL Hybrid              | 3247.02    | 3496.30    | 885.33     | 99.61        | 99.30        | 99.80        |
| Team Golf Tennessee Volunteers Putter Grip    | 4098.84    | 4518.02    | 876.52     | 98.56        | 98.49        | 99.82        |
| Glove It Urban Brick Golf Towel               | 2667.29    | 2414.87    | 861.62     | 99.73        | 99.93        | 99.84        |
| Team Golf St. Louis Cardinals Putter Grip     | 3622.63    | 4568.64    | 782.93     | 99.12        | 98.40        | 99.86        |
| Glove It Women's Mod Oval Golf Glove          | 2445.25    | 2614.87    | 780.30     | 99.83        | 99.72        | 99.87        |
| Hirzl Women's Soffft Flex Golf Glove          | 3280.07    | 2620.58    | 779.58     | 99.54        | 99.67        | 99.89        |
| Bridgestone e6 Straight Distance NFL Carolina | 5183.60    | 5562.40    | 726.00     | 97.91        | 97.46        | 99.91        |
| Team Golf Pittsburgh Steelers Putter Grip     | 4563.22    | 3825.95    | 718.95     | 98.21        | 98.84        | 99.92        |
| adidas Men's Germany Black Crest Away Tee     | 3479.63    | 3174.01    | 698.50     | 99.26        | 99.43        | 99.94        |
| Hirzl Men's Hybrid Golf Glove                 | 2345.69    | 2544.66    | 682.86     | 99.88        | 99.78        | 99.95        |
| Nike Women's Legend V-Neck T-Shirt            | 4030.25    | 3798.64    | 633.00     | 98.81        | 98.92        | 99.97        |
| Hirzl Women's Hybrid Golf Glove               | 2302.90    | 2416.06    | 620.87     | 99.92        | 99.88        | 99.98        |
| Glove It Imperial Golf Towel                  | 2300.14    | 3133.50    | 606.80     | 99.97        | 99.49        | 99.99        |
| Clicgear 8.0 Shoe Brush                       | 1406.19    | 1265.20    | 380.55     | 100.00       | 100.00       | 100.00       |

Throughout the period of 3 years from 2015 to 2017, around 80% of realized revenue was always generated by only roughly 8-13% of total number of products. This is considered a high risk as the company is heavily reliant on just a few items and might face a huge problem probably caused by minimal decrease of one of these products.

   **7.1.4 Analysis of revenue share by markets**

| Market       | qty_2015 | qty_2016 | qty_2017 | revenue_2015 | revenue_2016 | revenue_2017 |
| ------------ | -------- | -------- | -------- | ------------ | ------------ | ------------ |
| Europe       | 24332    | 4076     | 17904    | 1947790.68   | 327813.45    | 2054315.31   |
| LATAM        | 24981    | 0        | 24062    | 1987011.10   | 0.00         | 2007881.69   |
| Pacific Asia | 11280    | 21055    | 2614     | 922934.44    | 1700923.49   | 453292.13    |
| Africa       | 0        | 10222    | 1162     | 0.00         | 821556.97    | 88239.83     |
| USCA         | 0        | 25341    | 97       | 0.00         | 2023376.80   | 7238.19      |

Europe made up the largest proportion of realized revenue in 2017, followed by LATAM, Pacific Asia, Africa and USCA. Both Europe and LATAM experienced an increase in their revenue while that of other markets witnessed a severe downturn from 2016 to 2017.

| Product Name                                  | product_price | latam_2016 | pacific_asia_2016 | usca_2016 | africa_2016 | europe_2016 | latam_2017 | pacific_asia_2017 | usca_2017 | africa_2017 | europe_2017 | cum_pct_latam_2017 | cum_pct_pacific_2017 | cum_pct_usca_2017 | cum_pct_africa_2017 | cum_pct_europe_2017 |
| --------------------------------------------- | ------------- | ---------- | ----------------- | --------- | ----------- | ----------- | ---------- | ----------------- | --------- | ----------- | ----------- | ------------------ | -------------------- | ----------------- | ------------------- | ------------------- |
| Field & Stream Sportsman 16 Gun Fire Safe     | 399.980011    | 0.00       | 353338.87         | 414629.63 | 169742.80   | 66226.40    | 388100.93  | 18359.00          | 769.96    | 13779.26    | 315288.93   | 19.33              | 70.42                | 61.51             | 15.62               | 15.35               |
| Dell Laptop                                   | 1500          | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 55072.50          | 0.00      | 0.00        | 224070.00   | 100.00             | 29.58                | 100.00            | 100.00              | 26.25               |
| Perfect Fitness Perfect Rip Deck              | 59.99000168   | 0.00       | 219963.78         | 275028.36 | 100908.66   | 43503.43    | 256259.90  | 11056.99          | 1649.72   | 12717.54    | 182588.99   | 32.09              | 83.07                | 22.79             | 30.03               | 35.14               |
| Diamondback Women's Serene Classic Comfort Bi | 299.980011    | 0.00       | 205908.12         | 235482.99 | 92132.31    | 42043.54    | 245254.78  | 9962.27           | 1087.42   | 9999.78     | 178545.62   | 44.31              | 85.27                | 37.82             | 66.64               | 43.83               |
| Nike Men's Free 5.0+ Running Shoe             | 99.98999786   | 0.00       | 176635.39         | 214844.97 | 96637.60    | 32538.68    | 214224.96  | 9458.04           | 399.96    | 12018.29    | 162832.34   | 54.98              | 87.35                | 97.76             | 43.65               | 51.76               |
| Nike Men's Dri-FIT Victory Golf Polo          | 50            | 0.00       | 156859.50         | 189053.00 | 78335.00    | 33347.50    | 182262.25  | 8422.00           | 540.00    | 10286.00    | 134921.75   | 73.24              | 91.20                | 92.23             | 55.31               | 58.33               |
| Pelican Sunstream 100 Kayak                   | 199.9900055   | 0.00       | 153895.44         | 180953.92 | 76116.78    | 28277.43    | 184562.76  | 6496.63           | 553.97    | 8199.55     | 134864.49   | 64.17              | 95.85                | 84.77             | 75.93               | 64.89               |
| Nike Men's CJ Elite 2 TD Football Cleat       | 129.9900055   | 0.00       | 150921.61         | 176914.21 | 71220.91    | 26233.06    | 171533.96  | 7461.37           | 945.02    | 6781.52     | 118984.61   | 81.79              | 92.85                | 50.87             | 83.62               | 70.69               |
| O'Brien Men's Neoprene Life Vest              | 49.97999954   | 0.00       | 145198.84         | 170322.43 | 70195.01    | 30312.01    | 169364.56  | 7136.65           | 555.28    | 6496.61     | 114589.33   | 90.22              | 94.42                | 77.12             | 90.98               | 76.26               |
| Web Camera                                    | 452.0400085   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 1988.98           | 0.00      | 0.00        | 101397.10   | 100.00             | 98.51                | 100.00            | 100.00              | 81.20               |
| Children's heaters                            | 357.1000061   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 19376.29          | 0.00      | 0.00        | 69559.69    | 100.00             | 62.22                | 100.00            | 100.00              | 84.59               |
| Under Armour Girls' Toddler Spine Surge Runni | 39.99000168   | 0.00       | 62067.30          | 76398.79  | 30387.97    | 12303.01    | 73440.82   | 3844.20           | 574.66    | 2957.45     | 50596.03    | 93.88              | 96.70                | 69.45             | 94.33               | 87.05               |
| Porcelain crafts                              | 461.480011    | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 48630.83          | 0.00      | 0.00        | 42841.54    | 100.00             | 40.31                | 100.00            | 100.00              | 89.13               |
| Industrial consumer electronics               | 252.8800049   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 15488.84          | 0.00      | 0.00        | 30452.98    | 100.00             | 77.51                | 100.00            | 100.00              | 90.62               |
| SOLE E35 Elliptical                           | 1999.98999    | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 19449.89    | 100.00             | 100.00               | 100.00            | 100.00              | 91.56               |
| DVDs                                          | 164.3800049   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 16622.88          | 0.00      | 0.00        | 15824.03    | 100.00             | 74.09                | 100.00            | 100.00              | 92.33               |
| Garmin Forerunner 910XT GPS Watch             | 399.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 7069.80     | 100.00             | 100.00               | 100.00            | 100.00              | 92.68               |
| LIJA Women's Mid-Length Panel Golf Shorts     | 100           | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2321.00    | 0.00              | 0.00      | 0.00        | 7058.00     | 96.13              | 100.00               | 100.00            | 100.00              | 93.02               |
| GoPro HERO3+ Black Edition Camera             | 399.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 6565.82     | 100.00             | 100.00               | 100.00            | 100.00              | 93.34               |
| TaylorMade White Smoke IN-12 Putter           | 99.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 3455.14    | 0.00              | 0.00      | 0.00        | 5485.44     | 94.29              | 100.00               | 100.00            | 100.00              | 93.61               |
| Polar FT4 Heart Rate Monitor                  | 89.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2267.74    | 0.00              | 0.00      | 0.00        | 4801.40     | 96.35              | 100.00               | 100.00            | 100.00              | 93.84               |
| Nike Men's Free TR 5.0 TB Training Shoe       | 99.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2388.75    | 0.00              | 0.00      | 0.00        | 4726.00     | 95.77              | 100.00               | 100.00            | 100.00              | 94.07               |
| Garmin Approach S4 Golf GPS Watch             | 299.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 4693.33     | 100.00             | 100.00               | 100.00            | 100.00              | 94.30               |
| Nike Women's Free 5.0 TR FIT PRT 4 Training S | 94.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 3061.99    | 0.00              | 0.00      | 0.00        | 4639.33     | 94.61              | 100.00               | 100.00            | 100.00              | 94.53               |
| Pelican Maverick 100X Kayak                   | 349.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 4625.10     | 100.00             | 100.00               | 100.00            | 100.00              | 94.75               |
| Merrell Women's Grassbow Sport Hiking Shoe    | 99.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1627.82    | 0.00              | 0.00      | 0.00        | 4453.55     | 97.66              | 100.00               | 100.00            | 100.00              | 94.97               |
| Brooks Women's Ghost 6 Running Shoe           | 89.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2797.80    | 0.00              | 0.00      | 0.00        | 4452.70     | 94.75              | 100.00               | 100.00            | 100.00              | 95.18               |
| The North Face Women's Recon Backpack         | 99            | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2109.69    | 0.00              | 0.00      | 0.00        | 4372.34     | 96.67              | 100.00               | 100.00            | 100.00              | 95.40               |
| Total Gym 1400                                | 299.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 4309.34     | 100.00             | 100.00               | 100.00            | 100.00              | 95.61               |
| Titleist Club Glove Travel Cover              | 299.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 4253.84     | 100.00             | 100.00               | 100.00            | 100.00              | 95.81               |
| Garmin Approach S3 Golf GPS Watch             | 249.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2692.38    | 0.00              | 0.00      | 0.00        | 4053.57     | 95.02              | 100.00               | 100.00            | 100.00              | 96.01               |
| Bag Boy M330 Push Cart                        | 79.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1573.38    | 0.00              | 0.00      | 0.00        | 3836.31     | 97.90              | 100.00               | 100.00            | 100.00              | 96.20               |
| Elevation Training Mask 2.0                   | 79.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 4751.81    | 0.00              | 0.00      | 0.00        | 3654.73     | 94.12              | 100.00               | 100.00            | 100.00              | 96.38               |
| Sports Books                                  | 31.07999992   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 1125.90           | 0.00      | 0.00        | 3610.96     | 100.00             | 99.01                | 100.00            | 100.00              | 96.55               |
| Nike Kids' Grade School KD VI Basketball Shoe | 99.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2547.24    | 0.00              | 0.00      | 0.00        | 3567.64     | 95.41              | 100.00               | 100.00            | 100.00              | 96.73               |
| LIJA Women's Argyle Golf Polo                 | 80            | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2690.40    | 0.00              | 0.00      | 0.00        | 3429.20     | 95.16              | 100.00               | 100.00            | 100.00              | 96.89               |
| Fitbit The One Wireless Activity & Sleep Trac | 99.94999695   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2367.81    | 0.00              | 0.00      | 0.00        | 3368.34     | 95.89              | 100.00               | 100.00            | 100.00              | 97.06               |
| Mio ALPHA Heart Rate Monitor/Sport Watch      | 199           | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2117.36    | 0.00              | 0.00      | 0.00        | 3250.67     | 96.56              | 100.00               | 100.00            | 100.00              | 97.21               |
| Yakima DoubleDown Ace Hitch Mount 4-Bike Rack | 189           | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1367.42    | 0.00              | 0.00      | 0.00        | 3161.03     | 98.40              | 100.00               | 100.00            | 100.00              | 97.37               |
| Bowflex SelectTech 1090 Dumbbells             | 599.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 3131.94     | 100.00             | 100.00               | 100.00            | 100.00              | 97.52               |
| Diamondback Boys' Insight 24 Performance Hybr | 299.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 2920.39     | 100.00             | 100.00               | 100.00            | 100.00              | 97.66               |
| MDGolf Pittsburgh Penguins Putter             | 79.98999786   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1669.38    | 0.00              | 0.00      | 0.00        | 2904.44     | 97.50              | 100.00               | 100.00            | 100.00              | 97.80               |
| SOLE E25 Elliptical                           | 999.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 2869.97     | 100.00             | 100.00               | 100.00            | 100.00              | 97.94               |
| Diamondback Girls' Clarity 24 Hybrid Bike 201 | 299.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 2738.90     | 100.00             | 100.00               | 100.00            | 100.00              | 98.08               |
| TaylorMade 2014 Purelite Stand Bag            | 179.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2287.66    | 0.00              | 0.00      | 0.00        | 2574.74     | 96.24              | 100.00               | 100.00            | 100.00              | 98.20               |
| Merrell Men's All Out Flash Trail Running Sho | 109.9899979   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1151.03    | 0.00              | 0.00      | 0.00        | 2484.65     | 98.97              | 100.00               | 100.00            | 100.00              | 98.32               |
| GolfBuddy VT3 GPS Watch                       | 199.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1479.92    | 0.00              | 0.00      | 0.00        | 2482.86     | 98.12              | 100.00               | 100.00            | 100.00              | 98.44               |
| TaylorMade Women's RBZ SL Rescue              | 129.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1054.21    | 0.00              | 0.00      | 0.00        | 2305.35     | 99.07              | 100.00               | 100.00            | 100.00              | 98.56               |
| adidas Brazuca 2014 Official Match Ball       | 159.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1741.48    | 0.00              | 0.00      | 0.00        | 2271.84     | 97.33              | 100.00               | 100.00            | 100.00              | 98.67               |
| Ogio Race Golf Shoes                          | 169.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1657.39    | 0.00              | 0.00      | 0.00        | 2248.95     | 97.58              | 100.00               | 100.00            | 100.00              | 98.78               |
| Cleveland Golf Women's 588 RTX CB Satin Chrom | 119.9899979   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 872.32     | 0.00              | 0.00      | 0.00        | 2248.59     | 99.50              | 100.00               | 100.00            | 100.00              | 98.89               |
| Nike Men's Fingertrap Max Training Shoe       | 124.9899979   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 571.20     | 0.00              | 0.00      | 0.00        | 2219.19     | 99.90              | 100.00               | 100.00            | 100.00              | 98.99               |
| Stiga Master Series ST3100 Competition Indoor | 329.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 2130.08     | 100.00             | 100.00               | 100.00            | 100.00              | 99.10               |
| Titleist Small Wheeled Travel Cover           | 249.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2517.39    | 0.00              | 0.00      | 0.00        | 2014.91     | 95.54              | 100.00               | 100.00            | 100.00              | 99.20               |
| Cleveland Golf Collegiate My Custom Wedge 588 | 209.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1833.20    | 0.00              | 0.00      | 0.00        | 1822.70     | 97.16              | 100.00               | 100.00            | 100.00              | 99.28               |
| Merrell Women's Grassbow Sport Waterproof Hik | 129.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1431.83    | 0.00              | 0.00      | 0.00        | 1748.35     | 98.19              | 100.00               | 100.00            | 100.00              | 99.37               |
| Merrell Women's Siren Mid Waterproof Hiking B | 134.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1952.62    | 0.00              | 0.00      | 0.00        | 1721.79     | 96.97              | 100.00               | 100.00            | 100.00              | 99.45               |
| Bushnell Pro X7 Jolt Slope Rangefinder        | 599.9899902   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 0.00              | 0.00      | 0.00        | 1682.97     | 100.00             | 100.00               | 100.00            | 100.00              | 99.54               |
| Baby sweater                                  | 59.08000183   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 2690.23           | 0.00      | 0.00        | 1639.21     | 100.00             | 98.07                | 100.00            | 100.00              | 99.62               |
| LIJA Women's Button Golf Dress                | 108           | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1971.54    | 0.00              | 0.00      | 0.00        | 1539.00     | 96.87              | 100.00               | 100.00            | 100.00              | 99.69               |
| Polar Loop Activity Tracker                   | 109.9499969   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1262.23    | 0.00              | 0.00      | 0.00        | 1470.02     | 98.67              | 100.00               | 100.00            | 100.00              | 99.76               |
| insta-bed Neverflat Air Mattress              | 149.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 1163.91    | 0.00              | 0.00      | 0.00        | 1311.65     | 98.91              | 100.00               | 100.00            | 100.00              | 99.83               |
| Nike Men's Kobe IX Elite Low Basketball Shoe  | 199.9900055   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 2152.88    | 0.00              | 0.00      | 0.00        | 1235.93     | 96.46              | 100.00               | 100.00            | 100.00              | 99.89               |
| CDs of rock                                   | 11.28999996   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 581.44            | 0.00      | 0.00        | 686.28      | 100.00             | 99.29                | 100.00            | 100.00              | 99.92               |
| Under Armour Men's Compression EV SL Slide    | 44.99000168   | 0.00       | 2136.57           | 1809.04   | 942.98      | 319.88      | 1406.84    | 0.00              | 0.00      | 85.48       | 422.90      | 98.33              | 100.00               | 100.00            | 99.10               | 99.94               |
| Titleist Pro V1 High Numbers Personalized Gol | 51.99000168   | 0.00       | 2116.49           | 3249.10   | 1344.97     | 256.83      | 1753.61    | 0.00              | 0.00      | 272.43      | 249.55      | 97.24              | 100.00               | 100.00            | 96.10               | 99.95               |
| Under Armour Women's Micro G Skulpt Running S | 54.97000122   | 0.00       | 2899.68           | 2738.31   | 2217.21     | 491.98      | 1502.89    | 378.75            | 0.00      | 90.15       | 233.62      | 97.98              | 99.37                | 100.00            | 98.90               | 99.96               |
| adidas Men's F10 Messi TRX FG Soccer Cleat    | 59.99000168   | 0.00       | 2393.58           | 4198.10   | 2393.60     | 867.45      | 2357.29    | 283.45            | 0.00      | 116.38      | 178.17      | 96.01              | 99.44                | 100.00            | 98.70               | 99.97               |
| Columbia Men's PFG Anchor Tough T-Shirt       | 30            | 0.00       | 1464.15           | 3003.45   | 856.50      | 142.20      | 1604.70    | 26.40             | 0.00      | 0.00        | 126.00      | 97.74              | 99.98                | 100.00            | 100.00              | 99.98               |
| Under Armour Hustle Storm Medium Duffle Bag   | 34.99000168   | 0.00       | 1473.26           | 1824.00   | 1022.06     | 174.95      | 1257.18    | 173.20            | 0.00      | 171.45      | 95.52       | 98.73              | 99.57                | 100.00            | 97.63               | 99.98               |
| Glove It Women's Mod Oval Golf Glove          | 19.98999977   | 0.00       | 899.15            | 1139.02   | 376.40      | 200.30      | 620.19     | 0.00              | 0.00      | 70.36       | 89.75       | 99.76              | 100.00               | 100.00            | 99.52               | 99.99               |
| Team Golf St. Louis Cardinals Putter Grip     | 24.98999977   | 0.00       | 1855.38           | 1671.94   | 826.91      | 214.41      | 610.75     | 24.49             | 0.00      | 72.72       | 74.97       | 99.79              | 100.00               | 100.00            | 99.36               | 99.99               |
| Hirzl Women's Hybrid Golf Glove               | 14.98999977   | 0.00       | 698.84            | 1255.39   | 146.75      | 315.08      | 534.68     | 13.04             | 0.00      | 13.19       | 59.96       | 99.96              | 100.00               | 100.00            | 100.00              | 99.99               |
| Team Golf New England Patriots Putter Grip    | 24.98999977   | 0.00       | 1459.29           | 1921.11   | 623.37      | 252.90      | 1130.40    | 141.94            | 0.00      | 23.24       | 39.98       | 99.02              | 99.70                | 100.00            | 99.92               | 100.00              |
| Titleist Pro V1x High Numbers Personalized Go | 51.99000168   | 0.00       | 2710.49           | 2890.89   | 1286.75     | 827.68      | 2058.28    | 682.62            | 0.00      | 254.75      | 38.99       | 96.77              | 99.16                | 100.00            | 96.70               | 100.00              |
| Under Armour Women's Ignite PIP VI Slide      | 31.98999977   | 0.00       | 1263.91           | 2937.46   | 645.55      | 164.10      | 758.78     | 0.00              | 0.00      | 136.60      | 30.23       | 99.59              | 100.00               | 100.00            | 98.15               | 100.00              |
| Nike Women's Tempo Shorts                     | 30            | 0.00       | 1567.95           | 1785.00   | 212.85      | 204.90      | 1600.80    | 25.50             | 0.00      | 243.90      | 25.20       | 97.82              | 99.99                | 100.00            | 96.97               | 100.00              |
| Adult dog supplies                            | 84.40000153   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 9020.68           | 0.00      | 0.00        | 0.00        | 100.00             | 89.34                | 100.00            | 100.00              | 100.00              |
| adidas Youth Germany Black/Red Away Match Soc | 70            | 0.00       | 4438.70           | 5168.45   | 851.55      | 681.80      | 3418.45    | 132.30            | 0.00      | 373.80      | 0.00        | 94.46              | 99.76                | 100.00            | 95.46               | 100.00              |
| LIJA Women's Eyelet Sleeveless Golf Polo      | 65            | 0.00       | 3713.46           | 3972.15   | 1338.35     | 414.70      | 2762.18    | 0.00              | 0.00      | 620.75      | 0.00        | 94.89              | 100.00               | 100.00            | 95.03               | 100.00              |
| Titleist Pro V1x Golf Balls                   | 47.99000168   | 0.00       | 3196.60           | 2745.50   | 471.02      | 573.96      | 2555.68    | 203.96            | 40.79     | 230.83      | 0.00        | 95.28              | 99.53                | 99.58             | 97.24               | 100.00              |
| Nike Men's Comfort 2 Slide                    | 44.99000168   | 0.00       | 2999.02           | 3033.87   | 542.58      | 531.78      | 2394.36    | 0.00              | 0.00      | 88.18       | 0.00        | 95.66              | 100.00               | 100.00            | 99.00               | 100.00              |
| Titleist Pro V1x High Numbers Golf Balls      | 47.99000168   | 0.00       | 1877.35           | 1950.78   | 1813.30     | 347.45      | 1926.55    | 132.45            | 90.70     | 298.02      | 0.00        | 97.06              | 99.73                | 99.01             | 95.79               | 100.00              |
| adidas Kids' F5 Messi FG Soccer Cleat         | 34.99000168   | 0.00       | 1479.37           | 2346.24   | 1409.56     | 310.36      | 1735.32    | 28.69             | 0.00      | 92.37       | 0.00        | 97.42              | 99.98                | 100.00            | 98.80               | 100.00              |
| Bridgestone e6 Straight Distance NFL San Dieg | 31.98999977   | 0.00       | 2300.37           | 2023.67   | 293.67      | 309.98      | 1487.35    | 115.16            | 0.00      | 165.07      | 0.00        | 98.05              | 99.84                | 100.00            | 97.82               | 100.00              |
| Clicgear Rovic Cooler Bag                     | 39.99000168   | 0.00       | 2175.45           | 2102.05   | 647.04      | 427.48      | 1415.44    | 0.00              | 0.00      | 119.97      | 0.00        | 98.26              | 100.00               | 100.00            | 98.43               | 100.00              |
| ENO Atlas Hammock Straps                      | 29.98999977   | 0.00       | 2272.35           | 1988.75   | 876.76      | 467.09      | 1359.30    | 78.27             | 0.00      | 117.56      | 0.00        | 98.47              | 99.88                | 100.00            | 98.56               | 100.00              |
| TYR Boys' Team Digi Jammer                    | 39.99000168   | 0.00       | 1556.80           | 2901.45   | 427.50      | 290.32      | 1354.85    | 117.57            | 0.00      | 0.00        | 0.00        | 98.54              | 99.82                | 100.00            | 100.00              | 100.00              |
| Team Golf San Francisco Giants Putter Grip    | 24.98999977   | 0.00       | 1436.15           | 1076.81   | 812.54      | 87.96       | 1283.35    | 40.98             | 0.00      | 84.97       | 0.00        | 98.60              | 99.97                | 100.00            | 99.20               | 100.00              |
| Under Armour Men's Tech II T-Shirt            | 24.98999977   | 0.00       | 1391.56           | 1505.88   | 768.42      | 118.45      | 1253.10    | 0.00              | 0.00      | 179.93      | 0.00        | 98.79              | 100.00               | 100.00            | 97.44               | 100.00              |
| Glove It Women's Imperial Golf Glove          | 19.98999977   | 0.00       | 1043.87           | 770.20    | 453.97      | 59.37       | 1217.19    | 147.72            | 0.00      | 37.78       | 0.00        | 98.85              | 99.67                | 100.00            | 99.82               | 100.00              |
| Team Golf Texas Longhorns Putter Grip         | 24.98999977   | 0.00       | 744.19            | 1035.34   | 705.22      | 0.00        | 1045.57    | 0.00              | 0.00      | 23.74       | 0.00        | 99.13              | 100.00               | 100.00            | 99.87               | 100.00              |
| Nike Men's Deutschland Weltmeister Winners Bl | 30            | 0.00       | 2411.40           | 2765.70   | 889.20      | 143.40      | 1039.65    | 164.40            | 0.00      | 45.00       | 0.00        | 99.18              | 99.64                | 100.00            | 99.77               | 100.00              |
| Nike Dri-FIT Crew Sock 6 Pack                 | 22            | 0.00       | 1070.19           | 871.86    | 489.94      | 417.78      | 985.60     | 0.00              | 0.00      | 19.14       | 0.00        | 99.23              | 100.00               | 100.00            | 99.95               | 100.00              |
| Bridgestone e6 Straight Distance NFL Tennesse | 31.98999977   | 0.00       | 1692.41           | 1323.90   | 854.44      | 471.84      | 977.45     | 218.82            | 30.71     | 155.79      | 0.00        | 99.28              | 99.48                | 100.00            | 98.00               | 100.00              |
| Glove It Women's Mod Oval 3-Zip Carry All Gol | 21.98999977   | 0.00       | 1468.48           | 1761.51   | 177.34      | 209.12      | 963.83     | 0.00              | 0.00      | 18.47       | 0.00        | 99.32              | 100.00               | 100.00            | 99.97               | 100.00              |
| Bag Boy Beverage Holder                       | 24.98999977   | 0.00       | 1101.32           | 1572.72   | 724.45      | 276.64      | 961.47     | 112.46            | 0.00      | 0.00        | 0.00        | 99.37              | 99.87                | 100.00            | 100.00              | 100.00              |
| Under Armour Kids' Mercenary Slide            | 27.98999977   | 0.00       | 1614.01           | 918.08    | 330.01      | 228.67      | 911.63     | 0.00              | 0.00      | 269.82      | 0.00        | 99.42              | 100.00               | 100.00            | 96.41               | 100.00              |
| Under Armour Women's Ignite Slide             | 31.98999977   | 0.00       | 1693.22           | 1747.91   | 963.84      | 279.27      | 879.71     | 166.03            | 0.00      | 0.00        | 0.00        | 99.46              | 99.60                | 100.00            | 100.00              | 100.00              |
| Team Golf Tennessee Volunteers Putter Grip    | 24.98999977   | 0.00       | 1116.42           | 1933.96   | 1051.81     | 415.83      | 852.90     | 0.00              | 0.00      | 23.62       | 0.00        | 99.55              | 100.00               | 100.00            | 99.90               | 100.00              |
| Top Flite Women's 2014 XL Hybrid              | 19.98999977   | 0.00       | 917.42            | 1646.06   | 743.02      | 189.80      | 739.01     | 74.36             | 0.00      | 71.96       | 0.00        | 99.62              | 99.90                | 100.00            | 99.44               | 100.00              |
| Glove It Urban Brick Golf Towel               | 15.98999977   | 0.00       | 878.08            | 1131.28   | 405.51      | 0.00        | 729.70     | 65.56             | 0.00      | 66.36       | 0.00        | 99.66              | 99.91                | 100.00            | 99.60               | 100.00              |
| Team Golf Pittsburgh Steelers Putter Grip     | 24.98999977   | 0.00       | 1668.08           | 1235.87   | 594.51      | 327.49      | 718.95     | 0.00              | 0.00      | 0.00        | 0.00        | 99.69              | 100.00               | 100.00            | 100.00              | 100.00              |
| Bridgestone e6 Straight Distance NFL Carolina | 31.98999977   | 0.00       | 1735.13           | 1966.26   | 1416.67     | 444.34      | 663.94     | 0.00              | 0.00      | 62.06       | 0.00        | 99.73              | 100.00               | 100.00            | 99.67               | 100.00              |
| Hirzl Women's Soffft Flex Golf Glove          | 17.98999977   | 0.00       | 858.48            | 1210.80   | 521.80      | 29.50       | 593.20     | 60.45             | 0.00      | 125.93      | 0.00        | 99.82              | 99.94                | 100.00            | 98.29               | 100.00              |
| Hirzl Men's Hybrid Golf Glove                 | 14.98999977   | 0.00       | 869.01            | 997.28    | 622.61      | 55.76       | 581.83     | 52.17             | 0.00      | 48.86       | 0.00        | 99.85              | 99.95                | 100.00            | 99.72               | 100.00              |
| adidas Men's Germany Black Crest Away Tee     | 25            | 0.00       | 695.63            | 1849.38   | 532.25      | 96.75       | 577.25     | 121.25            | 0.00      | 0.00        | 0.00        | 99.88              | 99.79                | 100.00            | 100.00              | 100.00              |
| Nike Women's Legend V-Neck T-Shirt            | 25            | 0.00       | 1280.25           | 1952.88   | 389.38      | 176.13      | 559.75     | 49.00             | 0.00      | 24.25       | 0.00        | 99.93              | 99.96                | 100.00            | 99.84               | 100.00              |
| Glove It Imperial Golf Towel                  | 15.98999977   | 0.00       | 1069.07           | 1259.58   | 703.63      | 101.22      | 529.89     | 61.40             | 0.00      | 15.51       | 0.00        | 99.99              | 99.93                | 100.00            | 99.99               | 100.00              |
| Clicgear 8.0 Shoe Brush                       | 9.989999771   | 0.00       | 432.05            | 559.53    | 162.13      | 111.49      | 298.02     | 9.09              | 0.00      | 73.44       | 0.00        | 100.00             | 100.00               | 100.00            | 99.28               | 100.00              |
| Men's gala suit                               | 210.8500061   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 14157.55          | 0.00      | 0.00        | 0.00        | 100.00             | 80.63                | 100.00            | 100.00              | 100.00              |
| Smart watch                                   | 327.75        | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 18807.98          | 0.00      | 0.00        | 0.00        | 100.00             | 66.37                | 100.00            | 100.00              | 100.00              |
| Summer dresses                                | 215.8200073   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 21299.34          | 0.00      | 0.00        | 0.00        | 100.00             | 57.95                | 100.00            | 100.00              | 100.00              |
| Rock music                                    | 260.6499939   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 28727.48          | 0.00      | 0.00        | 0.00        | 100.00             | 53.25                | 100.00            | 100.00              | 100.00              |
| First aid kit                                 | 293.0400085   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 29920.85          | 0.00      | 0.00        | 0.00        | 100.00             | 46.91                | 100.00            | 100.00              | 100.00              |
| Lawn mower                                    | 532.5800171   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 79018.86          | 0.00      | 0.00        | 0.00        | 100.00             | 17.43                | 100.00            | 100.00              | 100.00              |
| Toys                                          | 11.53999996   | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 1165.44           | 0.00      | 0.00        | 0.00        | 100.00             | 98.76                | 100.00            | 100.00              | 100.00              |
| Fighting video games                          | 39.75         | 0.00       | 0.00              | 0.00      | 0.00        | 0.00        | 0.00       | 3497.43           | 0.00      | 0.00        | 0.00        | 100.00             | 97.47                | 100.00            | 100.00              | 100.00              |

| Market       | qty_2016 | revenue_2016 | arpu_2016 | qty_2017 | revenue_2017 | arpu_2017 |
| ------------ | -------- | ------------ | --------- | -------- | ------------ | --------- |
| Europe       | 4076     | 327813.45    | 80.43     | 17904    | 2054315.31   | 114.74    |
| LATAM        | 0        | 0.00         | null      | 24062    | 2007881.69   | 83.45     |
| Pacific Asia | 21055    | 1700923.49   | 80.78     | 2614     | 453292.13    | 173.41    |
| Africa       | 10222    | 821556.97    | 80.37     | 1162     | 88239.83     | 75.94     |
| USCA         | 25341    | 2023376.80   | 79.85     | 97       | 7238.19      | 74.62     |

Over 80% of revenue of Europe and LATAM were accounted for by products with leading revenue share across whole company scale. However, for Europe, its revenue was spread over a larger product range and despite having lower quantity of units sold, its overall realized revenue was still higher than LATAM’s because 16% of its revenue was generated by costly items i.e web camera and dell laptop.

As for Pacific Asia, its realized revenue dropped by 73% from 2016 to 2017. In 2016, the majority of revenue of this market was earned by the same products that were bringing the most revenue to the company. In 2017, this pattern changed as 80.6% of this market’s earnings was made by newly released products instead of those that led during 2016. Even though the revenue per unit of this market was twice as high as LATAM’s, the number of units sold to LATAM was 9.2 times higher than that sold to Pacific Asia. This in turn brought Pacific Asia the 3rd place in revenue ranking. 

The distribution of slightly over 80% of revenue over available products for Africa and USCA remained nearly the same as previous year and as that of LATAM. However, the quantity sold of these 02 markets was particularly low, leading them to end up as the 02 markets generating least revenue.
