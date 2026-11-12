<img width="1533" height="813" alt="Screenshot 2026-09-23 140747" src="https://github.com/user-attachments/assets/62d811b1-d05c-4870-a5cf-e7ed5b9bc308" />
<img width="1532" height="816" alt="Screenshot 2026-09-30 113103" src="https://github.com/user-attachments/assets/60e389e2-f5ec-4886-a84a-4b8b78e27f61" />
<img width="1535" height="806" alt="Screenshot 2026-09-30 113110" src="https://github.com/user-attachments/assets/a52c83d9-2c6f-4136-8ded-c419963df13c" />
<img width="1527" height="813" alt="Screenshot 2026-09-30 113130" src="https://github.com/user-attachments/assets/87e2d558-4b97-4746-a3a8-e48b4b20c99b" />
<img width="1535" height="812" alt="Screenshot 2026-09-30 113139" src="https://github.com/user-attachments/assets/d5956408-0299-426f-b7a5-362fd80a7673" />
<img width="1535" height="817" alt="Screenshot 2026-09-30 113149" src="https://github.com/user-attachments/assets/300d6492-38ef-44d3-868e-5f94f002b477" />
<img width="1535" height="813" alt="Screenshot 2026-09-30 113201" src="https://github.com/user-attachments/assets/31fbacd1-3c1f-470a-a0d8-94fa56730c59" />
<img width="1535" height="816" alt="Screenshot 2026-09-30 113215" src="https://github.com/user-attachments/assets/5af2f064-38b7-4f87-bcac-62607697b7e6" />
<img width="1532" height="813" alt="Screenshot 2026-09-30 113231" src="https://github.com/user-attachments/assets/5b5d58ce-ce9d-4bb6-a2dd-e9ebc40da1e9" />
<img width="1533" height="813" alt="Screenshot 2026-09-30 113533" src="https://github.com/user-attachments/assets/72532c44-49d2-42aa-95ed-4a1d5a544e6f" />
<img width="1530" height="810" alt="Screenshot 2026-09-30 113547" src="https://github.com/user-attachments/assets/5b7fa4b2-91f4-49cb-9506-cc5a18b6c47c" />
<img width="1535" height="812" alt="Screenshot 2026-09-30 113601" src="https://github.com/user-attachments/assets/5573c777-0087-4883-bb86-7f5208edbb3a" />
<img width="1535" height="813" alt="Screenshot 2026-09-30 113616" src="https://github.com/user-attachments/assets/9dc79957-1947-4824-ac4f-b71d855b8781" />
<img width="1535" height="817" alt="Screenshot 2026-09-30 113628" src="https://github.com/user-attachments/assets/99921286-d9da-4bf5-b1b3-96d47c272c37" />
USE ecommerce_db;

CREATE TABLE Review (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    product_id INT NOT NULL,
    review_text VARCHAR(500) NOT NULL,
    review_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    FOREIGN KEY (product_id) REFERENCES Product(product_id)
);

CREATE TABLE Rating (
    rating_id INT PRIMARY KEY AUTO_INCREMENT,
    review_id INT NOT NULL UNIQUE,
    customer_id INT NOT NULL,
    product_id INT NOT NULL,
    rating_value INT NOT NULL,
    rating_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    FOREIGN KEY (review_id) REFERENCES Review(review_id),
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    FOREIGN KEY (product_id) REFERENCES Product(product_id),
    CHECK (rating_value BETWEEN 1 AND 5)
);

INSERT INTO Review (customer_id, product_id, review_text) VALUES
(1, 1, 'Good sound quality and comfortable to use.'),
(2, 1, 'The headphones are useful and have good battery life.'),
(3, 2, 'Smart watch has useful features and works well.'),
(1, 3, 'Comfortable cotton material and good quality.'),
(2, 4, 'Helpful book for learning Java programming.');

INSERT INTO Rating (review_id, customer_id, product_id, rating_value) VALUES
(1, 1, 1, 5),
(2, 2, 1, 4),
(3, 3, 2, 5),
(4, 1, 3, 4),
(5, 2, 4, 5);

SELECT
    r.review_id,
    c.customer_name,
    p.product_name,
    r.review_text,
    r.review_date,
    rt.rating_value
FROM Review r
JOIN Customer c ON r.customer_id = c.customer_id
JOIN Product p ON r.product_id = p.product_id
LEFT JOIN Rating rt ON r.review_id = rt.review_id
ORDER BY r.review_date DESC, r.review_id;

SELECT
    p.product_id,
    p.product_name,
    COUNT(rt.rating_id) AS total_ratings,
    ROUND(AVG(rt.rating_value), 2) AS average_rating
FROM Product p
LEFT JOIN Rating rt ON p.product_id = rt.product_id
GROUP BY p.product_id, p.product_name
ORDER BY average_rating DESC;

SELECT
    p.product_id,
    p.product_name,
    ROUND(AVG(rt.rating_value), 2) AS average_rating,
    COUNT(rt.rating_id) AS total_ratings
FROM Product p
JOIN Rating rt ON p.product_id = rt.product_id
GROUP BY p.product_id, p.product_name
HAVING AVG(rt.rating_value) >= 4
ORDER BY average_rating DESC;

