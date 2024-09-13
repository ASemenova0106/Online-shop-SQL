# Примеры запросов на языке SQL, которые были использованы в проекте "Dashboard для отдела продаж онлайн-магазина"

**Общая выручка магазина по всем странам:**

SELECT
    Country,
    SUM(TotalPrice) AS sum_price
FROM (
    SELECT
    InvoiceNo,
    Country,
    SUM(Quantity * UnitPrice) as TotalPrice
    FROM default.retail
    WHERE Quantity >0
    GROUP BY InvoiceNo, Country)
GROUP BY Country
ORDER BY sum_price DESC

**Общая выручка магазина по странам без учета Великобритании:**

SELECT
    Country,
    SUM(TotalPrice) AS sum_price
FROM (
    SELECT
    InvoiceNo,
    Country,
    SUM(Quantity * UnitPrice) as TotalPrice
    FROM default.retail
    WHERE Quantity >0 AND Country != 'United Kingdom'
    GROUP BY InvoiceNo, Country)
GROUP BY Country
ORDER BY sum_price DESC

**Число уникальных клиентов в Великобритании по месяцам:**

SELECT
    Country,
    toStartOfMonth(InvoiceDate) as month,
    count(distinct CustomerID) AS MAU
FROM default.retail
WHERE Country = 'United Kingdom'
GROUP BY
    Country, month

**Средний чек заказов по странам:**

SELECT
    Country,
    AVG(TotalPrice) AS avg_price
FROM (
    SELECT
    InvoiceNo,
    Country,
    SUM(Quantity * UnitPrice) as TotalPrice
    FROM default.retail
    WHERE Quantity >0
    GROUP BY InvoiceNo, Country)
GROUP BY Country

**ТОП-20 самых продаваемых товаров в магазине:**

SELECT StockCode, Description, SUM(ABS(Quantity)) as sum_quantity
FROM default.retail
GROUP BY StockCode, Description
ORDER BY sum_quantity DESC
LIMIT 20
