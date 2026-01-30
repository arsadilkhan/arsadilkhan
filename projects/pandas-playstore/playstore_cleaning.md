### 0. Project Overview 
- Objective of the mini-project: Analyze the Google Play Store dataset to assess data quality, explore key characteristics of mobile applications, and identify actionable product and business insights through basic data analysis, visualization, and hypothesis testing.
- Dataset: Google Play Store
- Skills covered:
  - Data inspection
  - Missing values
  - Duplicates handling
  - Filtering & slicing
  - Data transformation
  - Aggregation & pivot tables

### 1. Dataset Description
- App — application name
- Category — application category
- Rating — user rating
- Reviews — number of user reviews
- Size — application size
- Installs — number of installs
- Type — free or paid
- Price — application price
- Content Rating — target age group
- Genres — application genres
- Last Updated — last update date
- Current Ver — current app version
- Android Ver — minimum required Android version


### 2. Initial Data Inspection 



### 3. Missing Value Analysis



### 4. Data Slicing and Concatenation



### 5. Handling Duplicate Applications



### 6. Column Name Standardization



### 7. Free vs Paid Applications



### 8. Filtering Educational Applications



### 9. Price Column Transformation



### 10. Aggregation and Pivot Table



### 11. Final Notes




```python
import pandas as pd
```


```python
playstore = pd.read_csv('playstore.csv')
```


```python
data_head = playstore.head(3)
data_tail = playstore.tail(3)
data_head
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
      <th>Unnamed: 0</th>
      <th>App</th>
      <th>Category</th>
      <th>Rating</th>
      <th>Reviews</th>
      <th>Size</th>
      <th>Installs</th>
      <th>Type</th>
      <th>Price</th>
      <th>Content Rating</th>
      <th>Genres</th>
      <th>Last Updated</th>
      <th>Current Ver</th>
      <th>Android Ver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Photo Editor &amp; Candy Camera &amp; Grid &amp; ScrapBook</td>
      <td>ART_AND_DESIGN</td>
      <td>4.1</td>
      <td>159</td>
      <td>19M</td>
      <td>10,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Art &amp; Design</td>
      <td>January 7, 2018</td>
      <td>1.0.0</td>
      <td>4.0.3 and up</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Coloring book moana</td>
      <td>ART_AND_DESIGN</td>
      <td>3.9</td>
      <td>967</td>
      <td>14M</td>
      <td>500,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Art &amp; Design;Pretend Play</td>
      <td>January 15, 2018</td>
      <td>2.0.0</td>
      <td>4.0.3 and up</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>U Launcher Lite – FREE Live Cool Themes, Hide ...</td>
      <td>ART_AND_DESIGN</td>
      <td>4.7</td>
      <td>87510</td>
      <td>8.7M</td>
      <td>5,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Art &amp; Design</td>
      <td>August 1, 2018</td>
      <td>1.2.4</td>
      <td>4.0.3 and up</td>
    </tr>
  </tbody>
</table>
</div>




```python
n_col = playstore.shape[0]
n_rows = playstore.shape[1]
n_col
n_rows
```




    14




```python
playstore['App'].nunique()
```




    9659




```python
rating_missing = playstore['Rating'].isna().sum()
rating_missing
```




    np.int64(1474)




```python
rating_missing=len(playstore[playstore['Rating'].isna()])
rating_missing
```




    1474




```python
df1 = playstore[['App', 'Size', 'Genres', 'Current Ver']].head(3)

ps2 = playstore[['App', 'Size', 'Genres', 'Current Ver']]
df2 = ps2.loc[6:8]

df3 = ps2.loc[16:19]

