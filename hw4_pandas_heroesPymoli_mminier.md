
## Pandas Homework: Heroes of Pymoli Report
### 03/20/2018


```python
# Import packages
import numpy as np
import pandas as pd

# Read in JSON file and create dataframe
file = "purchase_data.json"
df = pd.read_json(file)

# Confirm type
type(df)

# Export to excel
# df.to_excel('pymoli_data.xlsx')

# Inspect dataframe
# df.info
df.head()
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



### 1. Player Count


```python
# 1.1 Count unique number of players
tp = len(df['SN'].unique())

# Convert to Series
s = pd.Series(tp, name='Total Players')
s

# Print solution
df1 = pd.DataFrame(s)
df1
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
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



### 2. Purchasing Analysis (Total)


```python
# 2.1 Count unique number of items
unique = len(df['Item ID'].unique())
col1 = pd.Series(unique, name="Number of Unique Items")

# 2.2 Average Purchase Price
price_avg_tot = round(np.mean(df['Price']),2)
col2 = pd.Series(price_avg_tot, name="Average Purchase Price")

# 2.3 Total Number of Purchases-----------------------------------------------------------------------------
purch_tot = df['Item ID'].count()
col3 = pd.Series(purch_tot, name="Total Number of Purchases")

# 2.4 Total Revenue
rev_tot = np.sum(df['Price'])
col4 = pd.Series(rev_tot, name="Total Revenue")

# Join all in a single dataframe
df2 = pd.DataFrame(col1)
df2 = df2.join(col2)
df2 = df2.join(col3)
df2 = df2.join(col4)

# Print solution
df2
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
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
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
# 3-- Gender Demographics

df3 = df.groupby('SN')

# 3.1 Filter unique screen names in df2
df_gen = df3['Gender'].unique()

# 3.2 Count totals by gender
gen_ct = df_gen.value_counts()
gen_ct

# 3.3 Calculate percentage by gender
gen_pct = round(gen_ct / unique, 2)
gen_pct

# Join together in dataframe
df3 = pd.DataFrame({"Percentage" : gen_pct,
                      "Total Count" : gen_ct})

# Print solution
df3
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
      <th>Percentage</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>[Male]</th>
      <td>2.54</td>
      <td>465</td>
    </tr>
    <tr>
      <th>[Female]</th>
      <td>0.55</td>
      <td>100</td>
    </tr>
    <tr>
      <th>[Other / Non-Disclosed]</th>
      <td>0.04</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



### 4. Purchasing Analysis (Gender)


```python
# Group by gender and count purchases
gp_pur = df.groupby('Gender').count()
gp_pur

# Create list for Purchase Count
ls_pur = gp_pur['Price']
ls_pur

# Group by gender and find average
gp_avg = df.groupby('Gender').mean()
gp_avg

# Create list for Average Price
ls_avg = round(gp_avg['Price'],2)
ls_avg

# Group by gender and find Total Value
gp_sum = df.groupby('Gender').sum()
gp_sum

# Create list
ls_sum = gp_sum['Price']
ls_sum

# Convert unique count by gender to Series
unique = pd.Series(gen_ct, name = "Total People")
unique

un_count = pd.Series([100, 465, 8])
nf = round(382.91/100, 2)
nm = round(1867.68/465, 2)
no = round(35.74/8, 2)

norm = [nf, nm, no]
normalized = pd.Series(norm, name = "Normalized Totals")

df4 = pd.DataFrame(np.column_stack([ls_pur, ls_avg, ls_sum, normalized]), 
                               columns=['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals'], 
                                 index=['Female', 'Male', 'Other / Non-Disclosed'])

# Print solution
df4
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136.0</td>
      <td>2.82</td>
      <td>382.91</td>
      <td>3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633.0</td>
      <td>2.95</td>
      <td>1867.68</td>
      <td>4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11.0</td>
      <td>3.25</td>
      <td>35.74</td>
      <td>4.47</td>
    </tr>
  </tbody>
</table>
</div>



### 5. Age Demographics


