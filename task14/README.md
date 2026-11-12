<img width="1535" height="812" alt="Screenshot 2026-11-11 191139" src="https://github.com/user-attachments/assets/67728fcd-d9a0-430c-9332-7155449d3298" />
<img width="1535" height="815" alt="Screenshot 2026-11-11 191235" src="https://github.com/user-attachments/assets/e2b56756-8577-4fb2-901b-380abd32499e" />
<img width="1527" height="817" alt="Screenshot 2026-11-11 191242" src="https://github.com/user-attachments/assets/4ba5a76a-c8fd-4b28-aea5-cc93d0330d6b" />
<img width="1500" height="817" alt="Screenshot 2026-11-11 191251" src="https://github.com/user-attachments/assets/72c9e3ab-57a4-4020-a520-96a6a51ac6ad" />
<img width="1515" height="810" alt="Screenshot 2026-11-11 191258" src="https://github.com/user-attachments/assets/8e1bc7f6-0bf1-471e-804a-46aca3923f84" />
<img width="1533" height="816" alt="Screenshot 2026-11-11 191304" src="https://github.com/user-attachments/assets/e2ab2a6e-51f9-4b4f-9c0c-219196844935" />
<img width="1522" height="810" alt="Screenshot 2026-11-11 191311" src="https://github.com/user-attachments/assets/b29fcad8-5acc-464d-8def-fce73d7b6446" />
<img width="1535" height="808" alt="Screenshot 2026-11-11 191318" src="https://github.com/user-attachments/assets/9567ecca-4409-4143-921d-76ef3db7714c" />
<img width="1532" height="816" alt="Screenshot 2026-11-11 191325" src="https://github.com/user-attachments/assets/fe7a7f4a-ab11-4dba-b7f3-69724e49c36c" />
<img width="1532" height="817" alt="Screenshot 2026-11-11 191334" src="https://github.com/user-attachments/assets/49c280e2-d1db-4a4b-832f-6c2afe623ba8" />
<img width="1535" height="811" alt="Screenshot 2026-11-11 191341" src="https://github.com/user-attachments/assets/ce2047a3-1303-47a8-acde-336c31e42626" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 191351" src="https://github.com/user-attachments/assets/94a8682b-7d6a-407b-99b7-262fa9fb9bd1" />
<img width="1535" height="817" alt="Screenshot 2026-11-11 191359" src="https://github.com/user-attachments/assets/3b44468e-006f-4115-860a-5b9ef15d6b5c" />
CREATE DATABASE IF NOT EXISTS ecommerce_db;
USE ecommerce_db;

WITH CustomerMetrics AS (
    SELECT
        c.customer_id,
        c.customer_name,
        c.city,
        COUNT(DISTINCT o.order_id) AS total_orders,
        COALESCE(SUM(o.total_amount), 0.00) AS total_spend,
        COALESCE(AVG(o.total_amount), 0.00) AS average_order_value,
        MAX(o.order_date) AS last_order_date,
        DATEDIFF(CURRENT_DATE, MAX(o.order_date)) AS days_since_last_order
    FROM Customer c
    LEFT JOIN Orders o ON c.customer_id = o.customer_id
    GROUP BY c.customer_id, c.customer_name, c.city
)
SELECT
    customer_id,
    customer_name,
    city,
    total_orders,
    total_spend,
    average_order_value,
    last_order_date,
    days_since_last_order,
    CASE
        WHEN total_spend >= 5000 THEN 'HIGH_VALUE'
        WHEN total_spend >= 2000 THEN 'MEDIUM_VALUE'
        WHEN total_spend > 0 THEN 'LOW_VALUE'
        ELSE 'NO_PURCHASE'
    END AS customer_segment
FROM CustomerMetrics
ORDER BY total_spend DESC;

WITH ProductMetrics AS (
    SELECT
        p.product_id,
        p.product_name,
        p.category_id,
        p.price,
        p.stock,
        COALESCE(SUM(od.quantity), 0) AS total_quantity_sold,
        COALESCE(SUM(od.total_price), 0.00) AS total_revenue
    FROM Product p
    LEFT JOIN Order_Details od ON p.product_id = od.product_id
    GROUP BY p.product_id, p.product_name, p.category_id, p.price, p.stock
)
SELECT
    product_id,
    product_name,
    category_id,
    price,
    stock,
    total_quantity_sold,
    total_revenue,
    RANK() OVER (ORDER BY total_revenue DESC) AS revenue_rank,
    ROW_NUMBER() OVER (ORDER BY total_quantity_sold DESC, product_id) AS quantity_row_number