df4 = pd.concat([df1, df2, df3], axis = 0)
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
      <th>App</th>
      <th>Size</th>
      <th>Genres</th>
      <th>Current Ver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Photo Editor &amp; Candy Camera &amp; Grid &amp; ScrapBook</td>
      <td>19M</td>
      <td>Art &amp; Design</td>
      <td>1.0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Coloring book moana</td>
      <td>14M</td>
      <td>Art &amp; Design;Pretend Play</td>
      <td>2.0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>U Launcher Lite – FREE Live Cool Themes, Hide ...</td>
      <td>8.7M</td>
      <td>Art &amp; Design</td>
      <td>1.2.4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Smoke Effect Photo Maker - Smoke Editor</td>
      <td>19M</td>
      <td>Art &amp; Design</td>
      <td>1.1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Infinite Painter</td>
      <td>29M</td>
      <td>Art &amp; Design</td>
      <td>6.1.61.1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Garden Coloring Book</td>
      <td>33M</td>
      <td>Art &amp; Design</td>
      <td>2.9.2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Photo Designer - Write your name with shapes</td>
      <td>5.5M</td>
      <td>Art &amp; Design</td>
      <td>3.1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>350 Diy Room Decor Ideas</td>
      <td>17M</td>
      <td>Art &amp; Design</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>FlipaClip - Cartoon animation</td>
      <td>39M</td>
      <td>Art &amp; Design</td>
      <td>2.2.5</td>
    </tr>
    <tr>
      <th>19</th>
      <td>ibis Paint X</td>
      <td>31M</td>
      <td>Art &amp; Design</td>
      <td>5.5.4</td>
    </tr>
  </tbody>
</table>
</div>




```python
cols = ['App', 'Size', 'Genres', 'Current Ver']

df4 = pd.concat([
    playstore[cols].iloc[0:3],
    playstore[cols].loc[6:8],
    playstore[cols].loc[16:19]
])
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
      <th>App</th>
      <th>Size</th>
      <th>Genres</th>
      <th>Current Ver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Photo Editor &amp; Candy Camera &amp; Grid &amp; ScrapBook</td>
      <td>19M</td>
      <td>Art &amp; Design</td>
      <td>1.0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Coloring book moana</td>
      <td>14M</td>
      <td>Art &amp; Design;Pretend Play</td>
      <td>2.0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>U Launcher Lite – FREE Live Cool Themes, Hide ...</td>
      <td>8.7M</td>
      <td>Art &amp; Design</td>
      <td>1.2.4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Smoke Effect Photo Maker - Smoke Editor</td>
      <td>19M</td>
      <td>Art &amp; Design</td>
      <td>1.1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Infinite Painter</td>
      <td>29M</td>
      <td>Art &amp; Design</td>
      <td>6.1.61.1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Garden Coloring Book</td>
      <td>33M</td>
      <td>Art &amp; Design</td>
      <td>2.9.2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Photo Designer - Write your name with shapes</td>
      <td>5.5M</td>
      <td>Art &amp; Design</td>
      <td>3.1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>350 Diy Room Decor Ideas</td>
      <td>17M</td>
      <td>Art &amp; Design</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>FlipaClip - Cartoon animation</td>
      <td>39M</td>
      <td>Art &amp; Design</td>
      <td>2.2.5</td>
    </tr>
    <tr>
      <th>19</th>
      <td>ibis Paint X</td>
      <td>31M</td>
      <td>Art &amp; Design</td>
      <td>5.5.4</td>
    </tr>
  </tbody>
</table>
</div>




```python
#removing dubplicates and reset index
unique_playstore = (
    playstore
    .drop_duplicates(subset=['App'])
    .reset_index(drop=True)
)
unique_playstore


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
      <th>Unnamed: 0</th>
      <th>App</th>
      <th>Category</th>
      <th>Rating</th>
      <th>Reviews</th>
      <th>Size</th>
      <th>Installs</th>
      <th>Type</th>
      <th>Price</th>
      <th>Content Rating</th>
      <th>Genres</th>
      <th>Last Updated</th>
      <th>Current Ver</th>
      <th>Android Ver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Photo Editor &amp; Candy Camera &amp; Grid &amp; ScrapBook</td>
      <td>ART_AND_DESIGN</td>
      <td>4.1</td>
      <td>159</td>
      <td>19M</td>
      <td>10,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Art &amp; Design</td>
      <td>January 7, 2018</td>
      <td>1.0.0</td>
      <td>4.0.3 and up</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Coloring book moana</td>
      <td>ART_AND_DESIGN</td>
      <td>3.9</td>
      <td>967</td>
      <td>14M</td>
      <td>500,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Art &amp; Design;Pretend Play</td>
      <td>January 15, 2018</td>
      <td>2.0.0</td>
      <td>4.0.3 and up</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>U Launcher Lite – FREE Live Cool Themes, Hide ...</td>
      <td>ART_AND_DESIGN</td>
      <td>4.7</td>
      <td>87510</td>
      <td>8.7M</td>
      <td>5,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Art &amp; Design</td>
      <td>August 1, 2018</td>
      <td>1.2.4</td>
      <td>4.0.3 and up</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Sketch - Draw &amp; Paint</td>
      <td>ART_AND_DESIGN</td>
      <td>4.5</td>
      <td>215644</td>
      <td>25M</td>
      <td>50,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Teen</td>
      <td>Art &amp; Design</td>
      <td>June 8, 2018</td>
      <td>Varies with device</td>
      <td>4.2 and up</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Pixel Draw - Number Art Coloring Book</td>
      <td>ART_AND_DESIGN</td>
      <td>4.3</td>
      <td>967</td>
      <td>2.8M</td>
      <td>100,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Art &amp; Design;Creativity</td>
      <td>June 20, 2018</td>
      <td>1.1</td>
      <td>4.4 and up</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9654</th>
      <td>10835</td>
      <td>Sya9a Maroc - FR</td>
      <td>FAMILY</td>
      <td>4.5</td>
      <td>38</td>
      <td>53M</td>
      <td>5,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education</td>
      <td>July 25, 2017</td>
      <td>1.48</td>
      <td>4.1 and up</td>
    </tr>
    <tr>
      <th>9655</th>
      <td>10836</td>
      <td>Fr. Mike Schmitz Audio Teachings</td>
      <td>FAMILY</td>
      <td>5.0</td>
      <td>4</td>
      <td>3.6M</td>
      <td>100+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education</td>
      <td>July 6, 2018</td>
      <td>1.0</td>
      <td>4.1 and up</td>
    </tr>
    <tr>
      <th>9656</th>
      <td>10837</td>
      <td>Parkinson Exercices FR</td>
      <td>MEDICAL</td>
      <td>NaN</td>
      <td>3</td>
      <td>9.5M</td>
      <td>1,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Medical</td>
      <td>January 20, 2017</td>
      <td>1.0</td>
      <td>2.2 and up</td>
    </tr>
    <tr>
      <th>9657</th>
      <td>10838</td>
      <td>The SCP Foundation DB fr nn5n</td>
      <td>BOOKS_AND_REFERENCE</td>
      <td>4.5</td>
      <td>114</td>
      <td>Varies with device</td>
      <td>1,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Mature 17+</td>
      <td>Books &amp; Reference</td>
      <td>January 19, 2015</td>
      <td>Varies with device</td>
      <td>Varies with device</td>
    </tr>
    <tr>
      <th>9658</th>
      <td>10839</td>
      <td>iHoroscope - 2018 Daily Horoscope &amp; Astrology</td>
      <td>LIFESTYLE</td>
      <td>4.5</td>
      <td>398307</td>
      <td>19M</td>
      <td>10,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Lifestyle</td>
      <td>July 25, 2018</td>
      <td>Varies with device</td>
      <td>Varies with device</td>
    </tr>
  </tbody>
