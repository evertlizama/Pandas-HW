
# Heroes Of Pymoli Data Analysis Summary

* Overall, the male population had the highest purchase value based on the data analysis performed with a total purchase value of $1,867.68

* Per the grouped data by SN, the gamer that had the greatest number of purchases was Undirrala66, with 5 purchases. 

* The most popular item that was purchased out of all the items was Betrayal, Whisper of Grieving Widows with a purchase count of 11. 

* Although the most popular item was Betrayal, Whisper of Greiving Widows, the item that provided the greatest profit was Retribution Axe with a total purchase price of $37.26.

* The age demographic that contributed to the most purchases fell between ages 20-24 (45%). 

-----



```python
# Add something here
#import dependencies
import pandas as pd
pd.options.display.float_format = '${:,.2f}'.format
import numpy as np
```


```python
#make imported file readable
purchase_data = "purchase_data.json"
purchase_data_df = pd.read_json(purchase_data)
purchase_data_df.head()
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
      <td>$3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>$2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>$2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>$1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>$1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



Player Count


```python
#count players
playerCount = purchase_data_df['SN'].value_counts()
playerCount_df = pd.DataFrame({"Total Players": [playerCount.count()]})
playerCount_df
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
removed_duplicates = purchase_data_df.drop_duplicates(['SN'], keep="first")
#removed_duplicates.head()
```

Purchasing Analysis (Total)


```python
#purchasing analysis

uniqueItems = purchase_data_df['Item ID'].value_counts().count()

avgPurchasePrice = round(purchase_data_df['Price'].mean(),2)

totalPurchases = purchase_data_df['Item ID'].count()

totalRev = round(purchase_data_df['Price'].sum(),2)

purchasingAnalysis_df = pd.DataFrame({"Number of unique items" : [uniqueItems], 
                                      "Average purchase price" : [avgPurchasePrice],
                                     "Total # of purchases: " : [totalPurchases],
                                     "Total Revenue" : [totalRev]})
purchasingAnalysis_df
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
      <th>Average purchase price</th>
      <th>Number of unique items</th>
      <th>Total # of purchases:</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>183</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



Gender Demographics


```python
#gender demographics
df_gender_percentbreakdown = purchase_data_df["Gender"].value_counts(normalize=True)

df_gender_breakdown = removed_duplicates['Gender'].value_counts()

gender_summary_table = pd.DataFrame({
                           "Percentage of Players": (df_gender_percentbreakdown)*100,
                           "Total Count": df_gender_breakdown})
gender_summary_table["Percentage of Players"]=gender_summary_table["Percentage of Players"].map('{:,.2f}'.format)
gender_summary_table
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
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.44</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.41</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Gender)


```python
#gender purchase analysis
genderGroupBy_df = purchase_data_df.groupby(["Gender"])
avgPurchasePrice = round(genderGroupBy_df['Price'].mean(),2)
totalPurchasePrice = genderGroupBy_df['Price'].sum()
normalizedTotal = totalPurchasePrice/df_gender_breakdown
normalizedTotal

gender_purchase_analysis = pd.DataFrame({
                                       "Purchase Count" : genderGroupBy_df['Gender'].count(),
                                       "Average Purchase Price" : avgPurchasePrice,
                                       "Total Purchase Value" : totalPurchasePrice, 
                                       "Normalized Totals" : normalizedTotal
                                        })
gender_purchase_analysis_df = gender_purchase_analysis[["Purchase Count","Average Purchase Price","Total Purchase Value", "Normalized Totals"]]
gender_purchase_analysis_df
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
      <td>$1,867.68</td>
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



Age Demographics


