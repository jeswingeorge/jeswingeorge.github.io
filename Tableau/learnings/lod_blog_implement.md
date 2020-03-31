I tried to implement the Tableau visualizations using LOD expressions made in the blog [](https://www.tableau.com/about/blog/LOD-expressions).

Important Tip: Measures are aggregated when added to the view. Adding a dimension will increase the granularity of the view.  


## 1. Customer order frequency

[Download dataset used here](https://www.dropbox.com/s/qmn7q50r6d4i4bc/superstore_sample.xlsx?dl=0)

Number of customers who ordered one order, two orders, three others and four others. To create a histogram of the number of customers who purchased 1,2,3,...N times.  

Steps:
- Create a calculated field __Number of distinct customers__: `COUNTD([Customer ID])` and drag it to Rows shelf and set Marks card to Bar type. 
- Create a calculated field __Number of orders per customer__: `{ FIXED [Customer ID]: COUNTD([Order ID])}` and convert this to dimension.
- Then drag the calculated field __Number of orders per customer__ to the Column shelf and also drag calculated field __Number of distinct customers__ to the color and Label Marks card. Then we get a histogram of the number of customers who purchased 1,2,3,...N times.
- Then do the necessary formatting, title and tooltip modififcation.

[Link to the created visualisation](https://public.tableau.com/views/HistogramofCustomerOrdersCount/Dashboard1?:display_count=y&publish=yes&:origin=viz_share_link)

![LOD1](../../images/tableau/learning/15_lods/lod1.PNG)


## 2. Cohort analysis

[Download dataset used here](https://www.dropbox.com/s/ypodk3kminqa7il/Global%20Superstore.xls?dl=0)

To compare a group (cohort) of customers based on their first year of order date and check their contribution to the overall Sales.

Important reference: [Tableau Order of expression evaluation](https://help.tableau.com/current/pro/desktop/en-us/order_of_operations.htm)

Steps:
- Create a calculated field __Customer Acquisition Date__ : `{ FIXED [Customer ID]: MIN([Order Date])}` and it will be a dimension.
- Drag __Sales__ to the Rows Shelf and set Marks card to Bar type and we get a bar graph with _SUM(Sales)_.
- Now to drag the calculated field __Customer Acquisition Date__ to color in the marks card and we get a stacked bar graph for each year in consideration showing the customer cohort year.
- Drag __Market__ to filters card to get the visualization for each market.
- Now duplicate the sheet  and to get percentage of sales for each year do a table calculation
![LOD1](../../images/tableau/learning/15_lods/lod2_table.PNG)
- Now do the formatting, labels and create dashboard.
- [Link to the created visualization](https://public.tableau.com/views/LOD-Revenuecontributedbyeachcustomercohortannuallybyeachcustomercohort/Dashboard1?:display_count=y&publish=yes&:origin=viz_share_link).
![LOD2](../../images/tableau/learning/15_lods/lod2.PNG)

## 3. Daily profit KPI

[Download dataset used here](https://www.dropbox.com/s/qmn7q50r6d4i4bc/superstore_sample.xlsx?dl=0)

To know the number of profitable days achieved each month or year, especially if we were curious about seasonal effects. The following view shows how LOD Expressions allow us to easily create bins on aggregated data such as profit per day, while the underlying data is recorded at a transactional level. 

Here we will set KPIS as : 
- __Profit per day__ > 2000 => Highly Profitable
- __Profit per day__ > 0 => Profitable
- __Profit per day__ < 0 => Unprofitable

Steps:
- Need to calculate profit each day using calculated field (and Fixed LOD) __Profit per day__: `{FIXED [Order Date] : SUM([Profit])}`. This helps us to easily bin the days in a secondary calculation.
- Create a calculated field __Daily Profit KPI__ : 
	```
	IF [Profit per day]>2000 THEN "Highly Profitable"
	ELSEIF [Profit per day]>0 THEN "Profitable"
	ELSE 'Unprofitable' END
	```
- Now drag __Order Date__ to columns shelf and keep __YEAR(Order Date)__ and __MONTH(Order Date)__.
- Drag calculated field __Daily Profit KPI__ to Rows shelf.
- Create a calculated field __Count of Distinct order dates__: `COUNTD([Order Date])` and drag it to Rows shelf.
- Select Mark type to Area.
- Drag calculated field __Daily Profit KPI__ to color Marks card.
- [Link to the created visualization](https://public.tableau.com/views/DailyProfitKPI_15854523679980/Dashboard1?:display_count=y&publish=yes&:origin=viz_share_link).
![LOD3](../../images/tableau/learning/15_lods/lod3_dashboard.png)

## 4. Percent of total