</table>
<p>9659 rows × 14 columns</p>
</div>




```python
#column name standardization 

unique_playstore.columns = (unique_playstore.columns.str.lower().str.replace(' ', '_')) 

unique_playstore.columns
```




    Index(['unnamed:_0', 'app', 'category', 'rating', 'reviews', 'size',
           'installs', 'type', 'price', 'content_rating', 'genres', 'last_updated',
           'current_ver', 'android_ver'],
          dtype='object')




```python
#calculate share of free vs paid applications
```


```python
unique_playstore['type'].value_counts()
```




    type
    Free    8902
    Paid     756
    Name: count, dtype: int64




```python
free_paid_cnt = unique_playstore['type'].value_counts().reset_index()
free_paid_cnt.columns = ['type', 'count']
free_paid_cnt

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
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Free</td>
      <td>8902</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Paid</td>
      <td>756</td>
    </tr>
  </tbody>
</table>
</div>




```python
unique_playstore['type'].value_counts(normalize=True).round(2)

```




    type
    Free    0.92
    Paid    0.08
    Name: proportion, dtype: float64




```python
# Filtering Educational Applications

education_playstore = unique_playstore[ 
    (unique_playstore['category'] == 'EDUCATION')
    & (unique_playstore['reviews'] > 1000)
    ].reset_index(drop=True)
    
