<img width="1535" height="802" alt="Screenshot 2026-11-11 230451" src="https://github.com/user-attachments/assets/7534f66d-3206-4e13-9c90-52c38c9e19b1" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 230458" src="https://github.com/user-attachments/assets/524837d2-8194-4e9b-b75d-e3a5c74e3fb9" />
<img width="1526" height="813" alt="Screenshot 2026-11-11 230509" src="https://github.com/user-attachments/assets/7fe5351b-f11c-4834-a0c4-885fcee3cd92" />
<img width="1533" height="816" alt="Screenshot 2026-11-11 230519" src="https://github.com/user-attachments/assets/641eb5a6-816e-42c0-bff9-609dc7fbb0fa" />
<img width="1535" height="817" alt="Screenshot 2026-11-11 230527" src="https://github.com/user-attachments/assets/1b3d623f-0d9d-4ca2-b4b8-408f658d3399" />
<img width="1535" height="810" alt="Screenshot 2026-11-11 230535" src="https://github.com/user-attachments/assets/fdeacf1e-fbf6-4870-99ce-0c270ff22460" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 230542" src="https://github.com/user-attachments/assets/9da25831-6426-47bb-a2f2-678810033f15" />
<img width="1535" height="813" alt="Screenshot 2026-11-11 230550" src="https://github.com/user-attachments/assets/33c2c1ef-ef20-4a65-b2fe-9dee8d4976fa" />
CREATE DATABASE IF NOT EXISTS ecommerce_db;
USE ecommerce_db;

CREATE OR REPLACE VIEW Order_Summary AS
SELECT
    o.order_id,
    o.customer_id,
    c.customer_name,
    o.order_date,
    o.order_status,
    o.total_amount
FROM Orders o
JOIN Customer c ON o.customer_id = c.customer_id;

CREATE OR REPLACE VIEW Product_Sales_Report AS
SELECT
    p.product_id,
    p.product_name,
    COALESCE(SUM(od.quantity), 0) AS total_quantity_sold,
    COALESCE(SUM(od.total_price), 0.00) AS total_sales
FROM Product p
LEFT JOIN Order_Details od ON p.product_id = od.product_id
GROUP BY p.product_id, p.product_name;

CREATE OR REPLACE VIEW Payment_Report AS
SELECT
    py.payment_id,
    py.order_id,
    c.customer_name,
    py.payment_mode,
    py.payment_date,
    py.amount,
    py.payment_status
FROM Payment py
JOIN Orders o ON py.order_id = o.order_id
JOIN Customer c ON o.customer_id = c.customer_id;

DROP FUNCTION IF EXISTS Calculate_Order_Sales;
DROP PROCEDURE IF EXISTS Process_Order;

DELIMITER $$

CREATE FUNCTION Calculate_Order_Sales(p_order_id INT)
RETURNS DECIMAL(10,2)
READS SQL DATA
BEGIN
    DECLARE v_sales DECIMAL(10,2);

    SELECT COALESCE(SUM(total_price), 0.00)
    INTO v_sales
    FROM Order_Details
    WHERE order_id = p_order_id;

    RETURN v_sales;
END$$

CREATE PROCEDURE Process_Order(
    IN p_customer_id INT,
    IN p_product_id INT,
    IN p_quantity INT
)
BEGIN
    DECLARE v_price DECIMAL(10,2);
    DECLARE v_stock INT;
    DECLARE v_order_id INT;
    DECLARE v_total DECIMAL(10,2);

    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        RESIGNAL;
    END;

    IF p_quantity <= 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Quantity must be greater than zero';
    END IF;

    START TRANSACTION;

    IF NOT EXISTS (
        SELECT 1
        FROM Customer
        WHERE customer_id = p_customer_id
    ) THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Customer does not exist';
    END IF;

    SELECT price, stock
    INTO v_price, v_stock
    FROM Product
    WHERE product_id = p_product_id
    FOR UPDATE;

    IF v_stock IS NULL THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Product does not exist';
    END IF;

    IF v_stock < p_quantity THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Insufficient product stock';
    END IF;

    SET v_total = v_price * p_quantity;

    INSERT INTO Orders (customer_id, total_amount, order_status)
    VALUES (p_customer_id, v_total, 'PLACED');

    SET v_order_id = LAST_INSERT_ID();

    INSERT INTO Order_Details (
        order_id, product_id, quantity, unit_price, total_price
    )
    VALUES (
        v_order_id, p_product_id, p_quantity, v_price, v_total
    );

    UPDATE Product
    SET stock = stock - p_quantity
    WHERE product_id = p_product_id;

    COMMIT;
END$$

DELIMITER ;

SELECT * FROM Order_Summary ORDER BY order_id;
SELECT * FROM Product_Sales_Report ORDER BY total_sales DESC;
SELECT * FROM Payment_Report ORDER BY payment_date DESC;
SELECT Calculate_Order_Sales(1) AS calculated_order_sales;
