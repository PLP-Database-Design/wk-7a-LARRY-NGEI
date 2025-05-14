# 📝 Assignment: Database Design and Normalization


--- 

## 📚 Assignment Questions

---

### Question 1 Achieving 1NF (First Normal Form) 🛠️
-- Create a new 1NF-compliant table
CREATE TABLE OrderProducts_1NF AS
SELECT 
    OrderID,
    CustomerName,
    TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(Products, ',', n.digit+1), ',', -1)) AS Product
FROM 
    ProductDetail
JOIN 
    (SELECT 0 AS digit UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3) n
    ON LENGTH(REPLACE(Products, ',', '')) <= LENGTH(Products) - n.digit
WHERE
    TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(Products, ',', n.digit+1), ',', -1)) != ''
ORDER BY
    OrderID, Product;

--- 

### Question 2 Achieving 2NF (Second Normal Form) 🧩
-- Step 1: Create Orders table (removes partial dependency)
CREATE TABLE Orders AS
SELECT DISTINCT
    OrderID,
    CustomerName
FROM 
    OrderDetails;

-- Step 2: Create OrderItems table with full dependency on composite key
CREATE TABLE OrderItems AS
SELECT 
    OrderID,
    Product,
    Quantity
FROM 
    OrderDetails;

-- Step 3: Add primary keys
ALTER TABLE Orders ADD PRIMARY KEY (OrderID);
ALTER TABLE OrderItems ADD PRIMARY KEY (OrderID, Product);

-- Step 4: Add foreign key relationship
ALTER TABLE OrderItems 
ADD CONSTRAINT fk_order
FOREIGN KEY (OrderID) REFERENCES Orders(OrderID);


---
