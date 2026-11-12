<img width="1530" height="812" alt="image" src="https://github.com/user-attachments/assets/37925297-89b9-4d76-a8e2-ed4442305c0c" />![Uploading Screenshot 2026-11-11 202020.png…]()
<img width="1535" height="812" alt="Screenshot 2026-11-11 202444" src="https://github.com/user-attachments/assets/d0f48597-17a5-4b6a-bf93-89c8c678f856" />
<img width="1535" height="815" alt="Screenshot 2026-11-11 202451" src="https://github.com/user-attachments/assets/b4be7877-ae09-4c53-b9b7-75c627c2d2b5" />
<img width="1535" height="812" alt="Screenshot 2026-11-11 202459" src="https://github.com/user-attachments/assets/7d280843-4151-4291-9178-a880be88c8fe" />
<img width="1532" height="817" alt="Screenshot 2026-11-11 202508" src="https://github.com/user-attachments/assets/1f57a3dc-766f-4a6e-b0fb-bfc360265883" />
<img width="1535" height="817" alt="Screenshot 2026-11-11 202514" src="https://github.com/user-attachments/assets/249a90c4-2b8d-4dc4-b044-5d7b2661d53f" />
<img width="1486" height="761" alt="Screenshot 2026-11-11 202522" src="https://github.com/user-attachments/assets/a2105fd3-693c-47d1-9c6f-78d7473ecc03" />

USE ecommerce_db;

SELECT COUNT(*) AS Total_Sales
FROM Sales;

SELECT SUM(Quantity * Price) AS Total_Sales_Amount
FROM Sales;

SELECT AVG(Quantity * Price) AS Average_Sales
FROM Sales;

SELECT MIN(Quantity * Price) AS Minimum_Sale
FROM Sales;

SELECT MAX(Quantity * Price) AS Maximum_Sale
FROM Sales;

SELECT 
    COUNT(*) AS Total_Orders,
    SUM(Quantity * Price) AS Total_Sales,
    AVG(Quantity * Price) AS Average_Sale,
    MIN(Quantity * Price) AS Minimum_Sale,
    MAX(Quantity * Price) AS Maximum_Sale
FROM Sales;

SELECT 
    Customer_Name,
    SUM(Quantity * Price) AS Total_Purchase
FROM Sales
GROUP BY Customer_Name
ORDER BY Total_Purchase DESC
LIMIT 1;

SELECT 
    Product_Name,
    SUM(Quantity * Price) AS Total_Revenue
FROM Sales
GROUP BY Product_Name
ORDER BY Total_Revenue DESC;

SELECT 
    Category,
    COUNT(*) AS Number_of_Sales,
    SUM(Quantity) AS Total_Quantity,
    SUM(Quantity * Price) AS Total_Sales,
    AVG(Quantity * Price) AS Average_Sale
FROM Sales
GROUP BY Category
ORDER BY Total_Sales DESC;

