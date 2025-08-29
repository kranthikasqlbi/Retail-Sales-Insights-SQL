# Retail-Sales-Insights-SQL
SQL Portfolio Project | End-to-End Car Sales Data Analytics (ETL, Trend Analysis, Dealer Insights)

# Retail Store Background
A retail store records its daily sales, including order details, customer information, product categories, order times, and order status. By analyzing this data, the business can understand customer behavior, identify top-selling products, manage inventory better, and track overall performance. The insights help in improving day-to-day operations, offering a better shopping experience to customers, and ultimately increasing profitability

## Note: 
This project is designed as a case study. I have also worked on similar real-time projects for clients in the car sales and retail domains, where I applied the same SQL and data analysis concepts to generate actionable business insights.

# Business Motive
Since the business is already profitable, our focus should be on increasing profits with minimal additional costs or expenses. To achieve this, I have identified 15 key business questions and designed SQL queries that will provide actionable insights. These queries will help in optimizing product strategy, customer engagement, delivery efficiency, and overall operational costs, ensuring sustained growth and higher profitability.


Recommended Business Questions & SQL Queries

Which products sell the most?
→ Identify top-selling products to focus on demand.

What do customers prefer the most?
→ Analyze purchase frequency across customer segments.

Which items bring in the highest profit margins?
→ Track profitability per product.

Where are things going wrong in delivery or operations?
→ Highlight delayed or failed deliveries.

At what time of day/week/month are sales highest or lowest?
→ Discover sales seasonality for promotions.

Which locations/stores are performing better or worse?
→ Compare store/location sales performance.

Which customers are most loyal and bring repeat business?
→ Segment loyal vs. one-time customers.

How are discounts and offers affecting overall sales and profitability?
→ Measure if discounts increase sales or cut margins.

Which products are frequently bought together (cross-sell opportunities)?
→ Find product bundling opportunities.

Which products have high returns or cancellations (quality/operational issue)?
→ Detect problem items causing losses.

Which suppliers/products cause frequent stock-outs?
→ Analyze supply-side issues.

What percentage of sales comes from new vs. repeat customers?
→ Track customer acquisition vs. retention.

How much is being spent on delivery/logistics vs. revenue earned?
→ Optimize logistics costs.

Which customers are most profitable vs. least profitable?
→ Focus marketing on high-value customers.

How can overall expenses be reduced without affecting sales?
→ Identify areas where cost-cutting won’t hurt revenue.


# Solution

### 1. Top-selling products

SQL Idea:

SELECT TOP 10 ProductName, SUM(Quantity) AS TotalSold
FROM Sales
GROUP BY ProductName
ORDER BY TotalSold DESC;

### Business Problem: Store doesn’t know which products sell the most.
### Impact: Helps in stock management & prioritizing best-sellers → more sales with less effort.


###  2. Least-selling products
SELECT TOP 10 ProductName, SUM(Quantity) AS TotalSold
FROM Sales
GROUP BY ProductName
ORDER BY TotalSold ASC;

### Problem: Capital blocked in slow-moving items.
### Impact: Reduce inventory cost by focusing only on products that actually sell.


### 3. Most profitable products
SELECT TOP 10 ProductName, SUM((Price - Cost) * Quantity) AS Profit
FROM Sales
GROUP BY ProductName
ORDER BY Profit DESC;

###  Problem: Store doesn’t know which items give higher margins.
###  Impact: Push high-margin products → profits grow without increasing costs.


### 4. Profit leakage products
SELECT ProductName, SUM((Price - Cost) * Quantity) AS Profit
FROM Sales
GROUP BY ProductName
HAVING SUM((Price - Cost) * Quantity) < 0;

### Problem: Some products might be sold at loss.
### Impact: Identifying them saves business from hidden losses.


