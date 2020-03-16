# Working with sets in Tableau

__Sets__ in Tableau are used to create subsets of data based on certain conditions defined by the user. There are two types of sets: __dynamic set__ and __fixed set__. In Tableau, sets can only be created based on dimension fields.

Here the following topics will be covered:
1. Creating a dynamic set
2. Creating a fixed set
3. Create a combined set
4. Use sets in the visualization
5. Using set actions to make sets more dynamic and interactive

__(Global superstore Data used for this blog can be downloaded from here)[https://1drv.ms/x/s!Al_62RL1SdJmiUZ17zQ5m6hokJLu?e=WZciJa]__  

## Creating a dynamic set

The members of a dynamics set changes when there is a change in the underlying data. Dynamic sets can only be based on a single dimension.

Lets try to answer the question - __Show the top 10 Products which has the maximum sales.__

1. In the Data pane, under Dimensions, right-click the field __Product Name__ and select __Create > Set__.
2. In the Create Set dialog box, configure your set. There are 3 tabs to aid in configuring the set:
	- __General__ Used to select one or more values that will be considered when computing the set. Usually the __Use all__/__Custom value list__ option is used to consider all members of the dimensions and later the condition/top tab is used for selecting the filtered or required member.
	![general](../../images/tableau/working_with_sets/1_general.PNG)

	- __Condition__ Use the Condition tab to define rules that determine what members to include in the set. Set conditions work the same as filter conditions.      
	Example: Condition to select only those products which has profit more than $4000. (Wont be using this tab's conditon in the main question defined above)
	![condition](../../images/tableau/working_with_sets/2_condition.PNG)

	Now following with the question, set condition to None.
	![condition](../../images/tableau/working_with_sets/3a_condition.PNG)

	- __Top__ Use the top tab to define the limits on what members to include in the set.  
	![top](../../images/tableau/working_with_sets/3_top.PNG)

3. When finished click __OK__. The new set is added to the bottom of the Data pane, under the Sets section.
![dynamic_set](../../images/tableau/working_with_sets/4_set.PNG)

4. Now lets observe the effect of sets. Drop the __Product Name__ dimension to the rows shelf. And drag measure __Sales__ to the Text in marks card and we get total sales for each Product. Then drag the set __Top 10 Products with max sales__ to the filter and we get the top 10 Products which has the maximum sales.
![observe](../../images/tableau/working_with_sets/5_observe.PNG)

5. Now select only the __Country__ - _India_ using filter and observe the change in the Products along with the their sales value. We get the top 10 Products sold in India.
 ![India](../../images/tableau/working_with_sets/6_india.PNG)


## Creating a fixed set









### Reference:
1. [Tableau help: Create Sets](https://help.tableau.com/current/pro/desktop/en-us/sortgroup_sets_create.htm)
2. [Analytics Vidhya - Intermediate Tableau guide for data science and business intelligence professionals](https://www.analyticsvidhya.com/blog/2018/01/tableau-for-intermediate-data-science/)
3. [Hands-On Guide to Tableau Sets](https://www.absentdata.com/sets-in-tableau/)