
<img width="1535" height="815" alt="Screenshot 2026-11-11 195246" src="https://github.com/user-attachments/assets/7b8884df-5790-41dd-9d5b-33b2cf6d6f4b" />
<img width="1520" height="817" alt="Screenshot 2026-11-11 195308" src="https://github.com/user-attachments/assets/b4578552-5529-4620-9ed4-a30d15106404" />
<img width="1521" height="813" alt="Screenshot 2026-11-11 195316" src="https://github.com/user-attachments/assets/f0e5e0ed-61fd-417e-a56a-f946a9505fc4" />
<img width="1530" height="810" alt="Screenshot 2026-11-11 195327" src="https://github.com/user-attachments/assets/81697322-c51f-4da4-a8c7-669ba54c0715" />
<img width="1535" height="811" alt="Screenshot 2026-11-11 195334" src="https://github.com/user-attachments/assets/b17e2fad-b4a8-4e45-b1a0-bfd10b501d18" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 195346" src="https://github.com/user-attachments/assets/f26e755d-9e5e-40e8-91f8-03a40b989151" />
<img width="1535" height="810" alt="Screenshot 2026-11-11 195355" src="https://github.com/user-attachments/assets/fc270daf-25db-40d3-86ab-ff1cedd3544d" />

USE ecommerce_db;

SELECT c.customer_id, c.customer_name, o.order_id, o.order_date,
       o.order_status, p.product_name, od.quantity,
       od.unit_price, od.total_price, py.payment_mode,
       py.payment_status, py.amount
FROM Customer c
INNER JOIN Orders o ON c.customer_id = o.customer_id
INNER JOIN Order_Details od ON o.order_id = od.order_id
INNER JOIN Product p ON od.product_id = p.product_id
INNER JOIN Payment py ON o.order_id = py.order_id
ORDER BY o.order_id;

SELECT c.customer_id, c.customer_name, o.order_id,
       o.order_date, o.total_amount, o.order_status
FROM Customer c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id, o.order_date DESC;

SELECT c.customer_id, c.customer_name, o.order_id,
       o.order_date, o.total_amount, o.order_status
FROM Customer c
RIGHT JOIN Orders o ON c.customer_id = o.customer_id
ORDER BY o.order_id;

SELECT o.order_id, o.order_date, o.order_status,
       c.customer_name, c.email, p.product_name,
       od.quantity, od.unit_price, od.total_price,
       py.payment_mode, py.payment_date,
       py.payment_status, py.amount
FROM Orders o
INNER JOIN Customer c ON o.customer_id = c.customer_id
INNER JOIN Order_Details od ON o.order_id = od.order_id
INNER JOIN Product p ON od.product_id = p.product_id
LEFT JOIN Payment py ON o.order_id = py.order_id
ORDER BY o.order_date DESC, o.order_id;

SELECT c.customer_id, c.customer_name,
       o.order_id, o.order_date, o.order_status,
       p.product_name, od.quantity, od.total_price
FROM Customer c
INNER JOIN Orders o ON c.customer_id = o.customer_id
INNER JOIN Order_Details od ON o.order_id = od.order_id
INNER JOIN Product p ON od.product_id = p.product_id
ORDER BY c.customer_id, o.order_date DESC;

SELECT c.customer_id, c.customer_name,
       COUNT(DISTINCT o.order_id) AS total_orders,
       COALESCE(SUM(o.total_amount), 0) AS total_purchase
FROM Customer c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_purchase DESC;

SELECT p.product_id, p.product_name,
       COUNT(DISTINCT od.order_id) AS total_orders,
       COALESCE(SUM(od.quantity), 0) AS total_quantity_sold,
       COALESCE(SUM(od.total_price), 0) AS total_sales
FROM Product p
LEFT JOIN Order_Details od ON p.product_id = od.product_id
GROUP BY p.product_id, p.product_name
ORDER BY total_sales DESC;

SELECT o.order_id, c.customer_name,
       o.total_amount, py.payment_mode,
       py.payment_status
FROM Orders o
INNER JOIN Customer c ON o.customer_id = c.customer_id
LEFT JOIN Payment py ON o.order_id = py.order_id
ORDER BY o.order_id;
