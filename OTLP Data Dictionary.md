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
