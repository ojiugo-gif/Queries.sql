# Queries.sql

-- SHOW DATABASES

-- use mintclassics;
-- SHOW TABLES;
-- DESCRIBE warehouses;
-- DESCRIBE customers;
-- DESCRIBE employees;
-- DESCRIBE offices;
-- DESCRIBE orderdetails;
-- DESCRIBE orders;
-- DESCRIBE payments;
-- DESCRIBE productlines;
-- DESCRIBE products;
-- DESCRIBE warehouses;

# 1. Inventory Distribution
 -- SELECT 
--      w.warehouseCode,
--     w.warehouseName,
--     SUM(p.quantityInStock) AS total_stock
-- FROM warehouses w
-- JOIN products p ON w.warehouseCode = p.warehouseCode
-- GROUP BY w.warehouseCode, w.warehouseName
--  ORDER BY total_stock DESC;
 # Finds which warehouse holds the largest and smallest inventory.

# 2. Revenue Contribution
--  SELECT 
--      w.warehouseCode,
--      w.warehouseName,
-- -     ROUND(SUM(od.quantityOrdered * od.priceEach), 2) AS total_revenue
--  FROM warehouses w
--  JOIN products p ON w.warehouseCode = p.warehouseCode
-- JOIN orderdetails od ON p.productCode = od.productCode
-- JOIN orders o ON od.orderNumber = o.orderNumber
--  GROUP BY w.warehouseCode, w.warehouseName
--  ORDER BY total_revenue ASC;
# Shows which warehouse brings in the most and least revenue.

# 3. Shipping Performance
-- SELECT 
--     w.warehouseCode,
--     round(AVG(DATEDIFF(o.shippedDate, o.orderDate)), 1)AS avg_shipping_days
-- FROM warehouses w
-- JOIN products p ON w.warehouseCode = p.warehouseCode
-- JOIN orderdetails od ON p.productCode = od.productCode
-- JOIN orders o ON od.orderNumber = o.orderNumber
-- WHERE o.shippedDate IS NOT NULL
-- GROUP BY w.warehouseCode
-- ORDER BY avg_shipping_days;
# Finds out how fast each warehouse ships orders.

# 4. Dead Stock
--  SELECT 
--      p.productCode,
--      p.productName,
--     p.quantityInStock,
--     COALESCE(SUM(od.quantityOrdered), 0) AS total_ordered
--  FROM products p
--  LEFT JOIN orderdetails od ON p.productCode = od.productCode
-- GROUP BY p.productCode, p.productName, p.quantityInStock
-- HAVING total_ordered = 0
--     OR total_ordered < (0.1 * p.quantityInStock)   -- tweak % as needed
--  ORDER BY total_ordered ASC;
# Identifies products with zero or very low sales (candidates for phase-out).

# 5. Efficiency Observation
-- SELECT 
--     w.warehouseCode,
--     w.warehouseName,
--     SUM(p.quantityInStock) AS total_stock,
--     ROUND(SUM(od.quantityOrdered * od.priceEach), 2) AS total_revenue
-- FROM warehouses w
-- JOIN products p ON w.warehouseCode = p.warehouseCode
-- JOIN orderdetails od ON p.productCode = od.productCode
-- JOIN orders o ON od.orderNumber = o.orderNumber
# Helps identify warehouses with large inventory but low sales (inefficiency).
-- GROUP BY w.warehouseCode, w.warehouseName
-- ORDER BY total_revenue ASC, total_stock DESC;
