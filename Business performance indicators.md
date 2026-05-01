

## 📊 KPI Queries

This file contains 20 key SQL KPIs for a food delivery platform.

---

## KPI 1: Total Revenue

```sql
SELECT
    ROUND(SUM(o.total_amount), 2) AS total_revenue_inr,
    COUNT(*) AS total_delivered_orders
FROM orders o
WHERE o.order_status = 'DELIVERED';
```

---

## KPI 2: Monthly Revenue Trend

```sql
SELECT
    DATE_FORMAT(ordered_at, '%Y-%m') AS order_month,
    COUNT(*) AS num_orders,
    ROUND(SUM(total_amount), 2) AS monthly_revenue
FROM orders
WHERE order_status = 'DELIVERED'
GROUP BY order_month
ORDER BY order_month;
```

---

## KPI 3: Revenue by City

```sql
SELECT
    r.city,
    COUNT(o.order_id) AS num_orders,
    ROUND(SUM(o.total_amount), 2) AS city_revenue,
    ROUND(AVG(o.total_amount), 2) AS avg_order_value
FROM orders o
JOIN restaurants r ON r.restaurant_id = o.restaurant_id
WHERE o.order_status = 'DELIVERED'
GROUP BY r.city
ORDER BY city_revenue DESC;
```

---

## KPI 4: Top Restaurants

```sql
SELECT
    r.restaurant_id,
    r.name AS restaurant_name,
    r.city,
    COUNT(o.order_id) AS total_orders,
    ROUND(SUM(o.total_amount), 2) AS total_revenue
FROM orders o
JOIN restaurants r ON r.restaurant_id = o.restaurant_id
WHERE o.order_status = 'DELIVERED'
GROUP BY r.restaurant_id, r.name, r.city
ORDER BY total_revenue DESC
LIMIT 10;
```

---

## KPI 5: Revenue by Cuisine

```sql
SELECT
    TRIM(SUBSTRING_INDEX(r.cuisine_type, ',', 1)) AS primary_cuisine,
    COUNT(o.order_id) AS num_orders,
    ROUND(SUM(o.total_amount), 2) AS cuisine_revenue
FROM orders o
JOIN restaurants r ON r.restaurant_id = o.restaurant_id
WHERE o.order_status = 'DELIVERED'
GROUP BY primary_cuisine
ORDER BY cuisine_revenue DESC;
```

---

## KPI 6: AOV by Payment Method

```sql
SELECT
    p.payment_method,
    COUNT(p.payment_id) AS num_payments,
    ROUND(AVG(p.amount), 2) AS avg_order_value
FROM payments p
WHERE p.payment_status = 'SUCCESS'
GROUP BY p.payment_method
ORDER BY avg_order_value DESC;
```

---

## KPI 7: Payment Distribution

```sql
SELECT
    payment_method,
    COUNT(*) AS count,
    ROUND(100.0 * COUNT(*) / SUM(COUNT(*)) OVER (), 2) AS pct_share
FROM payments
WHERE payment_status = 'SUCCESS'
GROUP BY payment_method
ORDER BY count DESC;
```

---

## KPI 8: Cancellation Rate

```sql
SELECT
    r.city,
    COUNT(o.order_id) AS total_orders,
    SUM(CASE WHEN o.order_status = 'CANCELLED' THEN 1 ELSE 0 END) AS cancelled_orders,
    ROUND(
        100.0 * SUM(CASE WHEN o.order_status = 'CANCELLED' THEN 1 ELSE 0 END)
        / COUNT(o.order_id), 2
    ) AS cancellation_rate_pct
FROM orders o
JOIN restaurants r ON r.restaurant_id = o.restaurant_id
GROUP BY r.city;
```

---

## KPI 9: Delivery Time

