# PyBer_Analysis
Python with Pandas and Matplotlib 

## Overview:
In this challenge we were given a new assignment to help V. Isualize create a summary DataFrame of the ride-sharing data by city type. We are then asked to create a multiple-line graph that will show the total weekly fares for each city type. With the help of Matplotlib and its visualization tools, we can better present to V and Omar the analysis of our dataset. 

## Purpose:
The purpose of this challenge is to help us practice using Matplotlib and the different forms of data visualization. During the module we were tasked to create different charts (bar, scatter, bubble, line, box/whisker, pie) to visualize PyBer data. Choosing the right visulization is a big part of Data Analytics. Every chart can present something different, and it is up to Data Analysts to choose the right one to present our analysis. We are also using past techniques learned such as merging datasets, importing csvs, and using the Pandas library to create dataframes. 

## Resources
* Data Source: city_data.csv, ride_data.csv
* Software Python 3.8.8, Conda 4.10.1, Jupyter Notebook 6.3.0

*Code:* 
# Pyber Challenge

### 4.3 Loading and Reading CSV files


```python
# Add Matplotlib inline magic command
%matplotlib inline
```


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import scipy.stats as sts
import numpy as np
from matplotlib.ticker import MultipleLocator
# Import mpl to change the plot configurations using rcParams.
import matplotlib as mpl
import os
```


```python
# gets present working directory just in case files are moved around
pwd = os.getcwd()

# File to Load (Remember to change these)
city_data_to_load = os.path.join(pwd, "Resources", "city_data.csv" )
ride_data_to_load = os.path.join(pwd, "Resources", "ride_data.csv" )

# Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)
```


```python
# check if df was read correctly using head()
ride_data_df.head(10)
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lake Jonathanshire</td>
      <td>2019-01-14 10:14:22</td>
      <td>13.83</td>
      <td>5739410935873</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Michelleport</td>
      <td>2019-03-04 18:24:09</td>
      <td>30.24</td>
      <td>2343912425577</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Samanthamouth</td>
      <td>2019-02-24 04:29:00</td>
      <td>33.44</td>
      <td>2005065760003</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>2019-02-10 23:22:03</td>
      <td>23.44</td>
      <td>5149245426178</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South Jack</td>
      <td>2019-03-06 04:28:35</td>
      <td>34.58</td>
      <td>3908451377344</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South Latoya</td>
      <td>2019-03-11 12:26:48</td>
      <td>9.52</td>
      <td>1994999424437</td>
    </tr>
    <tr>
      <th>6</th>
      <td>New Paulville</td>
      <td>2019-02-27 11:17:56</td>
      <td>43.25</td>
      <td>793208410091</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Simpsonburgh</td>
      <td>2019-04-26 00:43:24</td>
      <td>35.98</td>
      <td>111953927754</td>
    </tr>
    <tr>
      <th>8</th>
      <td>South Karenland</td>
      <td>2019-01-08 03:28:48</td>
      <td>35.09</td>
      <td>7995623208694</td>
    </tr>
    <tr>
      <th>9</th>
      <td>North Jasmine</td>
      <td>2019-03-09 06:26:29</td>
      <td>42.81</td>
      <td>5327642267789</td>
    </tr>
  </tbody>
</table>
</div>



```python
# Check for null values for city data
city_data_df.count()
```

    city            120
    driver_count    120
    type            120
    dtype: int64


```python
# Check for null values for ride data
ride_data_df.count()
```

### Merge the DataFrames

```python
# Combine the data into a single dataset
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])

# Display the data table for preview
pyber_data_df.head(15)
```

```python
# Just in case we want to do individual analysis of each city type, create separate city type df
# to get avg fare, we can use mean(). to get total number of riders per city we can use count on ride_id
# create the urban/suburban/rural city df
urban_cities_df = pyber_data_df[pyber_data_df["type"] == "Urban"]
suburban_cities_df = pyber_data_df[pyber_data_df["type"] == "Suburban"]
rural_cities_df = pyber_data_df[pyber_data_df["type"] == "Rural"]

urban_cities_df.count()
```

## Deliverable 1: Get a Summary DataFrame 


```python
#  1. Get the total rides for each city type
# we can only plot a series so we must separate into series. use groupby(['type']).count['ride_id']
city_type_ride_count = pyber_data_df.groupby(['type']).count()['ride_id']
city_type_ride_count
```


    type
    Rural        125
    Suburban     625
    Urban       1625
    Name: ride_id, dtype: int64


```python
# 2. Get the total drivers for each city type
# use groupby(['type']).sum()['driver_count']
city_type_driver_count = city_data_df.groupby(['type']).sum()['driver_count']
city_type_driver_count
```


    type
    Rural         78
    Suburban     490
    Urban       2405
    Name: driver_count, dtype: int64


