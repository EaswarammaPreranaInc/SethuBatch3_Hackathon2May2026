Table: **customers**

| Column        | Type         | Nullable | Description                                                       |
| ------------- | ------------ | -------- | ----------------------------------------------------------------- |
| customer_id   | INT (PK, AI) | No       | Unique customer identifier                                        |
| full_name     | VARCHAR(120) | No       | Customer's full name                                              |
| email         | VARCHAR(180) | No       | Email address (unique)                                            |
| phone         | VARCHAR(20)  | No       | Mobile number (may have formatting issues in dirty data)          |
| city          | VARCHAR(60)  | No       | City of residence                                                 |
| pincode       | VARCHAR(10)  | No       | Postal code                                                       |
| gender        | VARCHAR(10)  | Yes      | M / F / Other — has nulls and dirty values (e.g., "male", "MALE") |
| date_of_birth | DATE         | Yes      | DOB — some records have future dates (dirty)                      |
| registered_at | DATETIME     | No       | Account creation timestamp                                        |
| is_active     | TINYINT(1)   | No       | 1 = active, 0 = deactivated                                       |

Table: **restaurants**

| Column           | Type         | Nullable | Description                                        |
| ---------------- | ------------ | -------- | -------------------------------------------------- |
| restaurant_id    | INT (PK, AI) | No       | Unique restaurant identifier                       |
| name             | VARCHAR(150) | No       | Restaurant name                                    |
| owner_name       | VARCHAR(120) | No       | Owner's name                                       |
| city             | VARCHAR(60)  | No       | City (may have case inconsistencies in dirty data) |
| area             | VARCHAR(80)  | No       | Neighbourhood/area name                            |
| pincode          | VARCHAR(10)  | No       | Postal code                                        |
| cuisine_type     | VARCHAR(80)  | No       | Comma-separated cuisine tags                       |
| avg_cost_for_two | DECIMAL(8,2) | Yes      | Approximate cost for two people (INR)              |
| opening_time     | TIME         | No       | Daily opening time                                 |
| closing_time     | TIME         | No       | Daily closing time                                 |
| is_veg_only      | TINYINT(1)   | No       | 1 = pure vegetarian restaurant                     |
| rating           | DECIMAL(3,2) | Yes      | Aggregate platform rating (1.0–5.0)                |
| registered_at    | DATETIME     | No       | Restaurant onboarding date                         |
| is_active        | TINYINT(1)   | No       | 1 = currently active on platform                   |

Table: **menu_items**
| Column        | Type         | Nullable | Description                                                        |
| ------------- | ------------ | -------- | ------------------------------------------------------------------ |
| item_id       | INT (PK, AI) | No       | Unique item identifier                                             |
| restaurant_id | INT (FK)     | No       | Owning restaurant                                                  |
| item_name     | VARCHAR(150) | No       | Dish name                                                          |
| cuisine       | VARCHAR(60)  | No       | Cuisine classification (North Indian, South Indian, Chinese, etc.) |
| category      | VARCHAR(60)  | No       | Starter / Main Course / Dessert / Beverage / Bread / Rice          |
| price         | DECIMAL(8,2) | No       | Current price in INR (may have negatives or zeros in dirty data)   |
| is_veg        | TINYINT(1)   | No       | 1 = vegetarian                                                     |
| is_available  | TINYINT(1)   | No       | 1 = currently on menu                                              |
| description   | VARCHAR(300) | Yes      | Short dish description                                             |

Table: **offers**
| Column          | Type         | Nullable | Description                                             |
| --------------- | ------------ | -------- | ------------------------------------------------------- |
| offer_id        | INT (PK, AI) | No       | Unique offer identifier                                 |
| offer_code      | VARCHAR(30)  | No       | Coupon code (e.g., SAVE50)                              |
| offer_type      | VARCHAR(40)  | No       | FLAT_DISCOUNT / PERCENT_DISCOUNT / FREE_DELIVERY / BOGO |
| discount_value  | DECIMAL(8,2) | No       | INR amount or percentage                                |
| min_order_value | DECIMAL(8,2) | No       | Minimum cart value to apply offer                       |
| max_discount    | DECIMAL(8,2) | Yes      | Maximum INR discount for percentage offers              |
| valid_from      | DATE         | No       | Offer start date                                        |
| valid_to        | DATE         | No       | Offer end date                                          |
| is_active       | TINYINT(1)   | No       | 1 = currently active                                    |

Table: **delivery_partners**

