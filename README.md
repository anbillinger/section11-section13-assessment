# Objectives
YW
* scrape a website for relevant information, store that information to a dataframe and save that dataframe as a csv file
* load in a dataframe and do the following
    * calculate the zscores of a given column
    * calculate the zscores of a point from a given column in the dataframe
    * calculate and plot the pmf and cdf of another column

# Part 1 - Webscraping
* use the following url scrape the first page of results
* for each item get the name of the item
* store the names to a dataframe and save that dataframe to csv then display
    * store the dataframe in the `data` folder in the repo
    * name the file `part1.csv` and make sure that when you write it you set `index=False`
* the head of the dataframe

* it should match the following
<img src="solutions/images/part1.png"/>


```python
url = "https://www.petsmart.com/dog/treats/dental-treats/#page_name=flyout&category=dog&cta=dentaltreat"
import requests
import pandas as pd
from bs4 import BeautifulSoup
```


```python
# scrape the data
html_page = requests.get(url)
soup = BeautifulSoup(html_page.content,'html.parser')
```


```python
treat_names = [x.attrs['title'] for x in soup.findAll('a',class_='name-link')]
treat_names
```




    ['Greenies Regular Dental Dog Treats',
     'Greenies Petite Dental Dog Treats',
     'Greenies Large Dental Dog Treats',
     'Pedigree Dentastix Large Dog Treats',
     'Greenies 6 Month+ Puppy Petite Dental Dog Treats',
     'Greenies 6 Month+ Puppy Dental Dog Treats',
     'Greenies 6 Month+ Puppy Teenie Dental Dog Treats',
     'Greenies Teenie Dental Dog Treats',
     'Authority® Dental & DHA Stick Puppy Treats Parsley Mint - Gluten Free, Grain Free',
     'Pedigree Dentastix Large Dog Sticks',
     'Milk-Bone Brushing Chews Large Dental Dog Treats',
     'Pedigree Dentastix Small/Medium Dog Sticks',
     'Pedigree Dentastix Triple Action Dental Dog Treats - Variety Pack',
     'WHIMZEES Variety Value Box Dental Dog Treat - Natural, Grain Free',
     'Pedigree Dentastix Mini Dog Sticks',
     'Virbac® C.E.T.® VeggieDent® Tartar Control Dog Chews',
     'Milk-Bone Brushing Chews Dental Dog Treat',
     'Authority® Dental & DHA Rings Puppy Treats Parsley Mint - Gluten Free, Grain Free',
     'Pedigree Dentastix Large Dog Sticks',
     'Pedigree Dentastix Triple Action Small Dog Treats - Fresh',
     'Greenies Teenie Dog Dental Treats - Blueberry',
     'Milk-Bone Brushing Chew Mini Dental Dog Treats',
     'Authority Dental & Multivitamin Large Dog Treats Parsley Mint - Gluten Free, Grain Free',
     'Authority Dental & Multivitamin Small Dog Treats Parsley Mint - Gluten Free, Grain Free']




```python
# load the data into a dataframe file
df = pd.DataFrame(treat_names,columns=['name'])
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
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Greenies Regular Dental Dog Treats</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Greenies Petite Dental Dog Treats</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Greenies Large Dental Dog Treats</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Pedigree Dentastix Large Dog Treats</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Greenies 6 Month+ Puppy Petite Dental Dog Treats</td>
    </tr>
  </tbody>
</table>
</div>




```python
# save the data as a csv file
df.to_csv('data/part1.csv',index=False)
```


```python
# display df.head()
df_new = pd.read_csv('data/part1.csv')
df_new.head()
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
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Greenies Regular Dental Dog Treats</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Greenies Petite Dental Dog Treats</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Greenies Large Dental Dog Treats</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Pedigree Dentastix Large Dog Treats</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Greenies 6 Month+ Puppy Petite Dental Dog Treats</td>
    </tr>
  </tbody>
</table>
</div>



# Part 2

load in the csv file located in the `data` folder called `part2.csv`

create a function that calculates the zscores of an array

then calculate the zscores for each column in part2.csv and add them as columns

See below for final result

