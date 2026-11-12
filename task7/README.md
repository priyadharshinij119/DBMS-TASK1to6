<img width="1535" height="815" alt="Screenshot 2026-09-22 182512" src="https://github.com/user-attachments/assets/4225a1af-05f2-48e0-8fd1-d60c75da18bb" />
<img width="1535" height="813" alt="Screenshot 2026-09-22 182626" src="https://github.com/user-attachments/assets/502210fd-71e2-41f8-8796-fbb73d2de607" />
<img width="1532" height="816" alt="Screenshot 2026-09-22 182636" src="https://github.com/user-attachments/assets/39180c71-3c18-4bb6-a796-bf5ff579dd06" />
<img width="1535" height="817" alt="Screenshot 2026-09-22 182646" src="https://github.com/user-attachments/assets/ed67ffc1-3730-41f6-89c4-8febaeaf1b3c" />
<img width="1535" height="813" alt="Screenshot 2026-09-22 182652" src="https://github.com/user-attachments/assets/c6afb749-8f41-4e5f-aee2-b4c3fcdcc07c" />
<img width="1535" height="810" alt="Screenshot 2026-09-22 182716" src="https://github.com/user-attachments/assets/dd211513-3892-4dc2-a4c7-93f3f7ded973" />
<img width="1531" height="787" alt="Screenshot 2026-09-22 182725" src="https://github.com/user-attachments/assets/134952d3-548e-45c2-9b34-921710a6561a" />
<img width="1535" height="807" alt="Screenshot 2026-09-22 182735" src="https://github.com/user-attachments/assets/0622fd0b-6f6b-40a6-bcc6-d50cb75a3b0b" />
<img width="1497" height="851" alt="Screenshot 2026-09-22 182743" src="https://github.com/user-attachments/assets/aabf06a6-6bc4-4ce0-a494-a2ae5f3d13b9" />
<img width="1523" height="782" alt="Screenshot 2026-09-22 182750" src="https://github.com/user-attachments/assets/f80618c6-2382-40f7-9d85-627450b1b827" />
<img width="1535" height="802" alt="Screenshot 2026-09-22 182800" src="https://github.com/user-attachments/assets/cc3b47c4-db01-4e2e-9cc4-24906da8c5ca" />
<img width="1535" height="808" alt="Screenshot 2026-09-22 182809" src="https://github.com/user-attachments/assets/af356e65-c29c-4073-8a2e-16beb5ec1844" />
<img width="1530" height="800" alt="Screenshot 2026-09-22 182817" src="https://github.com/user-attachments/assets/9f64717e-835f-47a1-8db3-d8ed82cf700a" />
<img width="1531" height="798" alt="Screenshot 2026-09-22 182826" src="https://github.com/user-attachments/assets/d285cd6d-18bd-45cb-aad8-622fa28613f9" />
<img width="1533" height="808" alt="Screenshot 2026-09-22 182836" src="https://github.com/user-attachments/assets/bccc967b-9379-4e80-8038-ce14e81040fd" />
<img width="1535" height="797" alt="Screenshot 2026-09-22 182844" src="https://github.com/user-attachments/assets/8c544396-c40a-40af-80bb-43dfcdec2a54" />
USE ecommerce_db;

SELECT * FROM Product;
SELECT product_id, product_name, price, stock FROM Product;

SELECT product_id, product_name, price FROM Product
WHERE price BETWEEN 500 AND 2000;

SELECT p.product_id, p.product_name, c.category_name, p.price
FROM Product p JOIN Category c ON p.category_id = c.category_id
WHERE c.category_name = 'Electronics';

SELECT product_id, product_name, price, stock FROM Product
WHERE stock > 0;

SELECT product_id, product_name, price, stock FROM Product
WHERE stock = 0;

SELECT product_id, product_name, price FROM Product
ORDER BY price ASC;

SELECT product_id, product_name, price FROM Product
ORDER BY price DESC;

SELECT DISTINCT c.category_name
FROM Product p JOIN Category c ON p.category_id = c.category_id
WHERE p.stock > 0
ORDER BY c.category_name;

SELECT product_id, product_name, price, stock FROM Product
WHERE price >= 1000 ORDER BY price DESC;

SELECT p.product_id, p.product_name, c.category_name, p.price, p.stock
FROM Product p JOIN Category c ON p.category_id = c.category_id
WHERE c.category_name = 'Electronics' AND p.stock > 0
ORDER BY p.price DESC;

SELECT customer_id, customer_name, email, phone, city
FROM Customer ORDER BY customer_name;

SELECT customer_id, customer_name, email, phone
FROM Customer WHERE city = 'Chennai';

SELECT p.product_id, p.product_name, c.category_name, p.price, p.stock
FROM Product p JOIN Category c ON p.category_id = c.category_id
ORDER BY c.category_name, p.product_name;

SELECT product_id, product_name, price, stock
FROM Product
WHERE price < 2000 AND stock >= 25
ORDER BY price ASC;

SELECT product_id, product_name, price FROM Product
WHERE product_name LIKE '%Book%';

SELECT c.category_name,
       COUNT(p.product_id) AS product_count,
       COALESCE(SUM(p.stock), 0) AS total_stock
FROM Category c LEFT JOIN Product p ON c.category_id = p.category_id
GROUP BY c.category_id, c.category_name
ORDER BY product_count DESC;

SELECT c.customer_id, c.customer_name,
       COUNT(o.order_id) AS total_orders,
       COALESCE(SUM(o.total_amount), 0) AS total_spent
FROM Customer c LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_spent DESC;

SELECT payment_status,
       COUNT(payment_id) AS transaction_count,
       SUM(amount) AS total_amount
FROM Payment
GROUP BY payment_status
ORDER BY transaction_count DESC;

