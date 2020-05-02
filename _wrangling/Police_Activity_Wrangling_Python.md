---
title: "Police Activity - Wrangling (Python)"
classes: wide
comments: true
excerpt: "Analyzing police stops"
---



This a dataset on traffic stops by police traffice in Rhode Island. Let's first import the data using pandas, and do some validating and error tracking. Do note that error checking is _extremely_ important to be done as soon as possible, at it avoids us from more grim future problems.


```python
# Import the pandas library as pd
import pandas as pd

# Read 'police.csv' into a DataFrame named ri
ri = pd.read_csv('police.csv')

# Examine the head of the DataFrame
print(ri.head())

# Count the number of missing values in each column
# .sum() method count the value of nulls where True = 1 and False = 0
# Thus the number of counts in each columns represent the number of missing values
print(ri.isnull().sum())
```

      state   stop_date stop_time  county_name driver_gender driver_race  \
    0    RI  2005-01-04     12:55          NaN             M       White   
    1    RI  2005-01-23     23:15          NaN             M       White   
    2    RI  2005-02-17     04:15          NaN             M       White   
    3    RI  2005-02-20     17:15          NaN             M       White   
    4    RI  2005-02-24     01:20          NaN             F       White   

                        violation_raw  violation  search_conducted search_type  \
    0  Equipment/Inspection Violation  Equipment             False         NaN   
    1                        Speeding   Speeding             False         NaN   
    2                        Speeding   Speeding             False         NaN   
    3                Call for Service      Other             False         NaN   
    4                        Speeding   Speeding             False         NaN   

        stop_outcome is_arrested stop_duration  drugs_related_stop district  
    0       Citation       False      0-15 Min               False  Zone X4  
    1       Citation       False      0-15 Min               False  Zone K3  
    2       Citation       False      0-15 Min               False  Zone X4  
    3  Arrest Driver        True     16-30 Min               False  Zone X1  
    4       Citation       False      0-15 Min               False  Zone X3  
    state                     0
    stop_date                 0
    stop_time                 0
    county_name           91741
    driver_gender          5205
    driver_race            5202
    violation_raw          5202
    violation              5202
    search_conducted          0
    search_type           88434
    stop_outcome           5202
    is_arrested            5202
    stop_duration          5202
    drugs_related_stop        0
    district                  0
    dtype: int64


Since we already knows our data came from the state of Rhode Island, the 'state' column is obsolete. Also, since the county_name has the same number of missing/null values as to the total number of rows in the dataset(which means all values in that column are missing value), we can also remove that column


```python
# Examine the shape of the DataFrame
print(ri.shape)

# Drop the 'county_name' and 'state' columns
# Axis specify if we are removing the rows or the columns
# inplace=True allow us to modify the dataset directly without the need to 
# create new assignment such as ri = ri.drop(['county_name', 'state'], axis='columns')
ri.drop(['county_name', 'state'], axis='columns', inplace=True)

# Examine the shape of the DataFrame (again)
print(ri.shape)
```

    (91741, 15)
    (91741, 13)


Now, let's examine the missing values and remove them.


```python
# Count the number of missing values in each column
print(ri.isnull().sum())

# Let's say that the 'driver_gender' is crucial in our analysis, and any rows that have missing
# values in this column are considered not useful anymore. We can specify using subset=[] to tell
# Python which column to check for the na. If an na is found in that particular column, then
# the row are dropped
# Drop all rows that are missing 'driver_gender'
ri.dropna(subset=['driver_gender'], inplace=True)

# Count the number of missing values in each column (again)
print(ri.isnull().sum())

# Examine the shape of the DataFrame
print(ri.shape)
```

    stop_date                 0
    stop_time                 0
    driver_gender          5205
    driver_race            5202
    violation_raw          5202
    violation              5202
    search_conducted          0
    search_type           88434
    stop_outcome           5202
    is_arrested            5202
    stop_duration          5202
    drugs_related_stop        0
    district                  0
    dtype: int64
    stop_date                 0
    stop_time                 0
    driver_gender             0
    driver_race               0
    violation_raw             0
    violation                 0
    search_conducted          0
    search_type           83229
    stop_outcome              0
    is_arrested               0
    stop_duration             0
    drugs_related_stop        0
    district                  0
    dtype: int64
    (86536, 13)


