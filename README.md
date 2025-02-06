# Project Overview

   The primary business request for this data analyst project was to create an Executive Sales Report for Sales Managers.    This report was intended to provide actionable insights into internet sales performance through interactive dashboards    in Power BI.

# Business Objectives

   * The Sales Manager needed a dashboard that provides an overview of internet sales to monitor top-performing customers and products and track overall sales trends.
   * Sales Representatives required a detailed analysis of internet sales by customers to identify key buyers, improve engagement, and discover new sales opportunities.
   * A further request from Sales Representatives was for a detailed breakdown of internet sales by products to assess product performance and optimize sales strategies.
   * The Sales Manager also requested a sales performance dashboard to monitor sales over time and compare results against the budget.

# Data Summary
 
   * Data Source: SalesBudget.xlsx, Products.csv, InternetSales.csv, Customers.csv, Calendar.csv.
   
   * Data Description:
     *Key Variables:  Budget, Sales, Customer City, Product Category, Sales Amount etc.
     *Time Period: The period is from the year 2019 to 2021.

# Data Cleaning and Transformation
  
   *) Data Collection and Cleaning: The data was collected from the source containing multiple keys. There were multiple entries with null values which were removed. No duplicates were found during the analysis.  
   
   *)Data Exploration: The data was processed using SQL. For fulfilling the business needs the following tables were extracted using SQL.

      # Calender:
      -- Cleansed Date Table  
            SELECT  
                [DateKey],  
                [FullDateAlternateKey] AS Date,          -- Full date in a standard format  
                [EnglishDayNameOfWeek] AS Day,           -- Full name of the day (e.g., Monday)  
                [EnglishMonthName] AS Month,             -- Full name of the month (e.g., January)  
                LEFT([EnglishMonthName], 3) AS MonthShort,  -- Short form of the month (e.g., Jan)  
                [MonthNumberOfYear] AS MonthNo,          -- Month number (1 = January, 12 = December)  
                [CalendarQuarter] AS Quarter,            -- Calendar quarter (1 to 4)  
                [CalendarYear] AS Year                   -- Calendar year (e.g., 2019, 2020)  
                FROM  
                [AdventureWorksDW2019].[dbo].[DimDate]  
                 WHERE  
                [CalendarYear] >= 2019;                  -- Filter to include dates from 2019 onward

      # Customer
      -- Cleansed Customer Table  
            SELECT  
                c.CustomerKey AS CustomerKey,                 -- Unique identifier for each customer  
                c.FirstName AS [First Name],                  -- Customer's first name  
                c.LastName AS [Last Name],                    -- Customer's last name  
                c.FirstName + ' ' + c.LastName AS [Full Name], -- Combined full name (FirstName + LastName)  
                CASE c.Gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender, -- Gender mapping  
                c.DateFirstPurchase AS DateFirstPurchase,     -- Date of the customer's first purchase  
                g.City AS [Customer City]                     -- City information from the Geography table  
            FROM  
                [AdventureWorksDW2019].[dbo].[DimCustomer] AS c  
                LEFT JOIN [AdventureWorksDW2019].[dbo].[DimGeography] AS g  
                ON g.GeographyKey = c.GeographyKey            -- Join to get city details  
            ORDER BY  
                c.CustomerKey ASC;                            -- Order by CustomerKey for sorted output

      # Products
      -- Cleansed Product Table  
             SELECT  
                    p.ProductKey AS ProductKey,                      -- Unique identifier for each product  
                    p.ProductAlternateKey AS ProductItemCode,         -- Product item code (alternate key)  
                    p.EnglishProductName AS [Product Name],           -- Product name in English  
                    ps.EnglishProductSubcategoryName AS [Sub Category], -- Subcategory name from DimProductSubcategory  
                    pc.EnglishProductCategoryName AS [Product Category], -- Category name from DimProductCategory  
                    p.Color AS [Product Color],                       -- Product color  
                    p.Size AS [Product Size],                         -- Product size  
                    p.ProductLine AS [Product Line],                  -- Product line (e.g., R for Road, M for Mountain)  
                    p.ModelName AS [Product Model Name],              -- Product model name  
                    p.EnglishDescription AS [Product Description],    -- Product description in English  
                    ISNULL(p.Status, 'Outdated') AS [Product Status]  -- Product status with default value 'Outdated' if NULL  
                FROM  
                    [AdventureWorksDW2019].[dbo].[DimProduct] AS p  
                    LEFT JOIN [AdventureWorksDW2019].[dbo].[DimProductSubcategory] AS ps  
                        ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey  
                    LEFT JOIN [AdventureWorksDW2019].[dbo].[DimProductCategory] AS pc  
                        ON ps.ProductCategoryKey = pc.ProductCategoryKey  
                ORDER BY  
                    p.ProductKey ASC;                                 -- Ordered by ProductKey

                
      # InternetSales
      -- Cleansed FactInternetSales Table  
            SELECT  
                [ProductKey],                      -- Foreign key linking to the DimProduct table  
                [OrderDateKey],                    -- Order date key (links to DimDate)  
                [DueDateKey],                      -- Due date key (links to DimDate)  
                [ShipDateKey],                     -- Ship date key (links to DimDate)  
                [CustomerKey],                     -- Foreign key linking to the DimCustomer table  
                [SalesOrderNumber],                -- Unique sales order number  
                [SalesAmount]                      -- Total sales amount for the order  
            FROM  
                [AdventureWorksDW2019].[dbo].[FactInternetSales]  
            WHERE  
                LEFT(OrderDateKey, 4) >= YEAR(GETDATE()) - 2  -- Filters data for the last two years  
            ORDER BY  
                OrderDateKey ASC;                  -- Orders data by OrderDateKey for chronological output
            
                   
   *) Visualization and Analysis: Power BI and SQL.

# Data Model: 
Below is a screenshot of the data model after cleansed and prepared tables into Power BI. The model shows the relation between tables and other necessary insights.

![Model](https://github.com/user-attachments/assets/935d15c9-0f9d-4ecb-ba61-b390c277b6b9)


# Key Findings and Insights:

 * Dashboard Overview
     * Sales Insights Dashboard
       ![Sales Insights](https://github.com/user-attachments/assets/d3ee1624-893c-4e70-b461-54d4fc8c1f85)

       Sales by Product Category: Reveals which categories drive the most revenue.
       Top 10 Products: Shows the best-selling products to help optimize inventory and marketing strategies.
       Monthly Sales Performance: Tracks sales trends over time and compares them to the budgeted targets.
       Customer Distribution by City: Provides regional insights into customer concentration and sales distribution.

    * Customer Insights Dashboard
      ![Customer Insights](https://github.com/user-attachments/assets/b392445d-8d0a-4efe-9106-49e1635852b3)


      Top 10 Customers: Identifies our most valuable customers and their contribution to sales.
      Sales vs. Budget: Helps track how well sales performance aligns with monthly targets.
      Geographical Analysis: Understand regional sales patterns and target high-potential markets.
      Sales Trends: Visualizes customer purchasing behavior over time.
       
       