```python
#  3. Get the total amount of fares for each city type
city_type_fare_total = pyber_data_df.groupby(['type']).sum()['fare']
city_type_fare_total
```


    type
    Rural        4327.93
    Suburban    19356.33
    Urban       39854.38
    Name: fare, dtype: float64


```python
#  4. Get the average fare per ride for each city type. 
# Divide the sum of all fares by the total ride
city_type_avg_fare = city_type_fare_total /  city_type_ride_count
city_type_avg_fare
```


    type
    Rural       34.623440
    Suburban    30.970128
    Urban       24.525772
    dtype: float64


```python
# 5. Get the average fare per driver for each city type. 
# divide the sum of all fares by the total drivers
avg_fare_driver = city_type_fare_total / city_type_driver_count
avg_fare_driver
```


    type
    Rural       55.486282
    Suburban    39.502714
    Urban       16.571468
    dtype: float64



```python
#  6. Create a PyBer summary DataFrame. 
pyber_summary_df = pd.DataFrame({
          "Total Rides": city_type_ride_count, 
          "Total Drivers": city_type_driver_count, 
          "Total Fares": city_type_fare_total,
          "Average Fare per Ride": city_type_avg_fare, 
          "Average Fare per Driver": avg_fare_driver,})
pyber_summary_df
```



```python
#  7. Cleaning up the DataFrame. Delete the index name
pyber_summary_df.index.name = None
pyber_summary_df
```


```python
#  8. Format the columns. use .map(.format)
# Add dollar sign to fare(s) columns and commas to thousands place
pyber_summary_df["Total Rides"] = pyber_summary_df["Total Rides"].map("{:,}".format)
pyber_summary_df["Total Drivers"] = pyber_summary_df["Total Drivers"].map("{:,}".format)
pyber_summary_df["Total Fares"] = pyber_summary_df["Total Fares"].map("${:,.2f}".format)
pyber_summary_df["Average Fare per Ride"] = pyber_summary_df["Average Fare per Ride"].map("${:,.2f}".format)
pyber_summary_df["Average Fare per Driver"] = pyber_summary_df["Average Fare per Driver"].map("${:,.2f}".format)

#print out to check formatting
pyber_summary_df
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
      <th>Total Rides</th>
      <th>Total Drivers</th>
      <th>Total Fares</th>
      <th>Average Fare per Ride</th>
      <th>Average Fare per Driver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
      <td>78</td>
      <td>$4,327.93</td>
      <td>$34.62</td>
      <td>$55.49</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>625</td>
      <td>490</td>
      <td>$19,356.33</td>
      <td>$30.97</td>
      <td>$39.50</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1,625</td>
      <td>2,405</td>
      <td>$39,854.38</td>
      <td>$24.53</td>
      <td>$16.57</td>
    </tr>
  </tbody>
</table>
</div>



## Deliverable 2.  Create a multiple line plot that shows the total weekly of the fares for each type of city.


```python
# 1. Read the merged DataFrame
pyber_data_df
```




```python
# 2. Using groupby() to create a new DataFrame showing the sum of the fares 
#  for each date where the indices are the city type and date.
date_type_df = pyber_data_df.groupby(['type', 'date']).sum()['fare']

date_type_df
```




    type   date               
    Rural  2019-01-01 09:45:36    43.69
           2019-01-02 11:18:32    52.12
           2019-01-03 19:51:01    19.90
           2019-01-04 03:31:26    24.88
           2019-01-06 07:38:40    47.33
                                  ...  
    Urban  2019-05-08 04:20:00    21.99
           2019-05-08 04:39:49    18.45
           2019-05-08 07:29:01    18.55
           2019-05-08 11:38:35    19.77
           2019-05-08 13:10:18    18.04
    Name: fare, Length: 2375, dtype: float64




```python
# 3. Reset the index on the DataFrame you created in #1. This is needed to use the 'pivot()' function.
# df = df.reset_index()
date_type_df = date_type_df.reset_index()
```


```python
date_type_df
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
      <th>type</th>
      <th>date</th>
      <th>fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>2019-01-01 09:45:36</td>
      <td>43.69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Rural</td>
      <td>2019-01-02 11:18:32</td>
      <td>52.12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rural</td>
      <td>2019-01-03 19:51:01</td>
      <td>19.90</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rural</td>
      <td>2019-01-04 03:31:26</td>
      <td>24.88</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rural</td>
      <td>2019-01-06 07:38:40</td>
      <td>47.33</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2370</th>
      <td>Urban</td>
      <td>2019-05-08 04:20:00</td>
      <td>21.99</td>
    </tr>
    <tr>
      <th>2371</th>
      <td>Urban</td>
      <td>2019-05-08 04:39:49</td>
      <td>18.45</td>
    </tr>
    <tr>
      <th>2372</th>
      <td>Urban</td>
      <td>2019-05-08 07:29:01</td>
      <td>18.55</td>
    </tr>
    <tr>
      <th>2373</th>
      <td>Urban</td>
      <td>2019-05-08 11:38:35</td>
      <td>19.77</td>
    </tr>
    <tr>
      <th>2374</th>
      <td>Urban</td>
      <td>2019-05-08 13:10:18</td>
      <td>18.04</td>
    </tr>
  </tbody>