FROM ProductMetrics
ORDER BY revenue_rank, product_id;

WITH CustomerMetrics AS (
    SELECT
        c.customer_id,
        c.customer_name,
        COUNT(DISTINCT o.order_id) AS total_orders,
        COALESCE(SUM(o.total_amount), 0.00) AS total_spend,
        COALESCE(AVG(o.total_amount), 0.00) AS average_order_value,
        MAX(o.order_date) AS last_order_date,
        DATEDIFF(CURRENT_DATE, MAX(o.order_date)) AS days_since_last_order
    FROM Customer c
    LEFT JOIN Orders o ON c.customer_id = o.customer_id
    GROUP BY c.customer_id, c.customer_name
)
SELECT
    customer_id,
    customer_name,
    total_orders,
    total_spend,
    average_order_value,
    last_order_date,
    days_since_last_order,
    RANK() OVER (ORDER BY total_spend DESC) AS spending_rank,
    ROW_NUMBER() OVER (ORDER BY total_spend DESC, customer_id) AS customer_row_number
FROM CustomerMetrics
ORDER BY spending_rank, customer_id;

WITH CustomerFeatures AS (
    SELECT
        c.customer_id,
        c.customer_name,
        c.city,
        COUNT(DISTINCT o.order_id) AS order_count,
        COALESCE(SUM(o.total_amount), 0.00) AS total_spend,
        COALESCE(AVG(o.total_amount), 0.00) AS average_order_value,
        COALESCE(MAX(o.total_amount), 0.00) AS maximum_order_value,
        MAX(o.order_date) AS last_order_date,
        COALESCE(DATEDIFF(CURRENT_DATE, MAX(o.order_date)), -1) AS days_since_last_order
    FROM Customer c
    LEFT JOIN Orders o ON c.customer_id = o.customer_id
    GROUP BY c.customer_id, c.customer_name, c.city
)
SELECT
    customer_id,
    customer_name,
    city,
    order_count,
    total_spend,
    average_order_value,
    maximum_order_value,
    last_order_date,
    days_since_last_order,
    CASE WHEN order_count > 0 THEN 1 ELSE 0 END AS has_purchased,
    CASE
        WHEN total_spend >= 5000 THEN 'HIGH_VALUE'
        WHEN total_spend >= 2000 THEN 'MEDIUM_VALUE'
        WHEN total_spend > 0 THEN 'LOW_VALUE'
        ELSE 'NO_PURCHASE'
    END AS customer_segment
FROM CustomerFeatures
ORDER BY customer_id;

SELECT
    p.product_id,
    p.product_name,
    p.category_id,
    p.price,
    p.stock,
    CASE WHEN p.stock > 0 THEN 1 ELSE 0 END AS is_available,
    COALESCE(sales.total_quantity_sold, 0) AS total_quantity_sold,
    COALESCE(sales.total_revenue, 0.00) AS total_revenue,
    COALESCE(sales.average_unit_price, 0.00) AS average_unit_price
FROM Product p
LEFT JOIN (
    SELECT
        product_id,
        SUM(quantity) AS total_quantity_sold,
        SUM(total_price) AS total_revenue,
        AVG(unit_price) AS average_unit_price
    FROM Order_Details
    GROUP BY product_id
) sales ON p.product_id = sales.product_id
ORDER BY p.product_id;

SELECT
    o.order_id,
    o.customer_id,
    o.order_date,
    o.total_amount,
    COUNT(od.order_detail_id) AS distinct_line_items,
    COALESCE(SUM(od.quantity), 0) AS total_items,
    COALESCE(AVG(od.unit_price), 0.00) AS average_item_price,
    DATEDIFF(CURRENT_DATE, o.order_date) AS days_since_order,
    CASE WHEN o.total_amount >= 2000 THEN 1 ELSE 0 END AS is_large_order
FROM Orders o
LEFT JOIN Order_Details od ON o.order_id = od.order_id
GROUP BY o.order_id, o.customer_id, o.order_date, o.total_amount
ORDER BY o.order_date DESC, o.order_id;

