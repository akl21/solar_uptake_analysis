```python
#import modules
import pandas as pd
import numpy as np
```


```python
#provide encoding as latin1 to remediate bytes encoding error
#use the fips column as the index. fips is the census tract number
solar_df = pd.read_csv('deepsolar_tract.csv', engine = 'python', encoding = 'latin1', index_col = 'fips')
```


```python
#look at the beginning of the data frame
solar_df.head()
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
      <th>tile_count</th>
      <th>solar_system_count</th>
      <th>total_panel_area</th>
      <th>average_household_income</th>
      <th>county</th>
      <th>education_bachelor</th>
      <th>education_college</th>
      <th>education_doctoral</th>
      <th>education_high_school_graduate</th>
      <th>education_less_than_high_school</th>
      <th>...</th>
      <th>incentive_count_nonresidential</th>
      <th>incentive_residential_state_level</th>
      <th>incentive_nonresidential_state_level</th>
      <th>net_metering</th>
      <th>feedin_tariff</th>
      <th>cooperate_tax</th>
      <th>property_tax</th>
      <th>sales_tax</th>
      <th>rebate</th>
      <th>avg_electricity_retail_rate</th>
    </tr>
    <tr>
      <th>fips</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>27145011200</th>
      <td>0</td>
      <td>0</td>
      <td>0.000000</td>
      <td>70352.78987</td>
      <td>Stearns County</td>
      <td>569</td>
      <td>1690</td>
      <td>13</td>
      <td>1757</td>
      <td>336</td>
      <td>...</td>
      <td>39</td>
      <td>11</td>
      <td>13</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>12</td>
      <td>0</td>
      <td>9.46</td>
    </tr>
    <tr>
      <th>27145011301</th>
      <td>25</td>
      <td>21</td>
      <td>1133.436461</td>
      <td>61727.08520</td>
      <td>Stearns County</td>
      <td>674</td>
      <td>1434</td>
      <td>108</td>
      <td>767</td>
      <td>222</td>
      <td>...</td>
      <td>39</td>
      <td>11</td>
      <td>13</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>12</td>
      <td>0</td>
      <td>9.46</td>
    </tr>
    <tr>
      <th>27145011302</th>
      <td>3</td>
      <td>3</td>
      <td>64.505776</td>
      <td>71496.88658</td>
      <td>Stearns County</td>
      <td>854</td>
      <td>1459</td>
      <td>31</td>
      <td>1541</td>
      <td>289</td>
      <td>...</td>
      <td>39</td>
      <td>11</td>
      <td>13</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>12</td>
      <td>0</td>
      <td>9.46</td>
    </tr>
    <tr>
      <th>27145011304</th>
      <td>0</td>
      <td>0</td>
      <td>0.000000</td>
      <td>86840.15275</td>
      <td>Stearns County</td>
      <td>640</td>
      <td>1116</td>
      <td>68</td>
      <td>1095</td>
      <td>231</td>
      <td>...</td>
      <td>39</td>
      <td>11</td>
      <td>13</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>12</td>
      <td>0</td>
      <td>9.46</td>
    </tr>
    <tr>
      <th>27145011400</th>
      <td>5</td>
      <td>5</td>
      <td>164.583303</td>
      <td>89135.31560</td>
      <td>Stearns County</td>
      <td>654</td>
      <td>1314</td>
      <td>15</td>
      <td>982</td>
      <td>163</td>
      <td>...</td>
      <td>39</td>
      <td>11</td>
      <td>13</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>12</td>
      <td>0</td>
      <td>9.46</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 167 columns</p>
</div>




```python
#create a histogram of the column total_panel_area
solar_df.hist(column = 'total_panel_area', color = 'orange', bins = 50)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Input In [1], in <cell line: 2>()
          1 #create a histogram of the column total_panel_area
    ----> 2 solar_df.hist(column = 'total_panel_area', color = 'orange', bins = 50)
    

    NameError: name 'solar_df' is not defined



```python
!
```


```python
#creat a hisogram of the total_panel_area for all rows where total_panel_area < 90th percentile of total_panel_area
p90 = solar_df['total_panel_area'].quantile(q = 0.9)
solar_df[solar_df['total_panel_area'] < p90].hist(column = 'total_panel_area', color = 'orange', bins = 50)
```


```python
#create a histogram of the total_panel_area that is greater than the 10th percentile and less than the 90th percentile
p10 = solar_df['total_panel_area'].quantile(q = 0.1)
solar_df[((solar_df['total_panel_area'] < p90) & (solar_df['total_panel_area'] > p10))].hist(column = 'total_panel_area',\
                                                                                              color = 'orange', bins = 50)
```

![Image 3](https://github.com/akl21/solar_uptake_analysis/blob/main/solar_adoption_analysis_5_1.png)