education_playstore

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
      <th>unnamed:_0</th>
      <th>app</th>
      <th>category</th>
      <th>rating</th>
      <th>reviews</th>
      <th>size</th>
      <th>installs</th>
      <th>type</th>
      <th>price</th>
      <th>content_rating</th>
      <th>genres</th>
      <th>last_updated</th>
      <th>current_ver</th>
      <th>android_ver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>699</td>
      <td>Duolingo: Learn Languages Free</td>
      <td>EDUCATION</td>
      <td>4.7</td>
      <td>6289924</td>
      <td>Varies with device</td>
      <td>100,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education;Education</td>
      <td>August 1, 2018</td>
      <td>Varies with device</td>
      <td>Varies with device</td>
    </tr>
    <tr>
      <th>1</th>
      <td>700</td>
      <td>TED</td>
      <td>EDUCATION</td>
      <td>4.6</td>
      <td>181893</td>
      <td>18M</td>
      <td>10,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone 10+</td>
      <td>Education</td>
      <td>July 27, 2018</td>
      <td>3.2.5</td>
      <td>4.1 and up</td>
    </tr>
    <tr>
      <th>2</th>
      <td>701</td>
      <td>English Communication - Learn English for Chin...</td>
      <td>EDUCATION</td>
      <td>4.7</td>
      <td>2544</td>
      <td>18M</td>
      <td>100,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education</td>
      <td>December 29, 2017</td>
      <td>3.1</td>
      <td>4.0 and up</td>
    </tr>
    <tr>
      <th>3</th>
      <td>702</td>
      <td>Khan Academy</td>
      <td>EDUCATION</td>
      <td>4.6</td>
      <td>85375</td>
      <td>21M</td>
      <td>5,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education</td>
      <td>July 27, 2018</td>
      <td>5.0.0</td>
      <td>4.1 and up</td>
    </tr>
    <tr>
      <th>4</th>
      <td>703</td>
      <td>Learn English with Wlingua</td>
      <td>EDUCATION</td>
      <td>4.7</td>
      <td>314299</td>
      <td>3.3M</td>
      <td>10,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education</td>
      <td>May 2, 2018</td>
      <td>1.94.9</td>
      <td>4.0 and up</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>93</th>
      <td>848</td>
      <td>SoloLearn: Learn to Code for Free</td>
      <td>EDUCATION</td>
      <td>4.8</td>
      <td>256079</td>
      <td>7.6M</td>
      <td>1,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Teen</td>
      <td>Education</td>
      <td>July 12, 2018</td>
      <td>2.2.4</td>
      <td>4.0.3 and up</td>
    </tr>
    <tr>
      <th>94</th>
      <td>849</td>
      <td>Kids Learn Languages by Mondly</td>
      <td>EDUCATION</td>
      <td>4.4</td>
      <td>2078</td>
      <td>Varies with device</td>
      <td>100,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education;Education</td>
      <td>December 24, 2017</td>
      <td>1.0.2</td>
      <td>4.1 and up</td>
    </tr>
    <tr>
      <th>95</th>
      <td>850</td>
      <td>Blinkist - Nonfiction Books</td>
      <td>EDUCATION</td>
      <td>4.1</td>
      <td>16103</td>
      <td>13M</td>
      <td>1,000,000+</td>
      <td>Free</td>
      <td>0</td>
      <td>Everyone</td>
      <td>Education</td>
      <td>July 31, 2018</td>
      <td>5.7.1</td>
      <td>4.1 and up</td>
    </tr>
    <tr>
      <th>96</th>
      <td>853</td>
      <td>Toca Life: City</td>
      <td>EDUCATION</td>
      <td>4.7</td>
      <td>31085</td>
      <td>24M</td>
      <td>500,000+</td>
      <td>Paid</td>
      <td>$3.99</td>
      <td>Everyone</td>
      <td>Education;Pretend Play</td>
      <td>July 6, 2018</td>
      <td>1.5-play</td>
      <td>4.4 and up</td>
    </tr>
    <tr>
      <th>97</th>
      <td>854</td>
      <td>Toca Life: Hospital</td>
      <td>EDUCATION</td>
      <td>4.7</td>
      <td>3528</td>
      <td>24M</td>
      <td>100,000+</td>
      <td>Paid</td>
      <td>$3.99</td>
      <td>Everyone</td>
      <td>Education;Pretend Play</td>
      <td>June 12, 2018</td>
      <td>1.1.1-play</td>
      <td>4.4 and up</td>
    </tr>
  </tbody>
</table>
<p>98 rows × 14 columns</p>
</div>