### 5. Peak sales times
SELECT DATENAME(HOUR, OrderTime) AS HourOfDay, SUM(Quantity) AS TotalSold
FROM Sales
GROUP BY DATENAME(HOUR, OrderTime)
ORDER BY TotalSold DESC;

### Problem: Store doesn’t know when sales peak.
### Impact: Allocate staff/resources at peak hours → less cost, better service.


### 6. Sales by day/week/month
SELECT DATENAME(WEEKDAY, OrderDate) AS Day, SUM(SalesAmount) AS TotalSales
FROM Sales
GROUP BY DATENAME(WEEKDAY, OrderDate)
ORDER BY TotalSales DESC;

### Problem: Wrong promotions at wrong times.
### Impact: Run discounts only when sales are low → higher profit with less expense.


### 7. Best & worst performing stores
SELECT StoreID, SUM(SalesAmount) AS TotalSales, SUM((Price - Cost) * Quantity) AS Profit
FROM Sales
GROUP BY StoreID
ORDER BY Profit DESC;

### Problem: Store doesn’t know which branches are dragging performance.
### Impact: Focus on weaker stores, replicate success of top stores.


### 8. Customer loyalty (repeat buyers)
SELECT CustomerID, COUNT(DISTINCT OrderID) AS OrdersCount
FROM Sales
GROUP BY CustomerID
HAVING COUNT(DISTINCT OrderID) > 5;

### Problem: Store doesn’t know loyal customers.
### Impact: Reward loyal customers → low cost, high repeat revenue.


### 9. High-value customers (VIPs)
SELECT CustomerID, SUM(SalesAmount) AS TotalSpent
FROM Sales
GROUP BY CustomerID
ORDER BY TotalSpent DESC;

### Problem: VIP customers not identified.
### Impact: Special offers to VIPs → keep them loyal, ensure continuous revenue.


### 10. Customer churn (inactive customers)
SELECT CustomerID
FROM Sales
WHERE OrderDate < DATEADD(MONTH, -3, GETDATE());

### Problem: Customers who stopped buying.
### Impact: Targeted re-engagement campaigns → more sales without acquiring new customers.


### 11. Effect of discounts on sales
SELECT Discount, SUM(SalesAmount) AS Revenue, SUM((Price - Cost) * Quantity) AS Profit
FROM Sales
GROUP BY Discount;

### Problem: Discounts might reduce profit instead of increasing sales.
### Impact: Run only profitable discounts → save costs.


### 12. Delivery delays
SELECT OrderID, DATEDIFF(DAY, OrderDate, DeliveryDate) AS DeliveryDays
FROM Orders
WHERE DATEDIFF(DAY, OrderDate, DeliveryDate) > 5;

### Problem: Late deliveries reduce customer trust.
### Impact: Improve logistics → better customer experience, no extra cost.


### 13. Return rate per product
SELECT ProductName, COUNT(ReturnID) * 100.0 / COUNT(OrderID) AS ReturnRate
FROM Orders
LEFT JOIN Returns ON Orders.OrderID = Returns.OrderID
GROUP BY ProductName;

### Problem: High returns waste time & money.
### Impact: Fix quality issues → less cost, more profit.


### 14. Profit per category
SELECT Category, SUM((Price - Cost) * Quantity) AS Profit
FROM Sales
GROUP BY Category
ORDER BY Profit DESC;

### Problem: Store doesn’t know which categories are profitable.
### Impact: Invest more in profitable categories, cut low-performing ones.


### 15. Inventory optimization
SELECT ProductName, SUM(Quantity) AS TotalSold
FROM Sales
GROUP BY ProductName
HAVING SUM(Quantity) < 50;

### Problem: Overstock of items with low demand.
### Impact: Reduce storage costs, invest only in fast-moving stock.


# Summary:
All these SQL queries give actionable insights →
Cut unnecessary costs (inventory, discounts, delivery delays)
Focus on high profit areas (top products, VIP customers, profitable stores)
Boost revenue without heavy spending.
