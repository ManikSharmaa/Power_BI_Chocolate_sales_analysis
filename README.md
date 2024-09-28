# Power BI Chocolates Sales Analysis Dashboard

### Dashboard Link : 

## Problem Statement

The company faces a noticeable decline in key performance indicators (KPIs) across multiple dimensions of the chocolate sales process. Over the past month, the company experienced:

- A **10.8% drop in total sales**, with only **2.53M units sold** this month compared to the previous.

- A significant **17.5% decline in the number of boxes sold (143K)**, impacting overall revenue generation.

- A **19.3% reduction in total costs (926.6K)**, which, though reducing expenses, reflects a possible slowdown in production and distribution activities.

- A **4.9% decrease in profits (1.60M this month)**, indicating a less efficient sales operation.

- Total shipments **decreased by 10.6% (with only 449 shipments this month)**, signaling potential supply chain issues or market demand reduction.

Additionally, the salespersons’ performance varies considerably, with the top performers achieving profit margins between 60%-65%, while others show room for improvement. 

The "Shipments Analysis" also reveals that shipment volumes are concentrated in lower quantities, raising concerns about underperformance in certain regions or products.

**This dashboard helps identify trends and concerns across sales, costs, and shipments, making it imperative to address the root causes of declining sales, reduced shipments, and profit stagnation, as well as to optimize overall sales team performance.**

### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, **check "column distribution", "column quality" & "column profile" options.**


![image](https://github.com/user-attachments/assets/bb12d9f1-abf4-4196-b7cc-cdbb77f30297)

- Step 3 : It was observed that in the dataset there were alot of columns with wrong datatypes so corrected all the columnns and added proper datatypes for each column. 
- Step 4 : In the Shipments table I added few columns i.e. **Cost Per Box and Cost Per shipment** from the power query with the help of some merging queries and by creating custom column. 

![image](https://github.com/user-attachments/assets/0dd6d2d5-a550-49d3-9616-3d45fe9298bc)

![image](https://github.com/user-attachments/assets/03ff6dd6-2d5d-4c3a-8fa5-89864b65625f)

![image](https://github.com/user-attachments/assets/214db157-5559-402d-add8-29ce82ee376a)


- Step 5 : Then I modified the **Calender Table** by adding more columns to it. From the **Date** column, I extracted multiple differnt columns for the ease of doing analysis.
Columns that were created are ** Year, Month, Month Name, Day of Week, Day Name and Start of Month**

![image](https://github.com/user-attachments/assets/48dbc186-d953-445b-880c-73354c6557ca)


- Step 6 : Later I created the star schema for the dataset defining the **Shipments** table as the Fact Table and other tables as its dimension table. Making a relationship between each table for a smooth interaction between the tables.
![image](https://github.com/user-attachments/assets/cd62892b-9313-4bb5-8644-bac2b7790541)


- Step 7 : After doing all the Data Modelling and Data Cleaning Tasks, I created **_measures Table and _Latest_measures Table**

- Step 8 : Using DAX functions, different measures were created in both _measures and _Latest_measures table. 

Measures such as **Total Sales, Costs, Profit, Boxes , Shipments etc were added in the *_measures table* along with Month-on-Month% change in above mentioned category.**

![image](https://github.com/user-attachments/assets/02fa2ead-9bf1-40bf-84b5-c69d34095c10)




 



 Whereas Measures that show the latest changes, like **Latest Profit, boxes, cost, shipment etc along with Latest Month-on-Month% of the above mentioned categories are included in *_Latest_measures table*.**

 ![image](https://github.com/user-attachments/assets/b6a045c3-9b46-4d02-bd96-0f5d2346a92e)  

 I also created measures named as **LBS count and LBS%**, where LBS stands for *Low Box Shipment*. Every shipment that is less than 50 boxes comes into **LBS category**. 


## Some measures DAX:

- For Lastest Cost Change:

        Latest Cost = 
        var ld= _Latest_measures[Latest Date]
        return CALCULATE([Total Cost], 'calendar'[Start of Month]= ld)
  
- For Latest Cost% Change:
      
        Cost % change = 
        var ld= _Latest_measures[Latest Date]
        var this_month = _Latest_measures[Latest Cost]
        var prev_month = CALCULATE([Total Cost], 'calendar'[Start of Month]= EDATE(ld,-1))
        return divide(this_month - prev_month, prev_month)

- Latest MoM Box Change %:

        Latest MoM Box Change% = 
        var ld= _Latest_measures[Latest Date]
        var this_month = _Latest_measures[Latest Boxes]
        var prev_month = CALCULATE([Total Boxes], 'calendar'[Start of Month]= EDATE(ld,-1))
        return DIVIDE(this_month - prev_month, prev_month)

- LBS Count:

        LBS Count = CALCULATE(COUNTROWS(shipments), shipments[Boxes]< 50)


- MoM Profit Change %:

        MoM Profit change% = 
        var this_month = [Total Profit]
        var prev_month = CALCULATE([Total Profit], PREVIOUSMONTH('calendar'[Date]))
        RETURN
        DIVIDE(this_month - prev_month, prev_month)

- Previous Month Sales:

        Prev Month sales = CALCULATE([Total Sales], PREVIOUSMONTH('calendar'[Date]))

***and many more included in the process.***

- Step 9 : I started the visual work, by creating different **cards** for each category, and a **gauge** representing the profit percentage where the profit benchmark is shown as **60%.**
![image](https://github.com/user-attachments/assets/6f819f3e-209e-4a4e-ab4c-8affcaf1b7e6)

- Step 10 : After that I created a **Field Parameter** and integrated it with a ***slicer*** that represented all the 5 categories of our study, i.e. **Sales, Profit, Cost, Boxes, Shipments.**

And along with the slicer, I created a line graph and linked it with the Field Parameter , on the basis of **Start of Month** column from the calender table.

![image](https://github.com/user-attachments/assets/b4e66977-0a81-4e2b-89a3-4e5395301279)


And added a tooltip functionality on the line graph that provides us a breakthrough of all info of all the locations with the help of a **donut chart.**

![image](https://github.com/user-attachments/assets/686baed5-bcbd-45bc-9db9-235b8b89696f)

- Step 11: A Stacked column chart was created for the analysis of Shipments of the box , where I created the bins of boxes of size 20, to represent it in a Histogram form. 

![bins](https://github.com/user-attachments/assets/a07b80e7-08dc-47c5-aef4-12f6c4f8fe27)


Along with the Stacked Column Chart, I also added a **gauge** to represent the **LBS %age.**
![image](https://github.com/user-attachments/assets/c72acc53-45df-45ab-99c1-ea6d2f0b3825)
![image](https://github.com/user-attachments/assets/8b02c46f-05fb-4854-a406-982350103ba7)



- Step 12: Then I created a table that represents the **Sales person and Products data.** This Table acts as a multi-functional table as it uses the concept of **Bookmarks**. 
***Data swiches where any of the buttons are clicked.***


![image](https://github.com/user-attachments/assets/504f2cd7-ea72-4fb4-8b40-745c235955d6)


![image](https://github.com/user-attachments/assets/52252e12-a30b-4171-bad8-e129738dc19f)

- Step 13: Further formatting of visuals were done and add-up of some more functionalities. 


### Report Snapshot (Power BI DESKTOP)
 
 ![image](https://github.com/user-attachments/assets/2ff18717-4ed5-40fa-88ce-d16741a56c25)


Snapshot representing Tooltip utilization while hovering on the markers of **Line Graph chart.**


![image](https://github.com/user-attachments/assets/c0cdf3dd-eabe-4027-8528-f83bc06468fe)
