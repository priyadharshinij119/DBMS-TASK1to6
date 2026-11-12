<img width="1533" height="815" alt="Screenshot 2026-09-08 091134" src="https://github.com/user-attachments/assets/bd038128-23c9-4bb7-9f86-d245aa7e8ae7" />
<img width="1535" height="815" alt="Screenshot 2026-09-08 091157" src="https://github.com/user-attachments/assets/2a4e65d3-ac6c-4cd2-b664-f0624fc7770e" />
<img width="1522" height="817" alt="Screenshot 2026-09-08 091217" src="https://github.com/user-attachments/assets/df331cca-a543-47f0-920a-049be92ee25a" />
<img width="1527" height="813" alt="Screenshot 2026-09-08 091231" src="https://github.com/user-attachments/assets/e39224eb-d974-4854-98ab-d3aef168ac41" />
<img width="1535" height="812" alt="Screenshot 2026-09-08 091255" src="https://github.com/user-attachments/assets/a1ab4574-ab25-4eb5-9075-c6f8a1cccd70" />
<img width="1535" height="813" alt="Screenshot 2026-09-08 091329" src="https://github.com/user-attachments/assets/20402724-5161-447a-bdf7-81490be3fa1e" />
<img width="1533" height="817" alt="Screenshot 2026-09-08 091341" src="https://github.com/user-attachments/assets/845a48ce-b46d-485d-b56b-cd8200b622cf" />
<img width="1532" height="817" alt="Screenshot 2026-09-08 091354" src="https://github.com/user-attachments/assets/94f18d5d-3e7b-4d2d-8be4-c186c91b6096" />
<img width="1533" height="812" alt="Screenshot 2026-09-08 091420" src="https://github.com/user-attachments/assets/00ac1220-3d68-4669-b4c9-da4475c2616d" />
<img width="1530" height="817" alt="Screenshot 2026-09-08 134618" src="https://github.com/user-attachments/assets/6ca54d77-9a17-4513-82ee-a918ff166b6e" />
<img width="957" height="807" alt="Screenshot 2026-09-08 134807" src="https://github.com/user-attachments/assets/10d0b29e-61b2-49ed-b518-b9b6e8501197" />
<img width="1518" height="805" alt="Screenshot 2026-09-08 135011" src="https://github.com/user-attachments/assets/06c92b94-c6c9-470d-9418-3ab6196b5414" />
<img width="1535" height="810" alt="Screenshot 2026-09-08 091308" src="https://github.com/user-attachments/assets/057b204e-978a-4e5c-96ff-edf75ba93760" />

CREATE DATABASE IF NOT EXISTS ecommerce_db;
USE ecommerce_db;

CREATE TABLE Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    total_amount DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    order_status VARCHAR(30) NOT NULL DEFAULT 'PLACED',
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    CHECK (total_amount >= 0)
);

CREATE TABLE Order_Details (
    order_detail_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    total_price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Product(product_id),
    CHECK (quantity > 0), CHECK (unit_price >= 0), CHECK (total_price >= 0)
);

INSERT INTO Orders (customer_id, total_amount, order_status) VALUES
(1, 2098.00, 'PLACED'), (2, 2499.00, 'PLACED');

INSERT INTO Order_Details (order_id, product_id, quantity, unit_price, total_price) VALUES
(1, 1, 1, 1499.00, 1499.00),
(1, 3, 1, 599.00, 599.00),
(2, 2, 1, 2499.00, 2499.00);

UPDATE Orders SET order_status = 'CONFIRMED' WHERE order_id = 1;

SELECT c.customer_id, c.customer_name, o.order_id, o.order_date, o.order_status,
       p.product_name, od.quantity, od.unit_price, od.total_price
FROM Customer c
JOIN Orders o ON c.customer_id = o.customer_id
JOIN Order_Details od ON o.order_id = od.order_id
JOIN Product p ON od.product_id = p.product_id
ORDER BY c.customer_id, o.order_date DESC, o.order_i

