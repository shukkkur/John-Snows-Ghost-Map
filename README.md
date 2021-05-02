<h2> Recreating John Snow's Ghost Map </h2>
<p align='center'>
  <img width=300 height=400 src='https://upload.wikimedia.org/wikipedia/commons/c/cc/John_Snow.jpg'>
</p>
<p>In 1854, Dr. John Snow used spatial analysis by mapping patterns and occurrences of cholera outbreaks in Soho, London. He mapped the deaths in the neighbourhood and determined that a vast majority occurred around one particular water well and that those that died used that well.</p>
<p>In this Python project, you will reanalyze the data and recreate his famous map.</p>


<h3>1. Dr. John Snow </h3>

<p>Dr. John Snow (1813-1858) was a famous British physician and is widely recognized as a legendary figure in the history of public health and a leading pioneer in the development of anesthesia. Some even say one of the greatest physicians of all time.</p>

```python
deaths = pd.read_csv('datasets/deaths.csv')

newcols = {
    'Death': 'death_count',
    'X coordinate': 'x_latitude', 
    'Y coordinate': 'y_longitude' 
    }
    
deaths.rename(newcols, inplace=True,axis='columns')
print(deaths.shape)

>>> (489, 3)

print(deaths.head())
```

|  | death_count | x_latitude |y_longitude|
|------------:|-----------:|------------:|----------:|
|      0      | 1          | 51.513418   | -0.137930 |
|      1      | 1          | 51.513418   | -0.137930 |
|      2      | 1          | 51.513418   | -0.137930 |
|      3      | 1          | 51.513361   | -0.137883 |
|      4      | 1          | 51.513361   | -0.137883 |

<h3>2. Cholera attacks!</h3>
<p>Originally he formulated and published his theory that cholera is spread by water or food in an essay, which received negative reviews, but we know he was right.
  
```python
print(deaths.describe())
```
|  |death_count  |x_latitude  |       y_longitude     |
|------------:|-----------:|------------:|-----------:|
|    count    | 489.0      | 489.000000  | 489.000000 |
|     mean    | 1.0        | 51.513398   | -0.136403  |
|     std     | 0.0        | 0.000705    | 0.001503   |
|     min     | 1.0        | 51.511856   | -0.140074  |
|     25%     | 1.0        | 51.512964   | -0.137562  |
|     50%     | 1.0        | 51.513359   | -0.136226  |
|     75%     | 1.0        | 51.513875   | -0.135344  |
|     max     | 1.0        | 51.515834   | -0.132933  |


<h3>3. You know nothing, John Snow!</h3>
<p>His medical colleagues simply said: "You know nothing, John Snow!" However, a reviewer made a helpful suggestion in terms of what evidence would be compelling: the crucial natural experiment would be to find people living side by side with lifestyles similar in all respects except for the water source.</p>

<h4>Snow's Map</h4>
<p align='center'>
  <img src='http://atlas-dev.s3.amazonaws.com/uploads/assets/Snow-cholera-map-1(1).jpg'>
</p>

<h4>We now know how John Snow did it and have the data too, so let's recreate his map using modern techniques.</h4>

```python
import folium

locations = deaths[['x_latitude', 'y_longitude']]
deaths_list = locations.values.tolist()

map = folium.Map(location=[51.5132119,-0.13666], tiles='Stamen Toner', zoom_start=17)
for point in range(0, len(deaths_list)):
    folium.CircleMarker(deaths_list[point], radius=8, color='red', fill=True, fill_color='red', opacity = 0.4).add_to(map)
map
```
<p align='center'><img src='https://github.com/shukkkur/John-Snows-Ghost-Map/blob/a12126fa75b5f181c2daf01b0d3e4d780f2bd506/datasets/choleraAttacks.gif'><p>