```python
#removing excess symbols from price column and transferring data type to float

unique_playstore['price'] = unique_playstore['price'].str.replace('$','').astype('float')
unique_playstore['price'].unique()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[91], line 3
          1 #removing excess symbols from price column and transferring data type to float
    ----> 3 unique_playstore['price'] = unique_playstore['price'].str.replace('$','').astype('float')
          4 unique_playstore['price'].unique()
    

    File ~\AppData\Local\Programs\Python\Python313\Lib\site-packages\pandas\core\generic.py:6299, in NDFrame.__getattr__(self, name)
       6292 if (
       6293     name not in self._internal_names_set
       6294     and name not in self._metadata
       6295     and name not in self._accessors
       6296     and self._info_axis._can_hold_identifiers_and_holds_name(name)
       6297 ):
       6298     return self[name]
    -> 6299 return object.__getattribute__(self, name)
    

    File ~\AppData\Local\Programs\Python\Python313\Lib\site-packages\pandas\core\accessor.py:224, in CachedAccessor.__get__(self, obj, cls)
        221 if obj is None:
        222     # we're accessing the attribute of the class, i.e., Dataset.geo
        223     return self._accessor
    --> 224 accessor_obj = self._accessor(obj)
        225 # Replace the property with the accessor object. Inspired by:
        226 # https://www.pydanny.com/cached-property.html
        227 # We need to use object.__setattr__ because we overwrite __setattr__ on
        228 # NDFrame
        229 object.__setattr__(obj, self._name, accessor_obj)
    

    File ~\AppData\Local\Programs\Python\Python313\Lib\site-packages\pandas\core\strings\accessor.py:191, in StringMethods.__init__(self, data)
        188 def __init__(self, data) -> None:
        189     from pandas.core.arrays.string_ import StringDtype
    --> 191     self._inferred_dtype = self._validate(data)
        192     self._is_categorical = isinstance(data.dtype, CategoricalDtype)
        193     self._is_string = isinstance(data.dtype, StringDtype)
    

    File ~\AppData\Local\Programs\Python\Python313\Lib\site-packages\pandas\core\strings\accessor.py:245, in StringMethods._validate(data)
        242 inferred_dtype = lib.infer_dtype(values, skipna=True)
        244 if inferred_dtype not in allowed_types:
    --> 245     raise AttributeError("Can only use .str accessor with string values!")
        246 return inferred_dtype
    

    AttributeError: Can only use .str accessor with string values!



```python
unique_playstore['price'].unique()
```




    array([  0.  ,   4.99,   3.99,   6.99,   1.49,   2.99,   7.99,   5.99,
             3.49,   1.99,   9.99,   7.49,   0.99,   9.  ,   5.49,  10.  ,
            24.99,  11.99,  79.99,  16.99,  14.99,   1.  ,  29.99,  12.99,
             2.49,  10.99,   1.5 ,  19.99,  15.99,  33.99,  74.99,  39.99,
             3.95,   4.49,   1.7 ,   8.99,   2.  ,   3.88,  25.99, 399.99,
            17.99, 400.  ,   3.02,   1.76,   4.84,   4.77,   1.61,   2.5 ,
             1.59,   6.49,   1.29,   5.  ,  13.99, 299.99, 379.99,  37.99,
            18.99, 389.99,  19.9 ,   8.49,   1.75,  14.  ,   4.85,  46.99,
           109.99, 154.99,   3.08,   2.59,   4.8 ,   1.96,  19.4 ,   3.9 ,
             4.59,  15.46,   3.04,   4.29,   2.6 ,   3.28,   4.6 ,  28.99,
             2.95,   2.9 ,   1.97, 200.  ,  89.99,   2.56,  30.99,   3.61,
           394.99,   1.26,   1.2 ,   1.04])




```python
pivot = (
    unique_playstore
    .groupby(['category', 'type'])
    .agg(
        mean_price=('price', 'mean'),
        mean_rating=('rating', 'mean'),
        mean_reviews=('reviews', 'mean')
    )
    .round(2)
)

