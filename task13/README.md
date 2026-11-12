<img width="727" height="802" alt="Screenshot 2026-11-11 192609" src="https://github.com/user-attachments/assets/fdf5d824-3a06-40ea-994f-273bc49866cb" />
<img width="755" height="817" alt="Screenshot 2026-11-11 193035" src="https://github.com/user-attachments/assets/ab2edc18-f723-4664-bbc5-c39e7f0186bf" />
<img width="1535" height="811" alt="Screenshot 2026-11-11 194057" src="https://github.com/user-attachments/assets/d0dbe7f0-e547-41fb-9813-7421b6610e0b" />
<img width="1535" height="813" alt="Screenshot 2026-11-11 194104" src="https://github.com/user-attachments/assets/e5c1101f-7a0e-4875-b6c8-8f6c55376bdc" />
<img width="1535" height="813" alt="Screenshot 2026-11-11 194112" src="https://github.com/user-attachments/assets/b366cff8-63ce-42ee-a2fe-5d98171a4222" />
<img width="1487" height="823" alt="Screenshot 2026-11-11 194119" src="https://github.com/user-attachments/assets/1de06d9b-c298-4048-8846-e6fddd8ec6e6" />
<img width="1513" height="817" alt="Screenshot 2026-11-11 194132" src="https://github.com/user-attachments/assets/a0815490-cb57-45d7-8a2c-0c7ed1dbc21c" />
<img width="1535" height="817" alt="Screenshot 2026-11-11 194151" src="https://github.com/user-attachments/assets/4858af75-37d5-4841-ba61-7e8be3e81a58" />
<img width="1535" height="818" alt="Screenshot 2026-11-11 194159" src="https://github.com/user-attachments/assets/5071a9f7-99ab-49ea-9daa-cf1d32159245" />
<img width="1533" height="816" alt="Screenshot 2026-11-11 194126" src="https://github.com/user-attachments/assets/f140fed4-19b4-4c74-9750-d2cc79013cec" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 194143" src="https://github.com/user-attachments/assets/904ced7c-79d5-4feb-b114-38502041bfa6" />
CREATE DATABASE IF NOT EXISTS ecommerce_db;
USE ecommerce_db;

CREATE ROLE IF NOT EXISTS 'ecommerce_readonly';
CREATE ROLE IF NOT EXISTS 'ecommerce_data_entry';

GRANT SELECT ON ecommerce_db.Customer TO 'ecommerce_readonly';
GRANT SELECT ON ecommerce_db.Category TO 'ecommerce_readonly';
GRANT SELECT ON ecommerce_db.Product TO 'ecommerce_readonly';
GRANT SELECT ON ecommerce_db.Orders TO 'ecommerce_readonly';
GRANT SELECT ON ecommerce_db.Order_Details TO 'ecommerce_readonly';
GRANT SELECT ON ecommerce_db.Payment TO 'ecommerce_readonly';

GRANT SELECT, INSERT, UPDATE ON ecommerce_db.Customer TO 'ecommerce_data_entry';
GRANT SELECT, INSERT, UPDATE ON ecommerce_db.Product TO 'ecommerce_data_entry';
GRANT SELECT, INSERT, UPDATE ON ecommerce_db.Orders TO 'ecommerce_data_entry';
GRANT SELECT, INSERT, UPDATE ON ecommerce_db.Order_Details TO 'ecommerce_data_entry';
GRANT SELECT, INSERT, UPDATE ON ecommerce_db.Payment TO 'ecommerce_data_entry';

CREATE USER IF NOT EXISTS 'ecommerce_viewer'@'localhost'
IDENTIFIED BY 'ChangeThisViewer_123!';

CREATE USER IF NOT EXISTS 'ecommerce_operator'@'localhost'
IDENTIFIED BY 'ChangeThisOperator_123!';

GRANT 'ecommerce_readonly' TO 'ecommerce_viewer'@'localhost';
GRANT 'ecommerce_data_entry' TO 'ecommerce_operator'@'localhost';

SET DEFAULT ROLE 'ecommerce_readonly' TO 'ecommerce_viewer'@'localhost';
SET DEFAULT ROLE 'ecommerce_data_entry' TO 'ecommerce_operator'@'localhost';

REVOKE INSERT ON ecommerce_db.Payment FROM 'ecommerce_data_entry';

CREATE INDEX idx_customer_city ON Customer(city);
CREATE INDEX idx_product_name ON Product(product_name);
CREATE INDEX idx_product_category_price ON Product(category_id, price);
CREATE INDEX idx_orders_customer_date ON Orders(customer_id, order_date);
CREATE INDEX idx_orders_status ON Orders(order_status);
CREATE INDEX idx_order_details_product ON Order_Details(product_id);
CREATE INDEX idx_payment_status_date ON Payment(payment_status, payment_date);

EXPLAIN
SELECT product_id, product_name, price
FROM Product
WHERE product_name = 'Wireless Headphones';

EXPLAIN
SELECT product_id, product_name, price
FROM Product
WHERE category_id = 1 AND price < 2000;

EXPLAIN
SELECT order_id, order_date, total_amount, order_status
FROM Orders
WHERE customer_id = 1
ORDER BY order_date DESC;

EXPLAIN
SELECT payment_id, order_id, amount, payment_status
FROM Payment
WHERE payment_status = 'SUCCESS'
ORDER BY payment_date DESC;

SHOW INDEX FROM Customer;
SHOW INDEX FROM Product;
SHOW INDEX FROM Orders;
SHOW INDEX FROM Order_Details;
SHOW INDEX FROM Payment;

SHOW GRANTS FOR 'ecommerce_viewer'@'localhost';
SHOW GRANTS FOR 'ecommerce_operator'@'localhost';
SHOW GRANTS FOR 'ecommerce_readonly';
SHOW GRANTS FOR 'ecommerce_data_entry';