```sql
SELECT
    r.city,
    ROUND(AVG(
        TIMESTAMPDIFF(MINUTE, d.picked_up_at, d.delivered_at)
    ), 1) AS avg_delivery_minutes
FROM deliveries d
JOIN orders o ON o.order_id = d.order_id
JOIN restaurants r ON r.restaurant_id = o.restaurant_id
WHERE d.delivery_status = 'DELIVERED'
GROUP BY r.city;
```

---

## KPI 10: Fastest Delivery Partners

```sql
SELECT
    dp.partner_id,
    dp.full_name,
    ROUND(AVG(
        TIMESTAMPDIFF(MINUTE, d.picked_up_at, d.delivered_at)
    ), 1) AS avg_delivery_minutes
FROM deliveries d
JOIN delivery_partners dp ON dp.partner_id = d.partner_id
WHERE d.delivery_status = 'DELIVERED'
GROUP BY dp.partner_id, dp.full_name
ORDER BY avg_delivery_minutes ASC
LIMIT 10;
```

---

## KPI 11: Avg Food Rating

```sql
SELECT
    r.name,
    ROUND(AVG(rt.food_rating), 2) AS avg_food_rating
FROM ratings rt
JOIN restaurants r ON r.restaurant_id = rt.restaurant_id
GROUP BY r.name;
```

---

## KPI 12: Hourly Orders

```sql
SELECT
    HOUR(ordered_at) AS order_hour,
    COUNT(*) AS num_orders
FROM orders
GROUP BY order_hour;
```

---

## KPI 13: Orders by Day

```sql
SELECT
    DAYNAME(ordered_at) AS day,
    COUNT(*) AS num_orders
FROM orders
GROUP BY day;
```

---

## KPI 14: Repeat Customers

```sql
SELECT
    COUNT(DISTINCT customer_id) AS total_customers,
    SUM(CASE WHEN order_count > 1 THEN 1 ELSE 0 END) AS repeat_customers
FROM (
    SELECT customer_id, COUNT(*) AS order_count
    FROM orders
    WHERE order_status = 'DELIVERED'
    GROUP BY customer_id
) t;
```

---

## KPI 15: Offers Impact

```sql
SELECT
    of.offer_code,
    COUNT(o.order_id) AS usage_count,
    SUM(o.discount_amount) AS total_discount
FROM orders o
JOIN offers of ON of.offer_id = o.offer_id
GROUP BY of.offer_code;
```

---

## KPI 16: Customer Lifetime Value

```sql
SELECT
    c.customer_id,
    SUM(o.total_amount) AS lifetime_value
FROM orders o
JOIN customers c ON c.customer_id = o.customer_id
WHERE o.order_status = 'DELIVERED'
GROUP BY c.customer_id;
```

---

## KPI 17: Top Items

```sql
SELECT
    mi.item_name,
    SUM(oi.quantity) AS total_units
FROM order_items oi
JOIN menu_items mi ON mi.item_id = oi.item_id
GROUP BY mi.item_name
ORDER BY total_units DESC;
```

---

## KPI 18: Restaurant Ranking

```sql
SELECT
    r.city,
    r.name,
    RANK() OVER (PARTITION BY r.city ORDER BY AVG(rt.food_rating) DESC) AS rank
FROM ratings rt
JOIN restaurants r ON r.restaurant_id = rt.restaurant_id
GROUP BY r.city, r.name;
```

---

## KPI 19: New vs Returning Customers

```sql
WITH first_order AS (
    SELECT customer_id, MIN(ordered_at) AS first_order
    FROM orders
    GROUP BY customer_id
)
SELECT COUNT(*) FROM first_order;
```

---

## KPI 20: Delivery Efficiency Score

```sql
SELECT
    dp.partner_id,
    ROUND(AVG(d.delivery_rating)*10 - AVG(TIMESTAMPDIFF(MINUTE, d.picked_up_at, d.delivered_at))*0.1, 2) AS score
FROM deliveries d
JOIN delivery_partners dp ON dp.partner_id = d.partner_id
GROUP BY dp.partner_id;
```