| Column       | Type         | Nullable | Description               |
| ------------ | ------------ | -------- | ------------------------- |
| partner_id   | INT (PK, AI) | No       | Unique partner identifier |
| full_name    | VARCHAR(120) | No       | Partner's full name       |
| phone        | VARCHAR(20)  | No       | Contact number            |
| city         | VARCHAR(60)  | No       | Assigned city             |
| vehicle_type | VARCHAR(30)  | No       | BIKE / BICYCLE / SCOOTER  |
| joined_at    | DATETIME     | No       | Onboarding date           |
| is_active    | TINYINT(1)   | No       | 1 = currently active      |

Table: **orders**

| Column               | Type          | Nullable | Description                                                               |
| -------------------- | ------------- | -------- | ------------------------------------------------------------------------- |
| order_id             | INT (PK, AI)  | No       | Unique order identifier                                                   |
| customer_id          | INT (FK)      | No       | Placing customer                                                          |
| restaurant_id        | INT (FK)      | No       | Restaurant fulfilling the order                                           |
| offer_id             | INT (FK)      | Yes      | Applied offer (NULL = no offer)                                           |
| order_status         | VARCHAR(20)   | No       | PLACED / CONFIRMED / PREPARING / OUT_FOR_DELIVERY / DELIVERED / CANCELLED |
| ordered_at           | DATETIME      | No       | Order placement timestamp                                                 |
| confirmed_at         | DATETIME      | Yes      | Restaurant confirmation timestamp                                         |
| ready_at             | DATETIME      | Yes      | Food ready timestamp                                                      |
| subtotal             | DECIMAL(10,2) | No       | Sum of item prices before discount                                        |
| discount_amount      | DECIMAL(10,2) | No       | Total discount applied                                                    |
| delivery_fee         | DECIMAL(8,2)  | No       | Platform delivery charge                                                  |
| total_amount         | DECIMAL(10,2) | No       | subtotal − discount + delivery_fee                                        |
| special_instructions | VARCHAR(300)  | Yes      | Customer notes                                                            |

Table: **order_items**

| Column        | Type          | Nullable | Description                     |
| ------------- | ------------- | -------- | ------------------------------- |
| order_item_id | INT (PK, AI)  | No       | Unique line-item identifier     |
| order_id      | INT (FK)      | No       | Parent order                    |
| item_id       | INT (FK)      | No       | Menu item ordered               |
| quantity      | INT           | No       | Number of units                 |
| unit_price    | DECIMAL(8,2)  | No       | Price per unit at time of order |
| item_total    | DECIMAL(10,2) | No       | quantity × unit_price           |

Table: **payments**
| Column          | Type          | Nullable | Description                                   |
| --------------- | ------------- | -------- | --------------------------------------------- |
| payment_id      | INT (PK, AI)  | No       | Unique payment record                         |
| order_id        | INT (FK)      | No       | Corresponding order                           |
| payment_method  | VARCHAR(30)   | No       | UPI / CREDIT_CARD / DEBIT_CARD / COD / WALLET |
| payment_status  | VARCHAR(20)   | No       | SUCCESS / FAILED / PENDING / REFUNDED         |
| amount          | DECIMAL(10,2) | No       | Amount charged                                |
| paid_at         | DATETIME      | Yes      | Payment completion timestamp                  |
| transaction_ref | VARCHAR(60)   | Yes      | Gateway reference number                      |



Table: **deliveries**

| Column          | Type         | Nullable | Description                               |
| --------------- | ------------ | -------- | ----------------------------------------- |
| delivery_id     | INT (PK, AI) | No       | Unique delivery record                    |
| order_id        | INT (FK)     | No       | Associated order                          |
| partner_id      | INT (FK)     | No       | Assigned delivery partner                 |
| picked_up_at    | DATETIME     | Yes      | Pickup timestamp                          |
| delivered_at    | DATETIME     | Yes      | Delivery timestamp                        |
| delivery_status | VARCHAR(20)  | No       | ASSIGNED / PICKED_UP / DELIVERED / FAILED |
| distance_km     | DECIMAL(5,2) | Yes      | Delivery distance in kilometres           |
| delivery_rating | TINYINT      | Yes      | Customer rating for delivery (1–5)        |


Table: **ratings**

| Column          | Type         | Nullable | Description                     |
| --------------- | ------------ | -------- | ------------------------------- |
| rating_id       | INT (PK, AI) | No       | Unique rating record            |
| order_id        | INT (FK)     | No       | Rated order                     |
| customer_id     | INT (FK)     | No       | Rating customer                 |
| restaurant_id   | INT (FK)     | No       | Rated restaurant                |
| food_rating     | TINYINT      | No       | Food quality rating (1–5)       |
| delivery_rating | TINYINT      | Yes      | Delivery rating (1–5), nullable |
| review_text     | VARCHAR(500) | Yes      | Written review                  |
| rated_at        | DATETIME     | No       | Rating submission timestamp     |