</table>
<p>2375 rows × 3 columns</p>
</div>




```python
# Check data types
date_type_df.dtypes
```




    type     object
    date     object
    fare    float64
    dtype: object




```python
# 4. Create a pivot table with the 'date' as the index, the columns ='type', and values='fare' 
# to get the total fares for each type of city by the date. 
date_type_pivot = date_type_df.pivot(index="date", columns="type", values="fare")
date_type_pivot.head(10)
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
      <th>type</th>
      <th>Rural</th>
      <th>Suburban</th>
      <th>Urban</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 00:08:16</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>37.91</td>
    </tr>
    <tr>
      <th>2019-01-01 00:46:46</th>
      <td>NaN</td>
      <td>47.74</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 02:07:24</th>
      <td>NaN</td>
      <td>24.07</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 03:46:50</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.57</td>
    </tr>
    <tr>
      <th>2019-01-01 05:23:21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.75</td>
    </tr>
    <tr>
      <th>2019-01-01 09:45:36</th>
      <td>43.69</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 12:32:48</th>
      <td>NaN</td>
      <td>25.56</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 14:40:14</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.42</td>
    </tr>
    <tr>
      <th>2019-01-01 14:42:25</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.31</td>
    </tr>
    <tr>
      <th>2019-01-01 14:52:06</th>
      <td>NaN</td>
      <td>31.15</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# check data type df
date_type_pivot.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 2375 entries, 2019-01-01 00:08:16 to 2019-05-08 13:10:18
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Rural     125 non-null    float64
     1   Suburban  625 non-null    float64
     2   Urban     1625 non-null   float64
    dtypes: float64(3)
    memory usage: 74.2+ KB
    


```python
# 5. Create a new DataFrame from the pivot table DataFrame using loc on the given dates, '2019-01-01':'2019-04-29'.
tripcounts_Jan_Apr = date_type_pivot.loc['2019-01-01':'2019-04-29']
tripcounts_Jan_Apr
```

```python
# 6 changing the index 'date' to datetime type using df.index=pd.to_datetime(df.index)
tripcounts_Jan_Apr.index = pd.to_datetime(tripcounts_Jan_Apr.index)
tripcounts_Jan_Apr.info()
```

    <class 'pandas.core.frame.DataFrame'>
    DatetimeIndex: 2196 entries, 2019-01-01 00:08:16 to 2019-04-28 19:35:03
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Rural     114 non-null    float64
     1   Suburban  573 non-null    float64
     2   Urban     1509 non-null   float64
    dtypes: float64(3)
    memory usage: 68.6 KB
    


```python
# 8. Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
# Resample the DataFrame by the week. ie., "W", and get the trip fare for each week. 
weekly_fares_df = tripcounts_Jan_Apr.resample('W').sum()
weekly_fares_df.head(10)
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
      <th>type</th>
      <th>Rural</th>
      <th>Suburban</th>
      <th>Urban</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-06</th>
      <td>187.92</td>
      <td>721.60</td>
      <td>1661.68</td>
    </tr>
    <tr>
      <th>2019-01-13</th>
      <td>67.65</td>
      <td>1105.13</td>
      <td>2050.43</td>
    </tr>
    <tr>
      <th>2019-01-20</th>
      <td>306.00</td>
      <td>1218.20</td>
      <td>1939.02</td>
    </tr>
    <tr>
      <th>2019-01-27</th>
      <td>179.69</td>
      <td>1203.28</td>
      <td>2129.51</td>
    </tr>
    <tr>
      <th>2019-02-03</th>
      <td>333.08</td>
      <td>1042.79</td>
      <td>2086.94</td>
    </tr>
    <tr>
      <th>2019-02-10</th>
      <td>115.80</td>
      <td>974.34</td>
      <td>2162.64</td>
    </tr>
    <tr>
      <th>2019-02-17</th>
      <td>95.82</td>
      <td>1045.50</td>
      <td>2235.07</td>
    </tr>
    <tr>
      <th>2019-02-24</th>
      <td>419.06</td>
      <td>1412.74</td>
      <td>2466.29</td>
    </tr>
    <tr>
      <th>2019-03-03</th>
      <td>175.14</td>
      <td>858.46</td>
      <td>2218.20</td>
    </tr>
    <tr>
      <th>2019-03-10</th>
      <td>303.94</td>
      <td>925.27</td>
      <td>2470.93</td>
    </tr>
  </tbody>
