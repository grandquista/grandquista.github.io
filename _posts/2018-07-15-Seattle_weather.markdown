

```python
import datetime as dt
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sbs
from matplotlib.colors import ListedColormap
from sklearn import neighbors, datasets
from sklearn.metrics import confusion_matrix, classification_report, mean_squared_error
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

```


```python
# https://www.kaggle.com/rtatman/did-it-rain-in-seattle-19482017/downloads/did-it-rain-in-seattle-19482017.zip/1
df = pd.read_csv('./seattleWeather_1948-2017.csv')
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 25551 entries, 0 to 25550
    Data columns (total 5 columns):
    DATE    25551 non-null object
    PRCP    25548 non-null float64
    TMAX    25551 non-null int64
    TMIN    25551 non-null int64
    RAIN    25548 non-null object
    dtypes: float64(1), int64(2), object(2)
    memory usage: 998.2+ KB



```python
data = pd.DataFrame()
data['temp_max'] = df.TMAX
data['temp_min'] = df.TMIN
data['date'] = pd.to_datetime(df.DATE)
data['precip'] = pd.to_numeric(df.PRCP)
data['above_1'] = data.precip > 1
data['above_avg'] = data.precip > 0.25
data['any_rain'] = data.precip > 0
data['prev_temp_max'] = [data.temp_max[0]] + list(data.temp_max[:-1])
data['prev_temp_min'] = [data.temp_min[0]] + list(data.temp_min[:-1])
```


```python
data.plot(x='date', y='precip')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x112597630>




![png](output_3_1.png)



```python
data.plot(x='date', y='temp_min')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x112d9ceb8>




![png](output_4_1.png)



```python
data.describe()
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
      <th>temp_max</th>
      <th>temp_min</th>
      <th>precip</th>
      <th>prev_temp_max</th>
      <th>prev_temp_min</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>25551.000000</td>
      <td>25551.000000</td>
      <td>25548.000000</td>
      <td>25551.000000</td>
      <td>25551.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>59.544206</td>
      <td>44.514226</td>
      <td>0.106222</td>
      <td>59.544245</td>
      <td>44.514461</td>
    </tr>
    <tr>
      <th>std</th>
      <td>12.772984</td>
      <td>8.892836</td>
      <td>0.239031</td>
      <td>12.772956</td>
      <td>8.892690</td>
    </tr>
    <tr>
      <th>min</th>
      <td>4.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>4.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>50.000000</td>
      <td>38.000000</td>
      <td>0.000000</td>
      <td>50.000000</td>
      <td>38.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>58.000000</td>
      <td>45.000000</td>
      <td>0.000000</td>
      <td>58.000000</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>69.000000</td>
      <td>52.000000</td>
      <td>0.100000</td>
      <td>69.000000</td>
      <td>52.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>103.000000</td>
      <td>71.000000</td>
      <td>5.020000</td>
      <td>103.000000</td>
      <td>71.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 25551 entries, 0 to 25550
    Data columns (total 9 columns):
    temp_max         25551 non-null int64
    temp_min         25551 non-null int64
    date             25551 non-null datetime64[ns]
    precip           25548 non-null float64
    above_1          25551 non-null bool
    above_avg        25551 non-null bool
    any_rain         25551 non-null bool
    prev_temp_max    25551 non-null int64
    prev_temp_min    25551 non-null int64
    dtypes: bool(3), datetime64[ns](1), float64(1), int64(4)
    memory usage: 1.2 MB



```python
sbs.pairplot(data.drop(columns=['precip']))
```




    <seaborn.axisgrid.PairGrid at 0x1135e2eb8>




![png](output_7_1.png)



```python
X = data.drop(columns=['above_1', 'above_avg', 'any_rain'])
y_above_1 = data['above_1']
y_above_avg = data['above_avg']
y_any_rain = data['any_rain']

X_train, X_test, y_above_1_train, y_above_1_test = train_test_split(X, y_above_1, test_size=0.3, random_state=123)
X_train, X_test, y_above_avg_train, y_above_avg_test = train_test_split(X, y_above_avg, test_size=0.3, random_state=123)
X_train, X_test, y_any_rain_train, y_any_rain_test = train_test_split(X, y_any_rain, test_size=0.3, random_state=123)
```
