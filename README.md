# Powerbi-sales-analytics
End-to-end sales analytics project using Power BI, DAX, Power Query, and SQL.

 
Beauty Business Sales Analytics
Мета проєкту
Мета — пройти повний цикл роботи аналітика з даними: від підготовки сирих даних до створення інтерактивного Power BI Dashboard та виконання SQL-запитів.
Інструменти:
Power BI Desktop, Power Query, DAX, SQL (SQLite).
 
Дані
• Основна таблиця Sales містить:
- “Sale_ID” — ID продажу
- “Date” — дата продажу
- “Product_ID” — ID продукту
- “Customer_ID” — ID клієнта
- “Quantity” — кількість
- “Price” — ціна
- “Status” — статус замовлення
- “City” — місто
• Products
- “Product_ID”
- “Product_Name”
- “Cost”
• Customers
- “Customer_ID”
- “Customer_Name”
 
Power Query — підготовка даних
Перевірено та підготовлено сирі дані, було виявлено різне написання однакових значень, різні формати запису дат у вихідних даних.
- “Kyiv” / “kyiv”
- “Completed” / “completed”
-2026-09-01.  01/09/2026.  2026/09/02.  03.09.2026

  <img width="1430" height="775" alt="74F45A80-1408-40FE-9F1C-2435776661E1" src="https://github.com/user-attachments/assets/4a5bc32d-e791-4af5-a528-f8b10fdf4317" />

Виконані операції:
- перевірено унікальність “Sale_ID”;
- стандартизовано назви міст;
- стандартизовано статуси замовлень;
- перевірено формат дат;
- перевірено числові поля;
- збережено статус “Cancelled” для подальшого аналізу.
- 
 <img width="2046" height="1520" alt="97BCE773-9C2A-4EF8-8F0E-551A1F0CEBF6" src="https://github.com/user-attachments/assets/fd6d8a50-7a7d-481d-a1af-679f07ad8ae6" />

У Power Query створено новий стовпець:
Revenue = Quantity × Price
 
 
Data Model
Після підготовки даних створено модель у Power BI.
Використано три таблиці:
Sales
Products
Customers
Таблиці пов’язані за відповідними ID:
- “Sales[Product_ID]” → “Products[Product_ID]”
- “Sales[Customer_ID]” → “Customers[Customer_ID]”
Це дозволило використовувати інформацію про продажі, продукти та клієнтів у спільному аналізі.
 
DAX
Створено основні DAX measures.
 
Загальна виручка
Total Revenue
Total Revenue = SUM(Sales[Revenue])
 
Кількість унікальних замовлень
Total Orders
Total Orders = DISTINCTCOUNT(Sales[Sale_ID])
 
Середня вартість замовлення
Average Order Value =
DIVIDE([Total Revenue], [Total Orders])
 
Прибуток з урахуванням собівартості продуктів
Profit =
SUMX(
   Sales,
   Sales[Quantity] *
   (Sales[Price] – RELATED(Products[Cost]))
)
 
Маржинальність
Profit Margin =
DIVIDE([Profit], [Total Revenue])
 
Виручка тільки за завершеними замовленнями
Completed Revenue =
CALCULATE(
   [Total Revenue],
   Sales[Status] = “Completed”
)
У проєкті використано такі DAX-функції:
SUM, DISTINCTCOUNT, DIVIDE, SUMX, RELATED, CALCULATE.
 
Dashboard
 
KPI
- Total Revenue
- Total Orders
- Average Order Value
- Profit
 
Revenue by City
Діаграма показує виручку за містами.
Це дозволяє порівнювати результати продажів у різних містах.
 
Revenue by Product
Графік показує виручку в розрізі продуктів.
Це дозволяє визначити продукти з найбільшою виручкою.

<img width="2046" height="1514" alt="D73BEB32-A875-4EF9-9C28-F95B53E452BF" src="https://github.com/user-attachments/assets/cad3fe86-6a5d-4537-8eac-ccf33a3c398c" />

Revenue Trend
Графік показує динаміку виручки за датами.
 
Profit by Product
Графік показує прибуток у розрізі продуктів.
Це дозволяє порівняти продукти не тільки за виручкою, а й за прибутковістю.
 
Interactive Filters
Додано фільтри:
- City
- Date
 
Можливо вибрати конкретне місто або часовий період та переглядати відповідні показники.

<img width="2047" height="1518" alt="0C1F226F-766D-4785-A581-E13BE951BF51" src="https://github.com/user-attachments/assets/f5a31cf8-af27-495f-943a-fcdd0a9862b3" />

 
 
SQL Analysis
Виконано запити з використанням:
- “SELECT”
- “WHERE”
- “GROUP BY”
- “JOIN”
- “SUM”
 
Аналіз виручки за містами
 
SELECT
   City,
   SUM(Price * Quantity) AS Total_Revenue
FROM Sales
WHERE Status = ‘Completed’
GROUP BY City;
Запит показує виручку за містами тільки для завершених продажів.
 
 
JOIN Sales + Products
SELECT
   s.Sale_ID,
   p.Product_Name,
   s.Quantity,
   s.Price,
   s.Status,
   s.City
FROM Sales s
JOIN Products  
ON s.Product_ID = p.Product_ID;

<img width="1430" height="811" alt="6E0C2EE2-7483-40F5-9B8A-2C565DF59EEE" src="https://github.com/user-attachments/assets/760cf884-cfc3-4204-baee-fd0a085a4bef" />


Продемонстровано об’єднання таблиць за ключем.
 
JOIN трьох таблиць
SELECT
   s.Sale_ID,
   c.Customer_Name,
   p.Product_Name,
   s.Quantity,
   s.Price,
   s.Quantity * s.Price AS Revenue
FROM Sales s
JOIN Customers c
   ON s.Customer_ID = c.Customer_ID
JOIN Products p
   ON s.Product_ID = p.Product_ID;
 
В одному запиті об’єднано:
Sales + Customers + Products.

<img width="1430" height="894" alt="597DA900-9054-

<img width="1430" height="810" alt="48E9501D-8473-45FB-8315-BBBE6FAD92AA" src="https://github.com/user-attachments/assets/f0c29fb8-f63a-49f5-ac88-9eb0129f1b37" />
4934-A765-C466D667C32A" src="https://github.com/user-attachments/assets/8417e3c9-252f-420f-824b-75918ee8e69d" />