</table>
</div>



```python
# 8. Using the object-oriented interface method, plot the resample DataFrame using the df.plot() function. 
weekly_fares = weekly_fares_df.plot(figsize = (22,5), color = ['skyblue', 'coral', 'gold'])

# adding title and labels
weekly_fares.set_title('Total Fare by City Type')
weekly_fares.set_ylabel('Fare ($USD)')
# removing x-axis label: https://stackoverflow.com/questions/40705614/hide-axis-label-only-not-entire-axis-in-pandas-plot
ax1 = plt.axes()
ax1.xaxis.set_label_text('foo')
ax1.xaxis.label.set_visible(False)

# Import the style from Matplotlib.
from matplotlib import style
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')
plt.savefig("Analysis/PyBer_fare_summary.png", bbox_inches = 'tight')

```

    
![png](output_31_1.png)
    


## Analysis:

### Overview of the analysis: 
The purpose of this analysis is to visualize the difference in PyBer traffic between different city types. Using both pandas and matplotlib, we are supposed to visualize the difference between fare price, rides, and driver count between each city type. The questions we ask ourselves are: what is the average fare price per city type, how many drivers are in each city type, and how many rides are given per city type? By using pandas data frame and matplotlib visualization tools, we can easily analyze and plot the results in a comprehensive way. Although we were not asked specifically about outliers in the challenge, the module allowed us to plot box and whisker plots to visualize potential outliers within our dataset.  


### Results: 
Although the results were not astounding and is to be expected, we can take the results of the analysis to make possible game changing decisions at PyBer. The results were as follows: Urban cities lead in all categories of ride counts, driver counts, and total percentage fare amount, suburban cities are in second, and rural cities are last. See the below charts to see the overall results from Janurary to April:

![Fare Summary Line Plot](https://github.com/lo7kyle/PyBer_Analysis/blob/main/Analysis/PyBer_fare_summary.png)

We can take this analysis further and can easily explain the reason why the results are as shown. The first explanation of why there are more drivers in Urban cities is that there are more riders in urban cities. We see that these two factors go hand in hand. With more riders means more drivers because drivers want to make money and will go to a city with more riders. This then affects the total fare amount per city type because with more riders means a higher total fare. Look at the table below for reference to riders, drivers, and Total Fares.

![Fare Summary DF](https://github.com/lo7kyle/PyBer_Analysis/blob/main/Analysis/Pyber_summary_df.PNG)

Looking at the DataFrame we see that although there are less drivers and less riders for rural areas, the average fare per rider and driver increases. We see an inverse of relation that can be explained with basic economics of supply and demand. In the urban cities there are many drivers that are bidding against each other for riders. With a surplus amount of drivers, the cost of each fare goes down. In the rural cities there are not that many drivers and so if a rider needs a ride to a destination, the app will charge more to get a driver out to the rural city to make it worthwhile. We can also account for outliers in the data. After plotting three different box and whisker plots for the three city types, there was only one outlier for urban cities in terms of ride count. This is probably due to a higher amount of rides, which can cause a higher chance for an outlier, in addition to the possibility of surge charges during a busy night where there might be more riders than drivers available. 

![Urban Outlier](https://github.com/lo7kyle/PyBer_Analysis/blob/main/Analysis/Fig2.png)

### Summary: 
In short, with more riders are more drivers. The number of riders can be due to a factor of things such as population density where urban cities will have a higher population in their city compared to that of a rural one and real-estate. What I mean by real-estate is that in the city, for an example, Manhattan will have less parking spaces or houses without a garage and limited street parking compared to Rye which is another city in New York but will have houses with plenty of parking space and a garage. This can sway someone’s decision to even get a car in the first place; therefore, creating a need for a PyBer driver. We also see that with a higher fare total doesn't mean a higher cost of average fare price. This just means there has to be a supply of drivers to supplement the higher number of riders to keep the fares low. With this analysis there are three recommendations that I would suggest helping out the business of each city type. 1. For urban city since there are more people the company has to consider them first since they are the majority of users. I would add the addition of electric scooters or bikes that people can borrow for their short commute to the subway or bus so that in this way we can widen our market for people who doesn't want to pay for a PyBer ride to work or their activities. 2. Another suggestion that would help out all three city types would be to give promotions. Treat your loyal customer with a free PyBer ride up to $20 after x-x-number of rides and give the people in rural areas an incentive to use a PyBer by giving them a promotion to lower their fare since they have the highest average fare amount. 3. Give an incentive to drivers to be ready at all times to increase efficiency of finding a ride for potential riders. I personally have been in situations where I couldn't find any drivers near me. By incentivizing drivers with a small but effective base wage, you can guarantee riders in suburban/rural areas to use PyBer more often since there are less drivers.   
