# Working with sets in Tableau

__Sets__ in Tableau are used to create subsets of data based on certain conditions defined by the user. Types of sets: __dynamic set__, __fixed set__ and __combined set__. In Tableau, sets can only be created based on dimension fields.

Here the following topics will be covered:
1. Creating a dynamic set
2. Creating a fixed set
3. Create a combined set
4. Using set actions to make sets more dynamic and interactive

__[Global superstore Data used for this blog can be downloaded from here](https://1drv.ms/x/s!Al_62RL1SdJmiUZ17zQ5m6hokJLu?e=WZciJa)__  

## 1. Creating a dynamic set

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


## 2. Creating a fixed set

The members of a fixed set do not change. A fixed set can be based on a single dimension or multiple dimensions. This is particularly useful when we want to focus particularly on selected memebers of considered dimensions.

To create a fixed set:

1. Select required values of a dimension __Product Name__. And then either right click on selected values or on hovering we will get the follwong box with an icon for __Create set__ with Name _Interested Products_.

![setf](../../images/tableau/working_with_sets/7.select_dimension.png)

2. Checking the _Exclude_ option will exclude the selected members of the dimension from the set. Can also directly send the set to filters shelf by checking the _Add to filters shelf_. Then click OK to create the set. The new set is added to the bottom of the Data pane, under the Sets section.

 ![setf](../../images/tableau/working_with_sets/8_set.PNG)

3. Example - To see the sale of our interested items in the different regions of sale of global superstore.
![setf](../../images/tableau/working_with_sets/9.check_fset.PNG)

## 3. Creating a combined set

To see the states which are in the top 10 of both highest sales and highest profits.

1. Created a set which has top 10 states with the highest sales in US-
![highest_sales](../../images/tableau/working_with_sets/10_states_highest_sales.PNG)

2. Created a set which has top 10 states with highest profits in US-
![highest_profit](../../images/tableau/working_with_sets/11_states_highest_profit.PNG)

3. Resultant sets in the sets section
![resultant_sets](../../images/tableau/working_with_sets/12_resultant_set.PNG)

4. Right click on either of the 2 sets and select _create combined set_.
![combined_sets](../../images/tableau/working_with_sets/13_combined_set.png)

5. Combined set creation dialog box
![dialog_box](../../images/tableau/working_with_sets/14_states_highest_profit_sales.PNG)

6. Result of combined set has states common to both in the sets with 
![combined_set](../../images/tableau/working_with_sets/15_combined_set_result.PNG)


## 4. Using set actions to make sets more dynamic and interactive

With __Set Actions__, it is possible to dynamically pass dimension values to existing sets based on value selection from other visualizations by user.














***

### Reference:
1. [Tableau help: Create Sets](https://help.tableau.com/current/pro/desktop/en-us/sortgroup_sets_create.htm)
2. [Analytics Vidhya - Intermediate Tableau guide for data science and business intelligence professionals](https://www.analyticsvidhya.com/blog/2018/01/tableau-for-intermediate-data-science/)
3. [Hands-On Guide to Tableau Sets](https://www.absentdata.com/sets-in-tableau/)
4. [Using set actions in Tableau](https://visualbi.com/blogs/tableau/set-actions-tableau/)