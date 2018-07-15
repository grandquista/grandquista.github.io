

```python
from csv import reader
import pandas as pd
import datetime as dt
import matplotlib.pyplot as plt
```


```python
# https://www.kaggle.com/rtatman/did-it-rain-in-seattle-19482017/downloads/did-it-rain-in-seattle-19482017.zip/1
data = pd.read_csv('./seattleWeather_1948-2017.csv')
```


```python
data['temp_max'] = data.TMAX
data['temp_min'] = data.TMIN
data['date'] = pd.to_datetime(data.DATE)
data['precip'] = data.PRCP
data['above_1'] = data.precip > 1
data['above_avg'] = data.precip > 0.25
data['any_rain'] = data.precip > 0
data['prev_temp_max'] = [data.TMAX[0]] + list(data.TMAX[:-1])
data['prev_temp_min'] = [data.TMIN[0]] + list(data.TMIN[:-1])
```


```python
data.plot(x='date', y='precip')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x10bc1fa90>




![png](output_3_1.png)



```python
data.plot(x='date', y='temp_min')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x10bc097f0>




![png](output_4_1.png)

