# Data Analysis: Heroes of Pymoli

### Trends

- There are 576 players which are predominantly males (84%). Females represent 14% of the data. This significant difference in gender percentage is evident when analyzing the total purchase value by gender where males spent \\$1967.64 compared to the \\$361.94 spent by females. 
- The largest age group is 20-24 (46.8%) followed by 15-19 (17.4%) and 25-29 (13%). The age group 20-24 also represents the largest group of spenders which is slightly under 50% of the total revenue. The top spender has a relatively low purchase count of 5 items, which demonstrates that most of the revenue comes from single-item purchases with higher prices.  
- The ‘Average Purchase’ and ‘Average Price’ are slightly close with approximately $1.00 difference. Four of the most popular items of 179 unique items represent 7% of the total revenue. These 4 items are priced approximately 30% higher than the average price. The fifth most popular item is one of the lowest priced items. The most popular and most profitable items lists are similar, thus reinforcing that higher prices do not deter the players’ spending habits. 




```python
#Import Pandas Library 
import pandas as pd 
```


```python
# Set CSV path to import data 
csv_path = "Resources/purchase_data.csv"

# Read the CSV into a Pandas DataFrame
data = pd.read_csv(csv_path)

# Display columns for easy reference 
data.head(0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
# Check if there is any null entry in the data
data.isnull().sum()
```




    Purchase ID    0
    SN             0
    Age            0
    Gender         0
    Item ID        0
    Item Name      0
    Price          0
    dtype: int64



### Player Count


```python
# Count the total number of players
player_count = len(data["SN"].unique())
player_count

# Display the total number of players in dataframe
total = pd.DataFrame({"Total Players" :[player_count]})
total
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



 ### Purchasing Analysis  (Total)


```python
# Identify unique items and drop any duplicates 
items = data['Item ID'].drop_duplicates(keep='first')

# Count the total of unique items 
items_count = len(items)

# Calculate the average price 
average_price = round(data["Price"].mean(),2)

# Count the total number of purchases
number_purchases = data["Purchase ID"].count()

# Calculate the total revenue 
total_revenue = data["Price"].sum()

# Display results in dataframe 
purchasing_total = pd.DataFrame({"Number of Unique Items": [items_count],
                            "Average Price": [average_price],
                            "Number of Purchases": [number_purchases],
                            "Total Revenue": [total_revenue]})

# Change format of 'Average Price' and 'Total Revenue' to currency 
purchasing_total [["Average Price","Total Revenue"]] \
= purchasing_total [["Average Price","Total Revenue"]].applymap("${:,.2f}".format)

purchasing_total
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



### Gender Demographics


```python
# Drop any duplicates 
players = data[["Gender", "SN"]].drop_duplicates(keep='first')

# Count the amount of players by gender
gender_count = players["Gender"].value_counts()

# Calculate the percentage of players by gender
gender_percent = (round(gender_count / players["Gender"].count() * 100, 2))

# Display the gender demographics in a table
gender_demo = pd.DataFrame({"Total Count": gender_count,
                          "Percentage of Players" : gender_percent})

# Change the format 'Percentage of Players' to percentage
gender_demo["Percentage of Players"] = gender_demo["Percentage of Players"].apply("{0:.2f}%".format)

# Rename the axis to show data label "Gender"
gender_demo = gender_demo.rename_axis("Gender")

gender_demo.reset_index()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>484</td>
      <td>84.03%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>81</td>
      <td>14.06%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>11</td>
      <td>1.91%</td>
    </tr>
  </tbody>
</table>
</div>



###  Purchasing Analysis (Gender)


```python
# Count the amount of purchases by gender
purch_count_gender = data.groupby("Gender")['SN'].count()

# Calculate the average purchase price
avg_price_gender = data.groupby(["Gender"])["Price"].mean()

# Calculate the total purchase value 
purch_tot_gender = data.groupby(["Gender"])['Price'].sum()

# Drop any duplicates 
duplicates = data.drop_duplicates(subset='SN', keep="first")
grouped_dup = duplicates.groupby(["Gender"])

# Calculate the average purchase total per person by gender
avg_tot_gender = purch_tot_gender / grouped_dup["SN"].count()

# Display results in DataFrame 
purchasing_gender = pd.DataFrame({"Purchase Count": purch_count_gender,
                                  "Average Purchase Price": avg_price_gender,
                                  "Total Purchase Value": purch_tot_gender,
                                  "Avg Total Purchase per Person": avg_tot_gender})