pivot

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
      <th>mean_price</th>
      <th>mean_rating</th>
      <th>mean_reviews</th>
    </tr>
    <tr>
      <th>category</th>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">ART_AND_DESIGN</th>
      <th>Free</th>
      <td>0.00</td>
      <td>4.34</td>
      <td>23230.11</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>1.99</td>
      <td>4.73</td>
      <td>722.00</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">AUTO_AND_VEHICLES</th>
      <th>Free</th>
      <td>0.00</td>
      <td>4.18</td>
      <td>14140.28</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>4.49</td>
      <td>4.60</td>
      <td>1387.67</td>
    </tr>
    <tr>
      <th>BEAUTY</th>
      <th>Free</th>
      <td>0.00</td>
      <td>4.28</td>
      <td>7476.23</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>TRAVEL_AND_LOCAL</th>
      <th>Paid</th>
      <td>4.16</td>
      <td>4.10</td>
      <td>1506.08</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">VIDEO_PLAYERS</th>
      <th>Free</th>
      <td>0.00</td>
      <td>4.04</td>
      <td>424347.18</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>2.62</td>
      <td>4.10</td>
      <td>3341.75</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">WEATHER</th>
      <th>Free</th>
      <td>0.00</td>
      <td>4.23</td>
      <td>171249.62</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>4.05</td>
      <td>4.37</td>
      <td>17055.12</td>
    </tr>
  </tbody>
</table>
<p>63 rows × 3 columns</p>
</div>




```python
pivot = pd.pivot_table(
    unique_playstore,
    index=['category', 'type'],
    values=['price', 'rating', 'reviews'],
    aggfunc='mean'
).round(2)

pivot.columns = ['mean_price', 'mean_rating', 'mean_reviews']

```


```python
pivott = (
    unique_playstore
    .groupby(['category', 'type'])
    .agg(
        price_mean=('price', 'mean'),
        price_median=('price', 'median'),
        price_min=('price', 'min'),
        price_max=('price', 'max'),

        rating_mean=('rating', 'mean'),
        rating_median=('rating', 'median'),
        reviews_mean=('reviews', 'mean'),
        reviews_max=('rating', 'max')
    )
    .round(2)
)

pivott
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
      <th>price_mean</th>
      <th>price_median</th>
      <th>price_min</th>
      <th>price_max</th>
      <th>rating_mean</th>
      <th>rating_median</th>
      <th>reviews_mean</th>
      <th>reviews_max</th>
    </tr>
    <tr>
      <th>category</th>
      <th>type</th>
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
      <th rowspan="2" valign="top">ART_AND_DESIGN</th>
      <th>Free</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>4.34</td>
      <td>4.40</td>
      <td>23230.11</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>1.99</td>
      <td>1.99</td>
      <td>1.99</td>
      <td>1.99</td>
      <td>4.73</td>
      <td>4.70</td>
      <td>722.00</td>
      <td>4.8</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">AUTO_AND_VEHICLES</th>
      <th>Free</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>4.18</td>
      <td>4.30</td>
      <td>14140.28</td>
      <td>4.9</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>4.49</td>
      <td>1.99</td>
      <td>1.49</td>
      <td>9.99</td>
      <td>4.60</td>
      <td>4.60</td>
      <td>1387.67</td>
      <td>4.6</td>
    </tr>
    <tr>
      <th>BEAUTY</th>
      <th>Free</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>4.28</td>
      <td>4.30</td>
      <td>7476.23</td>
      <td>4.9</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>TRAVEL_AND_LOCAL</th>
      <th>Paid</th>
      <td>4.16</td>
      <td>3.49</td>
      <td>1.49</td>
      <td>8.99</td>
      <td>4.10</td>
      <td>4.25</td>
      <td>1506.08</td>
      <td>4.7</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">VIDEO_PLAYERS</th>
      <th>Free</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>4.04</td>
      <td>4.20</td>
      <td>424347.18</td>
      <td>4.9</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>2.62</td>
      <td>1.74</td>
      <td>0.99</td>
      <td>5.99</td>
      <td>4.10</td>
      <td>4.25</td>
      <td>3341.75</td>
      <td>4.8</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">WEATHER</th>
      <th>Free</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>4.23</td>
      <td>4.30</td>
      <td>171249.62</td>
      <td>4.8</td>
    </tr>
    <tr>
      <th>Paid</th>
      <td>4.05</td>
      <td>3.49</td>
      <td>1.99</td>
      <td>6.99</td>
      <td>4.37</td>
      <td>4.50</td>
      <td>17055.12</td>
      <td>4.8</td>
    </tr>
  </tbody>
</table>
<p>63 rows × 8 columns</p>
</div>




```python

```
