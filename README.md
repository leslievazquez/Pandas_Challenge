<p align="center">
  <img width="1980" height="250" src="https://github.com/leslievazquez/Pandas_Challenge/blob/main/HeroesOfPymoli/Resources/Fantasy.png">
</p>

<h1 align ="center"><span>Heroes of Pymoli<br/>Data Analysis</span></h1>

The purpose of this data analysis is to identify trends in the purchase history of the players of the fantasy game "Heroes of Pymoli".

The data analysis for "Heroes of Pymoli" was performed using pandas through Jupyter Notebook. The dataset that was provided as a .csv file contained the purchase history of the players, items purchased and cost. The content of the data was organized in the following columns:

`| Purchase | ID | SN | Age | Gender | Item ID | Item Name | Price |`

The first data collected in this analysis was the count of players. It was noted that some players made multiple purchases. Therefore, it was very important to drop duplicate players (SN). 

After obtaining the count of players (576), an analysis on the purchasing total was performed to count the number of unique items, obtain a count on the number of purchases and calculate the total revenue. The following results were obtained:

| Number of Unique Items | Average Price | Number of Purchases | Total Revenue |
| ---------------------- | ------------- | ------------------- | ------------- |
|                    179 | $3.05	       |      780            |	$2,379.77    |

The age and gender of each player was included in the dataset provided. Therefore, additional analysis regarding the purchases performed by gender and age was included in this data analysis.

The last part of this analysis identifies the top spenders by player name and the most popular and profitable items. 

Below are some observable trends:
- There are 576 players which are predominantly males (84%). Females represent 14% of the data. This significant difference in gender percentage is evident when analyzing the total purchase value by gender where males spent \$1967.64 compared to the \$361.94 spent by females.
- The largest age group is 20-24 (46.8%) followed by 15-19 (17.4%) and 25-29 (13%). The age group 20-24 also represents the largest group of spenders which is slightly under 50% of the total revenue. The top spender has a relatively low purchase count of 5 items, which demonstrates that most of the revenue comes from single-item purchases with higher prices.
- The ‘Average Purchase’ and ‘Average Price’ are slightly close with approximately $1.00 difference. Four of the most popular items of 179 unique items represent 7% of the total revenue. These 4 items are priced approximately 30% higher than the average price. The fifth most popular item is one of the lowest priced items. The most popular and most profitable items lists are similar, thus reinforcing that higher prices do not deter the players’ spending habits.

Additional information including the summary tables for this analysis is located in the folder "HeroesOfPymoli" within this repository in the file `HeroesOfPymoli.md`. 


 &nbsp; &nbsp;

  
  

<p align="center">
  <img width="1980" height="250" src="https://github.com/leslievazquez/Pandas_Challenge/blob/main/HeroesOfPymoli/Resources/Fantasy2.jpg">
</p>
