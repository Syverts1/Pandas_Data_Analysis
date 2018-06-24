# Data Analysis of Fantasy Game "Heroes of Pymoli"

This project involves analyzing purchasing patterns for a dummy video game company called "Heroes of Pymoli". Analysis is done about:
- Player count
- Purchasing analysis (total)
- Gender demographics
- Purchasing analysis (by gender)
- Age demographics
- Top spenders
- Most popular items
- Most profitable items

Observed Trends:
- In both datasets, there are more men than women or other/non-disclosed genders purchasing items in the game
- In both datasets, the most items were purchased within the 20-24 age group
- In both datasets, kids under 10 were purchasing more items than people over 40


#### Markdown of Jupyter Notebook File

```python
#Dependencies
import pandas as pd
import numpy as np
import os
```


```python
filename = input("file name:")
```

    file name:purchase_data.json
    


```python
# Set path for file
json_path = os.path.join("Resources", str(filename))
```


```python
pymoli_data = pd.read_json(json_path)
pymoli_data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Player Count
player_count = pymoli_data["SN"].nunique()
player_frame = pd.DataFrame({"Total Players": [player_count]})
player_frame
```




<div>
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
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Total)

#Number of Unique Items
unique_items= pymoli_data["Item ID"].nunique()

#Average Purchase Price
avg_price = round(pymoli_data["Price"].mean(),2)

#Total Number of Purchases
tot_purch = len(pymoli_data)

#Total Revenue
revenue = round(pymoli_data["Price"].sum(),2)

purch_analysis = pd.DataFrame(
    {"Number of Unique Items": [unique_items],
    "Average Price": [avg_price],
    "Number of Purchases": [tot_purch],
    "Total Revenue": [revenue]})
purch_analysis= purch_analysis[["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"]]
purch_analysis
```




<div>
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
      <td>183</td>
      <td>2.93</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics

grouped_gender=pymoli_data.groupby(["Gender"])
count_gender = grouped_gender["SN"].nunique()

perc_gender = round((count_gender/player_count)*100,2)
gender = pd.DataFrame({"Total Count":count_gender,"Percentage of Players":perc_gender})
gender
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender)
purchase_count = grouped_gender["SN"].count()
avg_price = round(grouped_gender["Price"].mean(),2)
tot_price = grouped_gender["Price"].sum()
norm_totals = round(tot_price/count_gender,2)
purch_analysis = pd.DataFrame({"Purchase Count": purchase_count,
                              "Average Purchase Price": avg_price,
                              "Total Purchase Value": tot_price,
                              "Normalized Totals": norm_totals})
purch_analysis = purch_analysis[["Purchase Count","Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
purch_analysis
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.82</td>
      <td>382.91</td>
      <td>3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.95</td>
      <td>1867.68</td>
      <td>4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.25</td>
      <td>35.74</td>
      <td>4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Bins for Age Demographics
bins = [0]
bin_value = ["<10"]
max_age = pymoli_data["Age"].max()

for x in range(2,int(max_age/5)+2):
    bins = bins + [(x*5)-1]

for x in range(1,len(bins)-2):
    bin_value = bin_value + [str(bins[x]+1)+"-"+str(bins[x+1])]

bin_value = bin_value + [str(max_age) + "+"]
#print(bins)
#print(bin_value)

#Age Demographics - need to group by SN.. add back in age, gender.. groupby age group (count and percentage)
pymoli_data["Age Group"] = pd.cut(pymoli_data["Age"],bins,labels = bin_value)

```


