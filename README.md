# Sales_KPI_Project_PB

Sales KPI Power BI Project — 27 Business KPIs with DAX

A Power BI project built to practice designing and calculating business KPIs from scratch using DAX. The core deliverable is a bank of 27 KPIs covering sales, profitability, orders, fulfillment, discounts, and time intelligence, backed by a supporting practice dashboard.

Show Image
Show Image
Show Image

📌 Project Overview
	
Project type	KPI design practice (27 KPIs) + a practice analytical dashboard
Tool	Power BI Desktop
Data source	Ecommerce_Sales_Data.xlsx — sheet Sales_Data
Rows / Columns	2,500 orders × 31 columns
Report pages	All KPI's, Page 3 (cover), DAShBOARD
File	KPI_s_Project.pbix

The primary focus of this project is the 27-KPI catalog — each KPI is a standalone DAX measure. The DAShBOARD page is a simple, normal chart-based dashboard built on the side for extra Power BI practice.

🗂️ Data Model

Fact table: Sales_Data (2,500 rows, 31 columns), loaded from the Excel workbook via Power Query:

Order ID, Order Date, Customer ID, Customer Name, Customer Age, City, State, Zone,
Category, Product, SKU, Quantity, Unit Price, Gross Sales, Coupon Code, Discount Amount,
GST Rate %, Net Sales, GST Amount, Payment Mode, Order Status, Sales Channel,
Sales Employee, Delivery Partner, Delivery Days, Customer Rating, COGS, Profit,
Profit Margin %, Shiping Charges, Invoice Amount

Supporting tables:

Date — a calendar/date table used for all time-intelligence measures (MTD, QTD, YTD, YoY, running totals)
DateTableTemplate_* / LocalDateTable_* — Power BI's auto date/time tables

Measure table: All Measure — a dedicated table holding all 28 DAX measures (27 of them surfaced as KPI cards; one is an internal helper used inside another KPI).

📊 The 27 KPIs (with DAX)

All formulas below are copied exactly as authored in the .pbix file's data model.

1. Sales & Profitability
KPI	DAX
Total Sales	SUM(Sales_Data[Net Sales])
Gross Sales	SUM(Sales_Data[Gross Sales])
Total Profit	SUM(Sales_Data[Profit])
Profit Margin%	DIVIDE([Total Profit],[Total Sales])
Total COGS	SUM(Sales_Data[COGS])
2. Orders & Customers
KPI	DAX
Total Orders	DISTINCTCOUNT(Sales_Data[Order ID])
Total Customers	DISTINCTCOUNT(Sales_Data[Customer ID])
Average order value	DIVIDE([Total Sales],[Total Orders])
3. Discounts, GST & Shipping
KPI	DAX
Total Discount	SUM(Sales_Data[Discount Amount])
Discount%	DIVIDE([Total Discount],[Gross Sales])
Total GST	SUM(Sales_Data[GST Amount])
Total Shippimg	SUM(Sales_Data[Shiping Charges])
4. Order Fulfillment
KPI	DAX
Deliverd Orders	CALCULATE( [Total Orders],Sales_Data[Order Status]= "Delivered")
Returnd Orders	CALCULATE( [Total Orders],Sales_Data[Order Status]= "Returned")
Cancelled Orders	CALCULATE( [Total Orders],Sales_Data[Order Status]= "Cancelled")
Delivery Success Rate%	DIVIDE([Deliverd Orders],[Total Orders])
Return Rate%	DIVIDE([Returnd Orders],[Total Orders])
Cancellation Rate %	DIVIDE([Cancelled Orders],[Total Orders])
5. Time Intelligence
KPI	DAX
Sales MTD	TOTALMTD([Total Sales],'Date'[Date])
Sales QTD	TOTALQTD([Total Sales],'Date'[Date])
Sales YTD	TOTALYTD([Total Sales],'Date'[Date])
Profit YTD	TOTALYTD([Total Profit],'Date'[Date])
6. Year-over-Year Growth
KPI	DAX
Previous year Sales	CALCULATE([Total Sales],DATEADD('Date'[Date],-1,YEAR))
Previous Year Profit	CALCULATE([Total Profit],SAMEPERIODLASTYEAR('Date'[Date]))
YoY Sales Growth%	DIVIDE([Total Sales]-[previois year sales],[previois year sales])
YoY Profit Growth%	DIVIDE([Total Profit]-[Previous Year Profit],[Previous Year Profit])
7. Running Total
KPI	DAX
Running Total Sales	CALCULATE([Total Sales],FILTER(ALLSELECTED('Date'[Date]),'Date'[Date]<=MAX('Date'[Date])))

ℹ️ Helper measure: previois year sales = CALCULATE([Total Sales],SAMEPERIODLASTYEAR('Date'[Date])) — this is an internal support measure (not shown as its own card) used inside YoY Sales Growth%. It brings the total measure count in the model to 28, with 27 exposed as KPI cards.

🖥️ Report Pages
Page 1 — All KPI's

A KPI-card grid showing 25 of the 27 KPIs at a glance (Sales, Profit, Orders, Customers, Discounts, GST, Shipping, Delivery/Return/Cancellation rates, MTD/QTD/YTD, and Previous Year Sales).

Show Image

Page 2 — Page 3 (Cover)

A project cover/title page — "KPI's Project — Total 27 KPI's" — plus the remaining 2 KPI cards (YoY Profit Growth% and Running Total Sales).

Show Image

Page 3 — DAShBOARD (practice dashboard)

A supporting, normal analytical dashboard built for extra Power BI practice — not the main deliverable. It includes:

Total Customers by months Name — column chart
Total Sales by months Name — line/area chart
Gross Sales by Quarter — pie chart
Total Sales and Profit Margin% by year — donut chart (2024 vs 2025)
Average order value by Order Status — bar chart
Total Orders by Querter — bar chart
YoY Sales Growth% by Querter — bar chart
Slicers: Category, Order Status, Sales Channel, and an Order Date range slider

Show Image

🛠️ Tools Used
Power BI Desktop — data modeling, report building
Power Query — imported and typed the Sales_Data sheet from the Excel source
DAX — all 28 measures (27 KPIs) built from scratch
