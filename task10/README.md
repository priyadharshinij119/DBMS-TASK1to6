<img width="1535" height="812" alt="Screenshot 2026-11-11 195346" src="https://github.com/user-attachments/assets/878ee695-cd02-4c1f-a660-fca0c5a31cfe" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 222404" src="https://github.com/user-attachments/assets/47dd6347-f21d-4aa7-b75e-0dec89c6c6be" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 222413" src="https://github.com/user-attachments/assets/f0a31531-cb98-4ad9-9546-f6a809ce8d07" />
<img width="1535" height="816" alt="Screenshot 2026-11-11 222422" src="https://github.com/user-attachments/assets/c10e2a0b-70be-4604-b48e-eeb189e32d36" />
<img width="1535" height="817" alt="Screenshot 2026-11-11 222429" src="https://github.com/user-attachments/assets/47e1b1eb-12ac-4402-88a1-d3cfb94355b6" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 222440" src="https://github.com/user-attachments/assets/76912312-a655-47a5-ab8a-eda7e6d82e1c" />
<img width="1535" height="817" alt="Screenshot 2026-11-11 222449" src="https://github.com/user-attachments/assets/34a57b54-f580-493b-a8c3-0e6890e1dcfc" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 222459" src="https://github.com/user-attachments/assets/7057c1b0-5725-46f0-81a4-e3908acb4ecf" />
USE ecommerce_db;

SELECT Product_Name, Price
FROM Sales
WHERE Price > (
    SELECT AVG(Price)
    FROM Sales
);

SELECT Product_Name, Price
FROM Sales
WHERE Price = (
    SELECT MAX(Price)
    FROM Sales
);

SELECT Customer_Name, Total_Purchase
FROM (
    SELECT Customer_Name,
           SUM(Quantity * Price) AS Total_Purchase
    FROM Sales
    GROUP BY Customer_Name
) AS Customer_Sales
WHERE Total_Purchase > 50000;

SELECT Customer_Name,
       SUM(Quantity * Price) AS Total_Purchase
FROM Sales
GROUP BY Customer_Name
ORDER BY Total_Purchase DESC
LIMIT 1;

SELECT Customer_Name,
       SUM(Quantity * Price) AS Total_Purchase
FROM Sales
GROUP BY Customer_Name
HAVING SUM(Quantity * Price) > 50000;

SELECT Product_Name,
       SUM(Quantity) AS Total_Quantity_Sold
FROM Sales
GROUP BY Product_Name
ORDER BY Total_Quantity_Sold DESC
LIMIT 1;


SELECT Category,
       SUM(Quantity * Price) AS Total_Sales
FROM Sales
GROUP BY Category
ORDER BY Total_Sales DESC
LIMIT 1;

SELECT DISTINCT Customer_Name
FROM Sales
WHERE Product_Name = 'Laptop';

SELECT
    Customer_Name,
    COUNT(*) AS Total_Orders,
    SUM(Quantity) AS Total_Quantity,
    SUM(Quantity * Price) AS Total_Purchase,
    AVG(Quantity * Price) AS Average_Purchase,
    MIN(Quantity * Price) AS Minimum_Purchase,
    MAX(Quantity * Price) AS Maximum_Purchase
FROM Sales
GROUP BY Customer_Name
ORDER BY Total_Purchase DESC;