<img src="solutions/images/part2_df_preview.png"/>


```python
import numpy as np
import scipy.stats as stats
df2 = pd.read_csv('data/part2.csv')
df2.head()
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
      <th>salaries</th>
      <th>NPS Score</th>
      <th>eventOutcome</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>44112.0</td>
      <td>-7.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>46777.0</td>
      <td>-12.0</td>
      <td>2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>50013.0</td>
      <td>50.0</td>
      <td>5</td>
    </tr>
    <tr>
      <td>3</td>
      <td>48983.0</td>
      <td>-13.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>50751.0</td>
      <td>-11.0</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create a function that calculates the zscores of an array
def calc_zscores(an_array):
    return stats.zscore(np.array(an_array))
```


```python
# calculate the zscore for each column and store them as a new column with the names used above
for i in [0,1,2]:
    new_col = df2.columns[i]+'_zscore'
    df2[new_col] = calc_zscores(df2[df2.columns[i]])
df2.head()
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
      <th>salaries</th>
      <th>NPS Score</th>
      <th>eventOutcome</th>
      <th>salaries_zscore</th>
      <th>NPS Score_zscore</th>
      <th>eventOutcome_zscore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>44112.0</td>
      <td>-7.0</td>
      <td>1</td>
      <td>-1.460301</td>
      <td>-0.913613</td>
      <td>-1.103276</td>
    </tr>
    <tr>
      <td>1</td>
      <td>46777.0</td>
      <td>-12.0</td>
      <td>2</td>
      <td>-0.794061</td>
      <td>-1.080776</td>
      <td>-0.668162</td>
    </tr>
    <tr>
      <td>2</td>
      <td>50013.0</td>
      <td>50.0</td>
      <td>5</td>
      <td>0.014927</td>
      <td>0.992046</td>
      <td>0.637182</td>
    </tr>
    <tr>
      <td>3</td>
      <td>48983.0</td>
      <td>-13.0</td>
      <td>0</td>
      <td>-0.242569</td>
      <td>-1.114209</td>
      <td>-1.538391</td>
    </tr>
    <tr>
      <td>4</td>
      <td>50751.0</td>
      <td>-11.0</td>
      <td>6</td>
      <td>0.199425</td>
      <td>-1.047343</td>
      <td>1.072296</td>
    </tr>
  </tbody>
</table>
</div>



# Part 3 
plot 'salaries' and 'NPS Score' on a subplot (1 row 2 columns) 
then repeat this for the zscores

see image below for reference
<img src="solutions/images/part2-plots.png"/>


```python
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
# plot for raw salaries and NPS Score data goes here
fig = plt.figure(figsize=(14,7))
axs=fig.subplots(1,2)
axs[0].hist(df2['salaries'])
axs[0].set_title('Salaries')
axs[1].hist(df2['NPS Score'])
axs[1].set_title('NPS Scores')
```




    Text(0.5, 1.0, 'NPS Scores')




![png](assessment_files/assessment_14_1.png)



```python
# plot for zscores for salaries and NPS Score data goes here
fig = plt.figure(figsize=(14,7))
axs=fig.subplots(1,2)
axs[0].hist(df2['salaries_zscore'])
axs[0].set_title('Salaries Z Scores')
axs[1].hist(df2['NPS Score_zscore'])
axs[1].set_title('NPS Scores Z scores')
```




    Text(0.5, 1.0, 'NPS Scores Z scores')




![png](assessment_files/assessment_15_1.png)


# Part 4 - PMF
using the column 'eventOutcomes'

create a PMF and plot the PMF as a bar chart

See image below for referenc

<img src="solutions/images/part4_pmf.png"/>


```python
pmf = df2['eventOutcome'].value_counts()/len(df2)
```




    Int64Index([4, 7, 3, 0, 6, 1, 2, 5], dtype='int64')




```python
plt.figure(figsize=(12,6))
plt.title('Event Outcomes')
plt.bar(pmf.index,pmf)
```




    <BarContainer object of 8 artists>




![png](assessment_files/assessment_18_1.png)


# Part 5 - CDF
plot the CDF of Event Outcomes as a scatter plot using the information above

See image below for reference 

<img src="solutions/images/part5_cmf.png"/>


```python
pmf_sort = pmf.sort_index(axis=0)
```


```python
plt.figure(figsize = (12,6))
plt.title('CMF for Event Outcomes')
plt.scatter(pmf_sort.index,np.cumsum(pmf_sort),)
```




    <matplotlib.collections.PathCollection at 0x19aaebc5c18>




![png](assessment_files/assessment_21_1.png)


# Bonus:
* using np.where find salaries with zscores <= -2.0

* calculate the skewness and kurtosis for the NPS Score column


```python
# find salaries with zscores <= 2.0 
df2.query('salaries_zscore<=-2')
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
      <th>salaries</th>
      <th>NPS Score</th>
      <th>eventOutcome</th>
      <th>salaries_zscore</th>
      <th>NPS Score_zscore</th>
      <th>eventOutcome_zscore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20</td>
      <td>39383.0</td>
      <td>47.0</td>
      <td>1</td>
      <td>-2.642533</td>
      <td>0.891748</td>
      <td>-1.103276</td>
    </tr>
    <tr>
      <td>41</td>
      <td>38063.0</td>
      <td>2.0</td>
      <td>5</td>
      <td>-2.972528</td>
      <td>-0.612719</td>
      <td>0.637182</td>
    </tr>
    <tr>
      <td>89</td>
      <td>41458.0</td>
      <td>65.0</td>
      <td>7</td>
      <td>-2.123791</td>
      <td>1.493535</td>
      <td>1.507411</td>
    </tr>
    <tr>
      <td>107</td>
      <td>40854.0</td>
      <td>27.0</td>
      <td>4</td>
      <td>-2.274789</td>
      <td>0.223096</td>
      <td>0.202067</td>
    </tr>
    <tr>
      <td>285</td>
      <td>40886.0</td>
      <td>43.0</td>
      <td>5</td>
      <td>-2.266789</td>
      <td>0.758018</td>
      <td>0.637182</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>4692</td>
      <td>38341.0</td>
      <td>37.0</td>
      <td>3</td>
      <td>-2.903029</td>
      <td>0.557422</td>
      <td>-0.233047</td>
    </tr>
    <tr>
      <td>4707</td>
      <td>41813.0</td>
      <td>96.0</td>
      <td>1</td>
      <td>-2.035042</td>
      <td>2.529946</td>
      <td>-1.103276</td>
    </tr>
    <tr>
      <td>4731</td>
      <td>41184.0</td>
      <td>21.0</td>
      <td>0</td>
      <td>-2.192290</td>
      <td>0.022500</td>
      <td>-1.538391</td>
    </tr>
    <tr>
      <td>4765</td>
      <td>40108.0</td>
      <td>43.0</td>
      <td>2</td>
      <td>-2.461286</td>
      <td>0.758018</td>
      <td>-0.668162</td>
    </tr>
    <tr>
      <td>4949</td>
      <td>38915.0</td>
      <td>13.0</td>
      <td>7</td>
      <td>-2.759531</td>
      <td>-0.244961</td>
      <td>1.507411</td>
    </tr>
  </tbody>
</table>
<p>123 rows × 6 columns</p>
</div>




```python
# calculate skewness and kurtosis of NPS Score column
print('Skewness:',round(stats.skew(df2['NPS Score']),4))
print('Kurtosis:',round(stats.kurtosis(df2['NPS Score']),4))
```

    Skewness: 0.0245
    Kurtosis: -0.0421
    

# run the cell below to convert your notebook to a README for assessment


```python
!jupyter nbconvert --to markdown assessment.ipynb && mv assessment.md README.md
```

    [NbConvertApp] Converting notebook assessment.ipynb to markdown
    [NbConvertApp] Support files will be in assessment_files\
    [NbConvertApp] Making directory assessment_files
    [NbConvertApp] Making directory assessment_files
    [NbConvertApp] Making directory assessment_files
    [NbConvertApp] Making directory assessment_files
    [NbConvertApp] Writing 487524 bytes to assessment.md
    


```python

```