```python
#age demographics
ageDemographics_df = pd.DataFrame(removed_duplicates)

ageBin = [5,9,14,19,24,29,34,39,45]
ageGroupNames = ['<10', '10-14', '15-19', '20-24', 
                 '25-29','30-34','35-39','40+']

ageDemographics = pd.cut(ageDemographics_df["Age"], 
                         ageBin, labels=ageGroupNames).value_counts()

playerPercentage = (ageDemographics/removed_duplicates['Item ID'].count())*100

ageDemo_analysis = pd.DataFrame({"Percentage of Players": playerPercentage, "Total Count": ageDemographics})
final_ageDemo_analysis_df = ageDemo_analysis.reindex(['<10', '10-14', '15-19', '20-24','25-29','30-34','35-39','40+'])
final_ageDemo_analysis_df["Percentage of Players"]=final_ageDemo_analysis_df["Percentage of Players"].map('{:,.2f}'.format)
final_ageDemo_analysis_df
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
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Age)


```python
agePurchasing_df = pd.DataFrame(purchase_data_df)

ageBin = [0,9,14,19,24,29,34,39,45]
ageGroupNames = ['<10','10-14', '15-19', '20-24', 
                 '25-29','30-34','35-39','40+']
purchaseBinned = pd.cut(purchase_data_df["Age"], ageBin, labels=ageGroupNames,right=True)
purchaseCount = purchaseBinned.value_counts()
grouped = purchase_data_df.groupby(purchaseBinned)
meanPurchasePrice = grouped['Price'].mean()
totalPurchaseP = purchaseCount*meanPurchasePrice
normalizedTotal2 = totalPurchaseP/ageDemographics

agePurchasing_analysis = pd.DataFrame({"Purchase Count": purchaseCount, "Average Purchase Price": meanPurchasePrice,
                                       "Total Purchase Value": totalPurchaseP,"Normalized Total": normalizedTotal2})
agePurchasing_analysis_df = agePurchasing_analysis.reindex(['10-14', '15-19', '20-24','25-29','30-34','35-39','40+','<10'])
FinalAgePurchasing_analysis_df = agePurchasing_analysis_df[["Purchase Count","Average Purchase Price","Total Purchase Value", "Normalized Total"]]
FinalAgePurchasing_analysis_df
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
      <th>Normalized Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$4.22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$4.42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$4.89</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$4.39</td>
    </tr>
  </tbody>
</table>
</div>



Top Spenders


```python
#Top Spenders
SNGroupBy_df = purchase_data_df.groupby(["SN"])
purchaseCount = SNGroupBy_df['Item ID'].count()
avgPurchasePrice = round(SNGroupBy_df['Price'].mean(),2)
TotalPurchaseValue = purchaseCount*avgPurchasePrice

TopSpender_analysis = pd.DataFrame({
                                       "Purchase Count" : purchaseCount,
                                       "Average Purchase Price" : avgPurchasePrice,
                                       "Total Purchase Value" : TotalPurchaseValue 
                                        }).sort_values('Total Purchase Value',ascending=False).head()
FinalTopSpender_analysis = TopSpender_analysis[["Purchase Count","Average Purchase Price","Total Purchase Value"]]
FinalTopSpender_analysis
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
      <td>$17.05</td>
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
      <td>$12.72</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.72</td>
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



Most Popular Items 


```python
##Most Popular Items

popularItems_GroupBy_df = purchase_data_df.groupby(["Item ID", "Item Name"])

itemPrice = popularItems_GroupBy_df["Price"].sum()/popularItems_GroupBy_df["Item ID"].count()

popularity_analysis = pd.DataFrame({
                                       "Item Count" : popularItems_GroupBy_df["Item ID"].count(),
                                        "Item Price": itemPrice,
                                       "Total Purchase Price" : popularItems_GroupBy_df["Price"].sum()
                                        }).sort_values('Item Count',ascending=False).head()
popularity_analysis
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
      <th>Item Count</th>
      <th>Item Price</th>
      <th>Total Purchase Price</th>
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



Most Profitable Items


```python
## Most Profitable Items

profitability_GroupBy_df = purchase_data_df.groupby(["Item ID", "Item Name"])

profitable_analysis = pd.DataFrame({
                                       "Item Count" : profitability_GroupBy_df["Item ID"].count(),
                                        "Item Price": profitability_GroupBy_df["Price"].mean(),
                                       "Total Purchase Price" : profitability_GroupBy_df["Price"].sum()
                                        }).sort_values('Total Purchase Price',ascending=False).head()
profitable_analysis
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
      <th>Item Count</th>
      <th>Item Price</th>
      <th>Total Purchase Price</th>
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