```python
#Purchasing Analysis (Age)
age_group = pymoli_data.groupby(["Age Group"])
age_count = age_group.size()
age_avg_price = round(age_group["Price"].mean(),2)
age_tot_value = age_group["Price"].sum()
age_norm_tot = round(age_tot_value/age_count,2)
age_dataframe = pd.DataFrame({"Purchase Count":age_count,
                             "Average Purchase Price": age_avg_price,
                             "Total Purchase Value": age_tot_value,
                             "Normalized Totals": age_norm_tot})

age_dataframe = age_dataframe[["Purchase Count","Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
age_dataframe
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>2.98</td>
      <td>83.46</td>
      <td>2.98</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>2.77</td>
      <td>96.95</td>
      <td>2.77</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>2.91</td>
      <td>386.42</td>
      <td>2.91</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>2.91</td>
      <td>978.77</td>
      <td>2.91</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>2.96</td>
      <td>370.33</td>
      <td>2.96</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>3.08</td>
      <td>197.25</td>
      <td>3.08</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>2.84</td>
      <td>119.40</td>
      <td>2.84</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>16</td>
      <td>3.19</td>
      <td>51.03</td>
      <td>3.19</td>
    </tr>
    <tr>
      <th>45+</th>
      <td>1</td>
      <td>2.72</td>
      <td>2.72</td>
      <td>2.72</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_spend_group = pymoli_data.groupby("SN")
top_spend_count = top_spend_group.size()
top_spend_avg = round(top_spend_group["Price"].mean(),2)
top_spenders = top_spend_group["Price"].sum()
top_spenders_frame = pd.DataFrame({"Total Purchase Value": top_spenders})

#Identify top 5 spenders
top_spenders_sort = top_spenders_frame.sort_values(by="Total Purchase Value",
                                                  ascending = False)
top_spenders_filter = top_spenders_sort.iloc[0:5].reset_index()

#join spenders with other calculations
top_spend_func_frame = pd.DataFrame({"Purchase Count": top_spend_count,
                                     "Average Purchase Price": top_spend_avg}).reset_index()

top_spenders_final = pd.merge(top_spenders_filter, top_spend_func_frame, on="SN")
top_spenders_final
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SN</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>17.06</td>
      <td>3.41</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Saedue76</td>
      <td>13.56</td>
      <td>3.39</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mindimnya67</td>
      <td>12.74</td>
      <td>3.18</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Haellysu29</td>
      <td>12.73</td>
      <td>4.24</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eoda93</td>
      <td>11.58</td>
      <td>3.86</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_items = pymoli_data.groupby("Item ID")
top_items_group2 = pymoli_data.groupby(["Item ID","Item Name","Price"])
top_items_count = top_items.size()
top_items_value = top_items_group2["Price"].sum()
top_items_frame = pd.DataFrame({"Purchase Count": top_items_count})
#Identify most popular items
top_items_sort = top_items_frame.sort_values(by="Purchase Count",
                                                  ascending = False).reset_index()


end = top_items_sort["Purchase Count"].size

lastrow = 0

for row in range(4, end):
    if(top_items_sort.get_value(row,"Purchase Count") != top_items_sort.get_value(row + 1,"Purchase Count")):
        lastrow = row
        break

if lastrow != 4:
    print("Note: As a result of ties, more than 5 items are the most popular. The tied values are printed")

top_items_filter = top_items_sort.iloc[0:lastrow+1].reset_index()

top_items_func_frame = pd.DataFrame({"Total Purchase Value": top_items_value}).reset_index()

#merge tables
top_items_final = pd.merge(top_items_filter, top_items_func_frame, on="Item ID")

#fix column order and price name
top_items_final = top_items_final[["Item ID","Item Name","Purchase Count","Price","Total Purchase Value"]]
top_items_final = top_items_final.rename(columns={"Price":"Item Price"})
top_items_final
```

    Note: As a result of ties, more than 5 items are the most popular. The tied values are printed
    




<div>
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
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31</td>
      <td>Trickster</td>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>3</th>
      <td>175</td>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>Serenity</td>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
    </tr>
    <tr>
      <th>5</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items
top_values_frame = pd.DataFrame({"Total Purchase Value": top_items_value})
top_values_sort = top_values_frame.sort_values(by="Total Purchase Value",
                                                  ascending = False)
top_values_filter = top_values_sort.iloc[0:5].reset_index()
top_values_final = pd.merge(top_values_filter,top_items_sort, on="Item ID")

#rearrange columns and rename price column
top_values_final = top_values_final[["Item ID","Item Name","Purchase Count","Price","Total Purchase Value"]]
top_values_final = top_values_final.rename(columns={"Price":"Item Price"})
print()
top_values_final
```

    
    




<div>
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
      <td>34</td>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>3</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


