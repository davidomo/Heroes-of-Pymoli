Heroes of Pymoli Data Analysis

1. The majority of purchases are bought by people aged 15-24, who make up nearly 50% of the player base. However, they do not have the highest purchasing value. This may be explained by their purchasing style of buying more of the elss expensive items.

2. Males make, by a large margin, the main gender to play the game. However, males only spend, on average, $0.19 more than females on the game. People who are non-binary or chose to not disclose their gender in this analysis spend, on average, the most.

3. Though the average price of all items is around $2.93, all age groups excepting players over 40, spent above this average. If the purchasing goal was set at this average, as in, everyone on average would buy one item from the store, this data shows we have met this goal. 

```python
import pandas as pd
```


```python
file = "HeroesOfPymoli/purchase_data.json"
rawdata_df = pd.read_json(file)
rawdata_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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




```python
PlayerCount = len(rawdata_df["SN"].unique())
playercount_disp = pd.DataFrame({"Player Count": [PlayerCount]})
playercount_disp
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Player Count</th>
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
# Data manipulaton
UniqueItems = len(rawdata_df["Item Name"].unique())
AvgPurchase = rawdata_df["Price"].mean()
NumPurchase = len(rawdata_df["Item Name"])
Revenue = rawdata_df["Price"].sum()

# Create new DataFrame
Purchasing_Analysis_Total = pd.DataFrame({"Number of Unique Items": [UniqueItems],
                                           "Average Price": [AvgPurchase],
                                           "Number of Purchases": [NumPurchase],
                                           "Total Revenue": [Revenue]})

