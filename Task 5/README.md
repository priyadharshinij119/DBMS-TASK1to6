<img width="1532" height="815" alt="Screenshot 2026-09-09 105559" src="https://github.com/user-attachments/assets/e4156d6f-6923-4da7-8948-6428dd325208" />
<img width="1522" height="810" alt="Screenshot 2026-09-09 105643" src="https://github.com/user-attachments/assets/a27f2bd0-29f7-48f0-b339-c2372187325a" />
<img width="1535" height="817" alt="Screenshot 2026-09-09 105652" src="https://github.com/user-attachments/assets/e69394ca-3a01-4089-9859-7afde725b196" />
<img width="1533" height="813" alt="Screenshot 2026-09-09 105703" src="https://github.com/user-attachments/assets/e91c9345-4e14-4d44-8252-d1dbe54f9824" />
<img width="1535" height="807" alt="Screenshot 2026-09-09 105716" src="https://github.com/user-attachments/assets/2b4f8742-6b78-4b55-8900-53d4045acb18" />
<img width="1527" height="817" alt="Screenshot 2026-09-09 105731" src="https://github.com/user-attachments/assets/b4829004-8c9f-4a24-99b3-11f2fa0376b6" />
<img width="1527" height="812" alt="Screenshot 2026-09-09 105741" src="https://github.com/user-attachments/assets/4bd63821-fb9f-4bfe-a11b-f714e1229764" />
<img width="1522" height="812" alt="Screenshot 2026-09-09 105757" src="https://github.com/user-attachments/assets/c602af65-0332-455a-a205-01b8de6fa2fe" />
<img width="1532" height="811" alt="Screenshot 2026-09-09 105809" src="https://github.com/user-attachments/assets/bd7c8428-0709-4532-b007-5296c5d15cd3" />
<img width="1535" height="820" alt="Screenshot 2026-09-09 105819" src="https://github.com/user-attachments/assets/26b59b60-bd3d-4241-a644-49bda10a6e82" />
<img width="1535" height="811" alt="Screenshot 2026-09-09 105831" src="https://github.com/user-attachments/assets/3925ae9c-36b5-4963-a55c-33dbc0deb2e6" />
<img width="1535" height="813" alt="Screenshot 2026-09-09 105843" src="https://github.com/user-attachments/assets/6c5a4f80-297a-4d27-87df-79c70205faed" />

CREATE DATABASE IF NOT EXISTS ecommerce_db;
USE ecommerce_db;

CREATE TABLE Payment (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    payment_mode VARCHAR(30) NOT NULL,
    payment_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    amount DECIMAL(10,2) NOT NULL,
    payment_status VARCHAR(30) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    CHECK (amount >= 0),
    CHECK (payment_mode IN ('UPI', 'CARD', 'NET_BANKING', 'CASH_ON_DELIVERY', 'WALLET')),
    CHECK (payment_status IN ('SUCCESS', 'FAILED', 'PENDING'))
);

INSERT INTO Payment (order_id, payment_mode, amount, payment_status) VALUES
(1, 'UPI', 2098.00, 'SUCCESS'),
(2, 'CARD', 2499.00, 'FAILED');

SELECT p.payment_id, o.order_id, c.customer_name, p.payment_mode,
       p.payment_date, p.amount, p.payment_status
FROM Payment p
JOIN Orders o ON p.order_id = o.order_id
JOIN Customer c ON o.customer_id = c.customer_id
ORDER BY p.payment_date DESC, p.payment_id;

SELECT payment_status, COUNT(*) AS transaction_count,
       SUM(amount) AS total_amount
FROM Payment
GROUP BY payment_status;

SELECT payment_mode, COUNT(*) AS total_transactions,
       SUM(amount) AS total_amount
FROM Payment
GROUP BY payment_mode
ORDER BY total_transactions DESC;

SELECT payment_mode, COUNT(*) AS successful_transactions,
       SUM(amount) AS successful_amount
FROM Payment
WHERE payment_status = 'SUCCESS'
GROUP BY payment_mode
ORDER BY successful_transactions DESC;