# Format values 
purchasing_gender["Average Purchase Price"] = purchasing_gender["Average Purchase Price"].apply("${:.2f}".format)
purchasing_gender["Total Purchase Value"] = purchasing_gender["Total Purchase Value"].apply("${:.2f}".format)
purchasing_gender["Avg Total Purchase per Person"] = purchasing_gender["Avg Total Purchase per Person"].apply("${:.2f}".format)
purchasing_gender = purchasing_gender[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Avg Total Purchase per Person"]]

purchasing_gender.reset_index()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase per Person</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



### Age Demographics


```python
# Drop any duplicates 
players = data[["Age", "SN"]].drop_duplicates()

# Establish bins for ages and age group
bins = [0,9,14,19,24,29,34,39,100]
age_group = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]

# Categorize the existing players using the age bins. 
players['Age Range'] = pd.cut(players['Age'], bins)

# Calculate the numbers and percentages by age group
age_count = players["Age Range"].value_counts()
age_percent = ((age_count/players["SN"].count()) * 100).round(2)

# Display results in DataFrame and format Percentage of Players values to percent %
players = pd.DataFrame({'Age Ranges': age_group, 'Total Count': age_count,'Percentage of Players': age_percent})
players['Percentage of Players'] = players['Percentage of Players'].apply('{:.2f}%'.format)

players.style.hide_index()
```




<style  type="text/css" >
</style><table id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8" ><thead>    <tr>        <th class="col_heading level0 col0" >Age Ranges</th>        <th class="col_heading level0 col1" >Total Count</th>        <th class="col_heading level0 col2" >Percentage of Players</th>    </tr></thead><tbody>
                <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row0_col0" class="data row0 col0" ><10</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row0_col1" class="data row0 col1" >258</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row0_col2" class="data row0 col2" >44.79%</td>
            </tr>
            <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row1_col0" class="data row1 col0" >10-14</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row1_col1" class="data row1 col1" >107</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row1_col2" class="data row1 col2" >18.58%</td>
            </tr>
            <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row2_col0" class="data row2 col0" >15-19</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row2_col1" class="data row2 col1" >77</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row2_col2" class="data row2 col2" >13.37%</td>
            </tr>
            <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row3_col0" class="data row3 col0" >20-24</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row3_col1" class="data row3 col1" >52</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row3_col2" class="data row3 col2" >9.03%</td>
            </tr>
            <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row4_col0" class="data row4 col0" >25-29</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row4_col1" class="data row4 col1" >31</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row4_col2" class="data row4 col2" >5.38%</td>
            </tr>
            <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row5_col0" class="data row5 col0" >30-34</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row5_col1" class="data row5 col1" >22</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row5_col2" class="data row5 col2" >3.82%</td>
            </tr>
            <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row6_col0" class="data row6 col0" >35-39</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row6_col1" class="data row6 col1" >17</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row6_col2" class="data row6 col2" >2.95%</td>
            </tr>
            <tr>
                                <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row7_col0" class="data row7 col0" >40+</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row7_col1" class="data row7 col1" >12</td>
                        <td id="T_c0ec148a_0d5e_11eb_b98d_d8c4970340e8row7_col2" class="data row7 col2" >2.08%</td>
            </tr>
    </tbody></table>



### Purchasing Analysis (Age)


```python
# Categorize the players using the age bins. 
data['Age Ranges'] = pd.cut(data['Age'], bins)
age_count = data["Age Ranges"].value_counts()

# Count the amount of purchases by age
purch_count_age = data.groupby("Age Ranges")['SN'].count()

# Calculate the average purchase price 
avg_price_age = data.groupby("Age Ranges")['Price'].mean()

# Calculate the total purchase value 
purch_tot_age = data.groupby("Age Ranges")['Price'].sum()

# Drop any duplicates
duplicates = data.drop_duplicates(subset='SN', keep="first")
age_dup = duplicates.groupby(["Age Ranges"])

# Calculate the average purchase total per person by gender
avg_tot_age = (purch_tot_age / age_dup["SN"].count())

# Display results in DataFrame 
purchasing_age = pd.DataFrame({'Age Ranges': age_group, 
                               'Purchase Count': purch_count_age,
                               'Average Purchase Price': avg_price_age,
                               'Total Purchase Value': purch_tot_age, 
                               'Avg Total Purchase per Person': avg_tot_age})


# Format values 
purchasing_age["Average Purchase Price"] = purchasing_age["Average Purchase Price"].apply("${:.2f}".format)
purchasing_age["Total Purchase Value"] = purchasing_age["Total Purchase Value"].apply("${:.2f}".format)
purchasing_age["Avg Total Purchase per Person"] = purchasing_age["Avg Total Purchase per Person"].apply("${:.2f}".format)
purchasing_age = purchasing_age[["Age Ranges","Purchase Count", "Average Purchase Price", "Total Purchase Value", "Avg Total Purchase per Person"]]


purchasing_age.style.hide_index()
```




<style  type="text/css" >
</style><table id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8" ><thead>    <tr>        <th class="col_heading level0 col0" >Age Ranges</th>        <th class="col_heading level0 col1" >Purchase Count</th>        <th class="col_heading level0 col2" >Average Purchase Price</th>        <th class="col_heading level0 col3" >Total Purchase Value</th>        <th class="col_heading level0 col4" >Avg Total Purchase per Person</th>    </tr></thead><tbody>
                <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row0_col0" class="data row0 col0" ><10</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row0_col1" class="data row0 col1" >23</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row0_col2" class="data row0 col2" >$3.35</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row0_col3" class="data row0 col3" >$77.13</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row0_col4" class="data row0 col4" >$4.54</td>
            </tr>
            <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row1_col0" class="data row1 col0" >10-14</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row1_col1" class="data row1 col1" >28</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row1_col2" class="data row1 col2" >$2.96</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row1_col3" class="data row1 col3" >$82.78</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row1_col4" class="data row1 col4" >$3.76</td>
            </tr>
            <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row2_col0" class="data row2 col0" >15-19</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row2_col1" class="data row2 col1" >136</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row2_col2" class="data row2 col2" >$3.04</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row2_col3" class="data row2 col3" >$412.89</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row2_col4" class="data row2 col4" >$3.86</td>
            </tr>
            <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row3_col0" class="data row3 col0" >20-24</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row3_col1" class="data row3 col1" >365</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row3_col2" class="data row3 col2" >$3.05</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row3_col3" class="data row3 col3" >$1114.06</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row3_col4" class="data row3 col4" >$4.32</td>
            </tr>
            <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row4_col0" class="data row4 col0" >25-29</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row4_col1" class="data row4 col1" >101</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row4_col2" class="data row4 col2" >$2.90</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row4_col3" class="data row4 col3" >$293.00</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row4_col4" class="data row4 col4" >$3.81</td>
            </tr>
            <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row5_col0" class="data row5 col0" >30-34</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row5_col1" class="data row5 col1" >73</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row5_col2" class="data row5 col2" >$2.93</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row5_col3" class="data row5 col3" >$214.00</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row5_col4" class="data row5 col4" >$4.12</td>
            </tr>
            <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row6_col0" class="data row6 col0" >35-39</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row6_col1" class="data row6 col1" >41</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row6_col2" class="data row6 col2" >$3.60</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row6_col3" class="data row6 col3" >$147.67</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row6_col4" class="data row6 col4" >$4.76</td>
            </tr>
            <tr>
                                <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row7_col0" class="data row7 col0" >40+</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row7_col1" class="data row7 col1" >13</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row7_col2" class="data row7 col2" >$2.94</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row7_col3" class="data row7 col3" >$38.24</td>
                        <td id="T_c0f66b6c_0d5e_11eb_b8cf_d8c4970340e8row7_col4" class="data row7 col4" >$3.19</td>
            </tr>
    </tbody></table>



###  Top Spenders


```python
# Count the frequency of purchases by players
purch_count_sn = data.groupby("SN")["Price"].count()

# Caluculate average price spent by user
cal_purch_price_sn = data.groupby("SN")["Price"].mean()
purch_price_sn = cal_purch_price_sn.map("${:,.2f}".format)

# Calculate total price spent by user
purch_tot_sn = data.groupby("SN")["Price"].sum()

# Display in DataFrame
top_spenders = pd.DataFrame({"Purchase Count": purch_count_sn,
                             "Average Purchase Price": purch_price_sn,
                             "Total Purchase Value": purch_tot_sn})

# Sort by total purchase value (do this before changing the format to currency to avoid changing types to string)
top_spenders = top_spenders.sort_values(by = "Total Purchase Value",ascending = False)

# Change the format to currency
top_spenders["Total Purchase Value"] = top_spenders["Total Purchase Value"].map("${:.2f}".format)

top_spenders.reset_index().head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lisosia93</td>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Idastidru52</td>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Chamjask73</td>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Iral74</td>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Iskadarya95</td>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



### Most Popular Items


```python
# Identify the most popular items by 'Item ID' count
popular_item = data.groupby(["Item ID","Item Name"])["Price"].count()

# Calculate the total sales amount by 'Item ID' and 'Item Name'
sales_tot_item= data.groupby(["Item ID","Item Name"])["Price"].sum()

# Retrieve the item price
item_price = sales_tot_item / popular_item

# Display results in DataFrame
items = pd.DataFrame({"Purchase Count": popular_item,
                      "Item Price" : item_price,
                      "Total Purchase Value": sales_tot_item})

# Sort by purchase count in descending order
items = items.sort_values("Purchase Count", ascending = False)

# Change the format to currency
items[["Item Price", "Total Purchase Value"]] \
= items[["Item Price", "Total Purchase Value"]].applymap("${:.2f}".format)

items.reset_index().head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>92</td>
      <td>Final Critic</td>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>178</td>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>2</th>
      <td>145</td>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>132</td>
      <td>Persuasion</td>
      <td>9</td>
      <td>$3.22</td>
      <td>$28.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
  </tbody>
</table>
</div>



### Most Profitable Items


```python
# Remove formatting from Total Purchase Value and asign type float
items["Total Purchase Value"] = items["Total Purchase Value"].str.replace('$', '')
items["Total Purchase Value"] = pd.to_numeric(items["Total Purchase Value"])

# Sort the above table by total purchase value in descending order
items = items.sort_values("Total Purchase Value", ascending = False)

# Change the format to currency
items["Total Purchase Value"] = items["Total Purchase Value"].apply("${:.2f}".format)

items.reset_index().head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>92</td>
      <td>Final Critic</td>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>178</td>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>2</th>
      <td>82</td>
      <td>Nirvana</td>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>145</td>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