```python
df['Age'].agg([min, max])

# Create the bins in which Data will be held
bins = [0,10,14,19,24,29,34,39,44,49]

# Create the names for the bins
bin_labels = ["<10", "10-14", "15-19", "20-24","25-29", "30-34","35-39", "40-44", "45-49"]

# Insert new column
df["Age Groups"] = pd.cut(df["Age"], bins, labels=bin_labels)
df.head()

# Calculate
count5 = df.groupby(['Age Groups']).count()["Price"]
avg5 = round(df.groupby(['Age Groups']).mean()["Price"],2)
sum5 = df.groupby(['Age Groups']).sum()["Price"]
norm5 = round(sum5 / count5, 2)

# Add to dataframe
df50 = pd.DataFrame({"Purchase Count":count5, "Average Purchase Price": avg5, 
                     "Total Purchase Value": sum5, "Normalized Totals": norm5})
# Print solution
df50
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
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Groups</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.02</td>
      <td>3.02</td>
      <td>32</td>
      <td>96.62</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>2.70</td>
      <td>2.70</td>
      <td>31</td>
      <td>83.79</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>2.91</td>
      <td>2.91</td>
      <td>133</td>
      <td>386.42</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>2.91</td>
      <td>2.91</td>
      <td>336</td>
      <td>978.77</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>2.96</td>
      <td>2.96</td>
      <td>125</td>
      <td>370.33</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>3.08</td>
      <td>3.08</td>
      <td>64</td>
      <td>197.25</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>2.84</td>
      <td>2.84</td>
      <td>42</td>
      <td>119.40</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>3.19</td>
      <td>3.19</td>
      <td>16</td>
      <td>51.03</td>
    </tr>
    <tr>
      <th>45-49</th>
      <td>2.72</td>
      <td>2.72</td>
      <td>1</td>
      <td>2.72</td>
    </tr>
  </tbody>
</table>
</div>



### 6. Top Spenders

    # 5 top spenders in the game by total purchase value (group by 'SN', then sum 'Price')
    # list SN, Purchase Count (count of 'price'), Average Purchase Price (avg 'price'), Total Purchase Value (sum 'price')


```python
# Calculate
df6 = df.groupby(['SN']).sum()["Price"]
count = df.groupby(['SN']).count()["Item Name"]
avg = round(df.groupby(['SN']).mean()["Price"],2)

# Add to dataframe
df60 = pd.DataFrame({"Total Purchase Value":df6, "Purchase Count": count, "Average Purchase Price": avg})

# Sort by Total Purchase Value
df60.sort_values("Total Purchase Value", ascending=False).head()
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>3.41</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3.39</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>3.18</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>4.24</td>
      <td>3</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3.86</td>
      <td>3</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>



### 7. Most Popular Items

    # 5 most popular items by purchase count (groupby item id, then count 'price')
    # list Item ID, Item Name, Purchase Count [count of 'price'], Item Price, Total Purchase Value [sum of 'price']


```python
# Calculate
tot_items = df.groupby(["Item ID", "Item Name"]).count()["Price"]
df_item_ct = df.groupby(["Item ID", "Item Name"]).count()["Price"]
df_item_rev = df.groupby(["Item ID", "Item Name"]).sum()["Price"]

# Add to dataframe
df8 = pd.DataFrame({"Purchase Count":tot_items, "Total Purchase Value":df_item_rev})

# Sort by Purchase Count
df8.sort_values("Purchase Count", ascending=False).head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>



### 8. Most Profitable Items

    # 5 most profitable items by total purchase value (groupby item id, then sum 'price')
    # list Item ID, Item Name, Purchase Count [count of 'price'], Item Price, Total Purchase Value [sum of 'price']


```python
# Calculate
df_item = df.groupby(["Item ID", "Item Name"]).sum()["Price"]
df_item_ct = df.groupby(["Item ID", "Item Name"]).count()["Price"]
df_item_rev = df.groupby(["Item ID", "Item Name"]).sum()["Price"]

# Add to dataframe
df8 = pd.DataFrame({"Purchase Count":df_item_ct, "Total Purchase Value":df_item_rev})

# Sort by Total Purchase Value
df8.sort_values("Total Purchase Value", ascending=False).head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>



### FINAL CONSIDERATIONS

    # Export markdown version of notebook called README.md

### 3 observable trends

    # 20-24 age group represents the highest sales volume and revenue.
    # The average spend per purchase and normalized totals do not vary significantly between age or gender demographics.
    # Perhaps, not surprisingly, males spent more than 4.5 times females on purchases.