From the .shape attribute, we can see that a significant amount of row are dropped. It is important
to weight the importance of the rows before dropping them as it might affect our analysis in the future. Now that we have removed columns and rows that are not useful for our analysis, we can continue our data cleaning by checking the datatype of each columns.


```python
ri.dtypes
```




    stop_date             object
    stop_time             object
    driver_gender         object
    driver_race           object
    violation_raw         object
    violation             object
    search_conducted        bool
    search_type           object
    stop_outcome          object
    is_arrested           object
    stop_duration         object
    drugs_related_stop      bool
    district              object
    dtype: object



Object are Python's strings, though it may include the presence of other Python object such as list. This is considered the default type when pandas read in the csv file that are provided when pandas are not sure what is the data type of that column. Let's change one column, that is is_arrested, since it is an object dtype even though the values only consist of True and False


```python
# Examine the head of the 'is_arrested' column
print(ri.is_arrested.head())

# Change the data type of 'is_arrested' to 'bool'
ri['is_arrested'] = ri.is_arrested.astype('bool')

# Check the data type of 'is_arrested'
print(ri.is_arrested.dtype)
```

    0    False
    1    False
    2    False
    3     True
    4    False
    Name: is_arrested, dtype: object
    bool


Another step that we can take is combine the data and time and convert it to datetime object to make analysis easier, as datetime dtype have specific attribute and methods that can be applied


```python
# Concatenate 'stop_date' and 'stop_time'
# sep specify the separator that we want between the two values after the are combined
combined = ri.stop_date.str.cat(ri.stop_time, sep=' ')

# Convert 'combined' to datetime format
ri['stop_datetime'] = pd.to_datetime(combined)

# Examine the data types of the DataFrame
print(ri.dtypes)
```

    stop_date                     object
    stop_time                     object
    driver_gender                 object
    driver_race                   object
    violation_raw                 object
    violation                     object
    search_conducted                bool
    search_type                   object
    stop_outcome                  object
    is_arrested                     bool
    stop_duration                 object
    drugs_related_stop              bool
    district                      object
    stop_datetime         datetime64[ns]
    dtype: object


Finally, we can change the index of the data to the stop_datetime column that we have created. Using datetime object as index makes it easier for us to filter dataframe by date, plot the data by date, and so on. If you are not sure what is the best column to use as the index, the datetime object may be the best place to start.


```python
# Set 'stop_datetime' as the index
ri.set_index('stop_datetime', inplace=True)

# Examine the index
print(ri.index)

# Examine the columns
print(ri.columns)
```

    DatetimeIndex(['2005-01-04 12:55:00', '2005-01-23 23:15:00',
                   '2005-02-17 04:15:00', '2005-02-20 17:15:00',
                   '2005-02-24 01:20:00', '2005-03-14 10:00:00',
                   '2005-03-29 21:55:00', '2005-04-04 21:25:00',
                   '2005-07-14 11:20:00', '2005-07-14 19:55:00',
                   ...
                   '2015-12-31 13:23:00', '2015-12-31 18:59:00',
                   '2015-12-31 19:13:00', '2015-12-31 20:20:00',
                   '2015-12-31 20:50:00', '2015-12-31 21:21:00',
                   '2015-12-31 21:59:00', '2015-12-31 22:04:00',
                   '2015-12-31 22:09:00', '2015-12-31 22:47:00'],
                  dtype='datetime64[ns]', name='stop_datetime', length=86536, freq=None)
    Index(['stop_date', 'stop_time', 'driver_gender', 'driver_race',
           'violation_raw', 'violation', 'search_conducted', 'search_type',
           'stop_outcome', 'is_arrested', 'stop_duration', 'drugs_related_stop',
           'district'],
          dtype='object')


These are some of the basic steps in data wrangling, though you may need more time to perform wrangling on larger and complex dataset.

That's it for wrangling! We will continue to use this dataset for the next step in our data science journey; Exploratory Data Analysis(EDA)

The below code is just me exporting the data for our next step


```python
ri.to_csv("ri_eda.csv")
```