# DataFrame formatting
Purchasing_Analysis_Total["Average Price"] = Purchasing_Analysis_Total["Average Price"].map("${:.2f}".format)
Purchasing_Analysis_Total["Total Revenue"] = Purchasing_Analysis_Total["Total Revenue"].map("${:.2f}".format)
Purchasing_Analysis_Total = Purchasing_Analysis_Total[["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]

Purchasing_Analysis_Total
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Data manipulation
duplicate = rawdata_df.drop_duplicates(subset='SN', keep="first")
TotalGen = duplicate["Gender"].count()
MaleGen = duplicate["Gender"].value_counts()['Male']
FemaleGen = duplicate["Gender"].value_counts()['Female']
NonGen = TotalGen - MaleGen - FemaleGen

MalePer = (MaleGen / TotalGen) * 100
FemalePer = (FemaleGen / TotalGen) * 100
NonPer = (NonGen / TotalGen) * 100

# Create new DataFrame
Gender_Demo = pd.DataFrame({"": ['Male', 'Female', 'Other/Non-Disclosed'],
                            "Percentage of Players": [MalePer, FemalePer, NonPer],
                            "Total Count": [MaleGen, FemaleGen, NonGen]})

# DataFrame formatting
Gender_Demo["Percentage of Players"] = Gender_Demo["Percentage of Players"].map("{:.2f}%".format)
Gender_Demo = Gender_Demo.set_index('')
Gender_Demo
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Group by Gender
grouped_df = rawdata_df.groupby(["Gender"])

# Data Manipulation
purchCount = grouped_df["SN"].count()
purchPrice = grouped_df["Price"].mean()
purchValue = grouped_df["Price"].sum()

# Normalize data by deleting all duplicates adn resort
duplicate = rawdata_df.drop_duplicates(subset='SN', keep="first")
grouped_dup = duplicate.groupby(["Gender"])

# Find normalized data
purchNorm = (grouped_df["Price"].sum() / grouped_dup["SN"].count())

# Create new DataFrame
Purch_Anal_Gen = pd.DataFrame({"Purchase Count": purchCount,
                              "Average Purchase Price": purchPrice,
                              "Total Purchase Value": purchValue,
                              "Normalized Totals": purchNorm})

# DataFrame formatting
Purch_Anal_Gen["Average Purchase Price"] = Purch_Anal_Gen["Average Purchase Price"].map("${:.2f}".format)
Purch_Anal_Gen["Total Purchase Value"] = Purch_Anal_Gen["Total Purchase Value"].map("${:.2f}".format)
Purch_Anal_Gen["Normalized Totals"] = Purch_Anal_Gen["Normalized Totals"].map("${:.2f}".format)
Purch_Anal_Gen = Purch_Anal_Gen[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
Purch_Anal_Gen
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Binning
bins = [0,10,15,20,25,30,35,40,200]
binLab = ['Under 10', '10 - 14', '15 - 19', '20 - 24', '25 - 29', '30 - 34', '35 - 39', 'Over 40']

# Add bins to new dataframe and groupby
binner_df = rawdata_df.copy()
binner_df["Age Groups"] = pd.cut(binner_df["Age"], bins, labels=binLab)
group_bin = binner_df.groupby(["Age Groups"])

# Data manipulation
binnerCount = group_bin["SN"].count()
countTotal = rawdata_df["SN"].count()
percentage = (binnerCount / countTotal) * 100
percentage

# Create new DataFrame
Age_Perc = pd.DataFrame({"Total Count": binnerCount,
                         "Percentage of Players": percentage})

# DataFrame formatting
Age_Perc["Percentage of Players"] = Age_Perc["Percentage of Players"].map("{:.2f}%".format)
Age_Perc.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Groups</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Under 10</th>
      <td>4.10%</td>
      <td>32</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>10.00%</td>
      <td>78</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>23.59%</td>
      <td>184</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>39.10%</td>
      <td>305</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>9.74%</td>
      <td>76</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>7.44%</td>
      <td>58</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>5.64%</td>
      <td>44</td>
    </tr>
    <tr>
      <th>Over 40</th>
      <td>0.38%</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Binning
bins = [0,10,15,20,25,30,35,40,200]
binLab = ['Under 10', '10 - 14', '15 - 19', '20 - 24', '25 - 29', '30 - 34', '35 - 39', 'Over 40']

# Add bins to new dataframe and groupby
binning_df = rawdata_df.copy()
binning_df["Age Groups"] = pd.cut(binning_df["Age"], bins, labels=binLab)
binColumn = pd.cut(binning_df["Age"], bins, labels=binLab)
grouped_bin = binning_df.groupby(["Age Groups"])

# Data Manipulation
binPCount = grouped_bin["Age"].count()
binPAver = grouped_bin["Price"].mean()
binPTotal = grouped_bin["Price"].sum()

# Normalize data by deleting duplicates for new counts
binningduplicate = rawdata_df.drop_duplicates(subset='SN', keep="first")
binningduplicate["Age Groups"] = pd.cut(binningduplicate["Age"], bins, labels=binLab)
binningduplicate = binningduplicate.groupby(["Age Groups"])

binningNorm = (grouped_bin["Price"].sum() / binningduplicate["SN"].count())
binningNorm

# Create new DF and format
Age_Demo = pd.DataFrame({"Purchase Count": binPCount,
                         "Average Purchase Price": binPAver,
                         "Total Purchase Value": binPTotal,
                         "Normalized Totals": binningNorm})

Age_Demo["Average Purchase Price"] = Age_Demo["Average Purchase Price"].map("${:.2f}".format)
Age_Demo["Total Purchase Value"] = Age_Demo["Total Purchase Value"].map("${:.2f}".format)
Age_Demo["Normalized Totals"] = Age_Demo["Normalized Totals"].map("${:.2f}".format)
Age_Demo = Age_Demo[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
Age_Demo.head(10)
```

    C:\Users\domor\Anaconda3\lib\site-packages\ipykernel_launcher.py:18: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Under 10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>78</td>
      <td>$2.87</td>
      <td>$224.15</td>
      <td>$4.15</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>184</td>
      <td>$2.87</td>
      <td>$528.74</td>
      <td>$3.80</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>305</td>
      <td>$2.96</td>
      <td>$902.61</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>76</td>
      <td>$2.89</td>
      <td>$219.82</td>
      <td>$4.23</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>58</td>
      <td>$3.07</td>
      <td>$178.26</td>
      <td>$4.05</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>44</td>
      <td>$2.90</td>
      <td>$127.49</td>
      <td>$5.10</td>
    </tr>
    <tr>
      <th>Over 40</th>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
      <td>$2.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Data Manipulation
groupedBySN = rawdata_df.groupby(["SN"])
groupedSNCount = groupedBySN["Item ID"].count()
groupedSNTotal = groupedBySN["Price"].sum()
groupedSNAvg = (groupedSNTotal / groupedSNCount)

# Build DF and format
SN_Demo = pd.DataFrame({"Purchase Count": groupedSNCount,
                         "Average Purchase Price": groupedSNAvg,
                         "Total Purchase Value": groupedSNTotal})

SN_Demo = SN_Demo.sort_values("Total Purchase Value", ascending=False) 
SN_Demo["Average Purchase Price"] = SN_Demo["Average Purchase Price"].map("${:.2f}".format)
SN_Demo["Total Purchase Value"] = SN_Demo["Total Purchase Value"].map("${:.2f}".format)
SN_Demo = SN_Demo[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
SN_Demo.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
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
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Data manipulation
groupItem = rawdata_df.groupby(["Item ID", "Item Name"])
groupItemC = groupItem["SN"].count()
groupPriceSum = groupItem["Price"].sum()
groupItemP = (groupPriceSum / groupItemC)
groupItemV = (groupItemP * groupItemC)

# New DF with formatting
Pop_Item = pd.DataFrame({"Purchase Count": groupItemC,
                          "Item Price": groupItemP,
                          "Total Purchase Value": groupItemV})

Pop_Item = Pop_Item.sort_values("Purchase Count", ascending=False) 
Pop_Item["Item Price"] = Pop_Item["Item Price"].map("${:.2f}".format)
Pop_Item["Total Purchase Value"] = Pop_Item["Total Purchase Value"].map("${:.2f}".format)
Pop_Item = Pop_Item[["Purchase Count", "Item Price", "Total Purchase Value"]]
Pop_Item.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Data manipulation
groupedItem = rawdata_df.groupby(["Item ID", "Item Name"])
groupedItemC = groupedItem["Gender"].count()
groupedSum = groupedItem["Price"].sum()
groupedItemP = (groupedSum / groupedItemC)

# Make a new DF and format
Pop_Val = pd.DataFrame({"Purchase Count": groupedItemC,
                          "Item Price": groupedItemP,
                          "Total Purchase Value": groupedSum})

Pop_Val = Pop_Val.sort_values("Total Purchase Value", ascending=False) 
Pop_Val["Item Price"] = Pop_Val["Item Price"].map("${:.2f}".format)
Pop_Val["Total Purchase Value"] = Pop_Val["Total Purchase Value"].map("${:.2f}".format)
Pop_Val = Pop_Val[["Purchase Count", "Item Price", "Total Purchase Value"]]
Pop_Val.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


