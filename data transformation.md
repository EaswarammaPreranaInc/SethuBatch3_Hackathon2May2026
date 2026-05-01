

| #  | Table       | Column         | Dirty Pattern                                            | 
| -- | ----------- | -------------- | -------------------------------------------------------- |
| 1  | customers   | phone          | Too-short numbers (8 digits), leading "0", "+91" prefix  | 
| 2  | customers   | gender         | Mixed case: "male", "MALE", "female", "FEMALE", "m", "f" | 
| 3  | customers   | gender         | NULL values                                              | 
| 4  | customers   | date_of_birth  | Future dates (impossible DOB)                            |
| 5  | customers   | city           | ALL CAPS or all lowercase city names                     | 
| 6  | restaurants | city           | ALL CAPS or all lowercase city names                     | 
| 7  | menu_items  | price          | Zero or negative prices                                  | 
| 8  | orders      | total_amount   | Negative total amounts                                   | 
| 9  | payments    | payment_status | FAILED / PENDING with no paid_at                         | 
| 10 | deliveries  | delivered_at   | NULL for DELIVERED status records                        |
| 11 | ratings     | food_rating    | Out-of-range values possible in edge cases               |
