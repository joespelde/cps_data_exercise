# CPS Data Project

### Import packages and csv files


```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline

df = pd.read_csv('faketeacherData.csv')
count_df = pd.read_csv('teacherdatacount.csv')
```

### Inspect datasets for datatype and no. of values


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2459 entries, 0 to 2458
    Data columns (total 16 columns):
    IEIN                      1336 non-null float64
    LocalTeacherID            2267 non-null object
    LastName                  2267 non-null object
    FirstName                 2267 non-null object
    BirthDate                 2267 non-null object
    SchoolYear                2459 non-null int64
    ServingLocationRCDTS      2459 non-null object
    EmployerRCDTS             2459 non-null object
    Term                      2459 non-null object
    StateCourseCode           2459 non-null object
    LocalCourseID             0 non-null float64
    LocalCourseTitle          0 non-null float64
    SectionNumber             2459 non-null object
    TeacherCourseStartDate    2267 non-null object
    EISPositionCode           2267 non-null float64
    TeacherCommitment         2267 non-null float64
    dtypes: float64(5), int64(1), object(10)
    memory usage: 307.5+ KB



```python
count_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 40 entries, 0 to 39
    Data columns (total 6 columns):
    CPS School ID                  40 non-null int64
    CPS School Name                40 non-null object
    School Type                    40 non-null object
    Network                        40 non-null object
    ISBE RCDTS Code                40 non-null object
    Number of Teachers Expected    40 non-null int64
    dtypes: int64(2), object(4)
    memory usage: 2.0+ KB


### Rename TeacherDataCount Columns for easier manipulation



```python
count_df.rename(columns={'CPS School ID':'cps_school_id',
                        'CPS School Name':'cps_school_name',
                        'School Type': 'school_type',
                        'Network': 'Network',
                        'ISBE RCDTS Code': 'isbe_rcdts_code',
                        'Number of Teachers Expected':'no_of_teachers_expected'},
                       inplace=True)
```

### Find number of teachers submitted based on TeacherDataCount


```python
# Find unique values
locations = count_df['isbe_rcdts_code'].unique()
```


```python
# Loop through and compare number of teachers expected to number of teachers submitted for each unique rcdts code
for location in locations:
    print(location) # unique rcdts code
    print(count_df[count_df['isbe_rcdts_code']==location]['no_of_teachers_expected'].sum()) #no. of teachers expected
    print((df.ServingLocationRCDTS == location).sum()) # no. of teachers submitted
    

```

    15016299025270C
    33
    33
    15016299025266C
    34
    34
    15016299025268C
    35
    35
    15016299025049C
    33
    33
    15016299025262C
    30
    30
    15016299025261C
    32
    32
    15016299025085C
    43
    43
    15016299025284C
    37
    37
    15016299025263C
    37
    37
    15016299025259C
    40
    2
    15016299025267C
    45
    45
    15016299025086C
    36
    36
    15016299025260C
    37
    37
    15016299025285C
    45
    45
    15016299025264C
    32
    32
    15016299025276C
    47
    47
    15016299025283C
    36
    36
    15016299025273C
    42
    207
    15016299025109C
    35
    35
    15016299025274C
    45
    45
    15016299025064C
    43
    43
    15016299025056C
    41
    41
    15016299025057C
    33
    33
    15016299025058C
    34
    34
    15016299025062C
    44
    44
    15016299025059C
    78
    78
    15016299025073C
    26
    26
    15016299025063C
    40
    40
    15016299025065C
    35
    35
    15016299025055C
    42
    42
    15016299025066C
    40
    40
    15016299025060C
    37
    37
    15016299025054C
    29
    29
    15016299025067C
    35
    35
    15016299025068C
    38
    820
    15016299025069C
    34
    34
    15016299025071C
    42
    42
    15016299025070C
    41
    41
    15016299025072C
    37
    37
    15016299025061C
    47
    47


## Remove entries with missing mandatory info for ISBE


```python
# Remove entries with missing teacher info in any of the following collumns 
df = df.dropna(axis=0, subset=['IEIN','LastName','FirstName','BirthDate',
                                       'SchoolYear','ServingLocationRCDTS','EmployerRCDTS',
                                       'Term','StateCourseCode','SectionNumber',
                                      'TeacherCourseStartDate','EISPositionCode','TeacherCommitment'])
```


```python
# Progress check of first 50 entries
df.head(50)
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
      <th>IEIN</th>
      <th>LocalTeacherID</th>
      <th>LastName</th>
      <th>FirstName</th>
      <th>BirthDate</th>
      <th>SchoolYear</th>
      <th>ServingLocationRCDTS</th>
      <th>EmployerRCDTS</th>
      <th>Term</th>
      <th>StateCourseCode</th>
      <th>LocalCourseID</th>
      <th>LocalCourseTitle</th>
      <th>SectionNumber</th>
      <th>TeacherCourseStartDate</th>
      <th>EISPositionCode</th>
      <th>TeacherCommitment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>374529.0</td>
      <td>804678</td>
      <td>Bernard</td>
      <td>Jorge</td>
      <td>11/5/1990</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>04051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>World History-Overview</td>
      <td>08/20/2018</td>
      <td>201.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>369472.0</td>
      <td>280726</td>
      <td>Rios</td>
      <td>Teagan</td>
      <td>11/20/1971</td>
      <td>2019</td>
      <td>15016299025059C</td>
      <td>15016299025059C</td>
      <td>S1</td>
      <td>21051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Technological Literacy</td>
      <td>01/21/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>509509.0</td>
      <td>251432</td>
      <td>Morris</td>
      <td>Noel</td>
      <td>6/11/1962</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>02051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Pre-Algebra</td>
      <td>01/13/2018</td>
      <td>200.0</td>
      <td>0.36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>922488.0</td>
      <td>303080</td>
      <td>Fernandez</td>
      <td>Dominique</td>
      <td>1/20/1982</td>
      <td>2019</td>
      <td>15016299025266C</td>
      <td>15016299025266C</td>
      <td>Y1</td>
      <td>01001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Language Arts I (9th grade)</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>689032.0</td>
      <td>344652</td>
      <td>Garrison</td>
      <td>Benjamin</td>
      <td>9/1/1974</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>01151A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Public Speaking</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>7</th>
      <td>34560.0</td>
      <td>254873</td>
      <td>Andrews</td>
      <td>Malcolm</td>
      <td>1/19/1990</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>17101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Exploration of Electricity/Electronics</td>
      <td>08/20/2018</td>
      <td>205.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>9</th>
      <td>548398.0</td>
      <td>228275</td>
      <td>Walls</td>
      <td>Nathen</td>
      <td>11/1/1964</td>
      <td>2019</td>
      <td>15016299025059C</td>
      <td>15016299025059C</td>
      <td>S1</td>
      <td>04001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>World Geography</td>
      <td>8/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>10</th>
      <td>306780.0</td>
      <td>dkelly</td>
      <td>Kelly</td>
      <td>Donavan</td>
      <td>4/18/1969</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>03101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Chemistry</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>11</th>
      <td>414712.0</td>
      <td>352448</td>
      <td>Cochran</td>
      <td>Carl</td>
      <td>4/8/1934</td>
      <td>2019</td>
      <td>15016299025276C</td>
      <td>15016299025276C</td>
      <td>S1</td>
      <td>02133A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>IB Mathematics and Computing-SL</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>13</th>
      <td>333744.0</td>
      <td>dbell</td>
      <td>Bell</td>
      <td>Devon</td>
      <td>8/4/1962</td>
      <td>2019</td>
      <td>15016299025065C</td>
      <td>15016299025065C</td>
      <td>Q1</td>
      <td>01101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Composition (freshmen and sophomores)</td>
      <td>02/10/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>14</th>
      <td>866618.0</td>
      <td>639272</td>
      <td>Duffy</td>
      <td>Lamar</td>
      <td>11/18/1970</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>04051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>World History-Overview</td>
      <td>02/11/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>15</th>
      <td>171578.0</td>
      <td>717956</td>
      <td>Soto</td>
      <td>Roland</td>
      <td>8/14/1982</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>16001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Exploration of Hospitality Careers</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>16</th>
      <td>443020.0</td>
      <td>460113</td>
      <td>Dodson</td>
      <td>Desmond</td>
      <td>4/6/1966</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>08001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Physical Education</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>17</th>
      <td>611044.0</td>
      <td>652417</td>
      <td>Daniels</td>
      <td>Grady</td>
      <td>3/11/1970</td>
      <td>2019</td>
      <td>15016299025262C</td>
      <td>15016299025262C</td>
      <td>S1</td>
      <td>02101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Number Theory</td>
      <td>08/20/2018</td>
      <td>201.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>21</th>
      <td>732701.0</td>
      <td>535618</td>
      <td>Cortez</td>
      <td>Dexter</td>
      <td>8/10/1990</td>
      <td>2019</td>
      <td>15016299025054C</td>
      <td>15016299025054C</td>
      <td>Y1</td>
      <td>09151A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Air Force Junior ROTC I</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>30</th>
      <td>288918.0</td>
      <td>785030</td>
      <td>Hanson</td>
      <td>Konnor</td>
      <td>9/17/1975</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>21001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Pre-Engineering Technology</td>
      <td>01/23/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>31</th>
      <td>332218.0</td>
      <td>111114</td>
      <td>Webb</td>
      <td>Ismael</td>
      <td>8/15/1965</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>Y1</td>
      <td>12001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Business/Office Career Exploration</td>
      <td>08/18/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>34</th>
      <td>267788.0</td>
      <td>219987</td>
      <td>Moses</td>
      <td>Pedro</td>
      <td>11/15/1971</td>
      <td>2019</td>
      <td>15016299025285C</td>
      <td>15016299025285C</td>
      <td>S1</td>
      <td>11051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Audio/Visual Production</td>
      <td>02/01/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>35</th>
      <td>948403.0</td>
      <td>375136</td>
      <td>Escobar</td>
      <td>Andres</td>
      <td>3/18/1956</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>Y1</td>
      <td>19101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Cosmetology-Licensing</td>
      <td>02/25/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>37</th>
      <td>17392.0</td>
      <td>179280</td>
      <td>Conner</td>
      <td>Asher</td>
      <td>8/16/1967</td>
      <td>2019</td>
      <td>15016299025056C</td>
      <td>15016299025056C</td>
      <td>S1</td>
      <td>04051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>World History-Overview</td>
      <td>02/16/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>39</th>
      <td>583282.0</td>
      <td>310357</td>
      <td>Meyers</td>
      <td>Maximilian</td>
      <td>5/20/1963</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>09051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Army Junior ROTC I</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>41</th>
      <td>499695.0</td>
      <td>763300</td>
      <td>Vang</td>
      <td>Reagan</td>
      <td>9/21/1981</td>
      <td>2019</td>
      <td>15016299025284C</td>
      <td>15016299025284C</td>
      <td>S1</td>
      <td>16051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Exploration of Restaurant, Food and Beverage S...</td>
      <td>01/14/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>43</th>
      <td>513041.0</td>
      <td>140822</td>
      <td>Shields</td>
      <td>Rocco</td>
      <td>11/2/1989</td>
      <td>2019</td>
      <td>15016299025260C</td>
      <td>15016299025260C</td>
      <td>Q1</td>
      <td>05001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Dance Technique</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>44</th>
      <td>450359.0</td>
      <td>297147</td>
      <td>Welch</td>
      <td>Jerimiah</td>
      <td>6/1/1977</td>
      <td>2019</td>
      <td>15016299025086C</td>
      <td>15016299025086C</td>
      <td>S1</td>
      <td>19001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Human Services Career Exploration</td>
      <td>01/26/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>47</th>
      <td>757959.0</td>
      <td>808714</td>
      <td>Clayton</td>
      <td>Trystan</td>
      <td>7/6/1969</td>
      <td>2019</td>
      <td>15016299025285C</td>
      <td>15016299025285C</td>
      <td>S1</td>
      <td>03051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Biology</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>48</th>
      <td>534955.0</td>
      <td>485042</td>
      <td>Medina</td>
      <td>Chad</td>
      <td>9/5/1980</td>
      <td>2019</td>
      <td>15016299025059C</td>
      <td>15016299025059C</td>
      <td>S1</td>
      <td>02101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Number Theory</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>49</th>
      <td>283239.0</td>
      <td>549042</td>
      <td>Castaneda</td>
      <td>Tripp</td>
      <td>10/14/1973</td>
      <td>2019</td>
      <td>15016299025056C</td>
      <td>15016299025056C</td>
      <td>S1</td>
      <td>12051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Introductory Business</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>51</th>
      <td>279855.0</td>
      <td>umacdonald</td>
      <td>Macdonald</td>
      <td>Ulises</td>
      <td>1/14/1961</td>
      <td>2019</td>
      <td>15016299025072C</td>
      <td>15016299025072C</td>
      <td>S1</td>
      <td>01051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Literature (freshmen and sophomores)</td>
      <td>09/18/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>52</th>
      <td>970753.0</td>
      <td>543637</td>
      <td>Harrell</td>
      <td>Nasir</td>
      <td>3/12/1956</td>
      <td>2019</td>
      <td>15016299025056C</td>
      <td>15016299025056C</td>
      <td>S1</td>
      <td>21051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Technological Literacy</td>
      <td>08/08/2018</td>
      <td>203.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>53</th>
      <td>221298.0</td>
      <td>367012</td>
      <td>Hamilton</td>
      <td>Talan</td>
      <td>9/16/1953</td>
      <td>2019</td>
      <td>15016299025061C</td>
      <td>15016299025061C</td>
      <td>S1</td>
      <td>20151A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Distribution-Comprehensive</td>
      <td>08/05/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>58</th>
      <td>617607.0</td>
      <td>tgould</td>
      <td>Gould</td>
      <td>Tyree</td>
      <td>6/19/1980</td>
      <td>2019</td>
      <td>15016299025066C</td>
      <td>15016299025066C</td>
      <td>S1</td>
      <td>03151A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Physics</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>59</th>
      <td>168100.0</td>
      <td>85266</td>
      <td>Davidson</td>
      <td>Antoine</td>
      <td>8/10/1962</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>02001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Informal Mathematics</td>
      <td>08/20/2018</td>
      <td>204.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>61</th>
      <td>580075.0</td>
      <td>745644</td>
      <td>Flynn</td>
      <td>Cruz</td>
      <td>9/18/1988</td>
      <td>2019</td>
      <td>15016299025086C</td>
      <td>15016299025086C</td>
      <td>S1</td>
      <td>02101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Number Theory</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>62</th>
      <td>592850.0</td>
      <td>786785</td>
      <td>Christian</td>
      <td>Charles</td>
      <td>7/27/1986</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>19101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Cosmetology-Licensing</td>
      <td>02/13/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>63</th>
      <td>450804.0</td>
      <td>596126</td>
      <td>Lawrence</td>
      <td>Carson</td>
      <td>5/28/1969</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>02151A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>General Applied Math</td>
      <td>09/21/2018</td>
      <td>201.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>65</th>
      <td>35418.0</td>
      <td>172316</td>
      <td>Hanna</td>
      <td>Luke</td>
      <td>5/1/1990</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>06101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Spanish I</td>
      <td>08/20/2018</td>
      <td>205.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>66</th>
      <td>135750.0</td>
      <td>101146</td>
      <td>Sherman</td>
      <td>Felipe</td>
      <td>4/22/1951</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>01001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Language Arts I (9th grade)</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>67</th>
      <td>194173.0</td>
      <td>867448</td>
      <td>Pitts</td>
      <td>Gunner</td>
      <td>3/20/1956</td>
      <td>2019</td>
      <td>15016299025061C</td>
      <td>15016299025061C</td>
      <td>S1</td>
      <td>01101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Composition (freshmen and sophomores)</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>68</th>
      <td>963325.0</td>
      <td>898345</td>
      <td>Bridges</td>
      <td>Mateo</td>
      <td>5/11/1989</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>21101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Drafting Careers Exploration</td>
      <td>08/20/2018</td>
      <td>205.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>69</th>
      <td>51249.0</td>
      <td>181104</td>
      <td>Adkins</td>
      <td>Antwan</td>
      <td>2/24/1957</td>
      <td>2019</td>
      <td>15016299025069C</td>
      <td>15016299025069C</td>
      <td>Q1</td>
      <td>01101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Composition (freshmen and sophomores)</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>70</th>
      <td>451108.0</td>
      <td>123895</td>
      <td>Garza</td>
      <td>Rudy</td>
      <td>6/2/1958</td>
      <td>2019</td>
      <td>15016299025058C</td>
      <td>15016299025058C</td>
      <td>S1</td>
      <td>09101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Naval Junior ROTC I</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>71</th>
      <td>792633.0</td>
      <td>67610</td>
      <td>Swanson</td>
      <td>Erick</td>
      <td>12/23/1985</td>
      <td>2019</td>
      <td>15016299025049C</td>
      <td>15016299025049C</td>
      <td>S1</td>
      <td>03051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Biology</td>
      <td>08/05/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>72</th>
      <td>218932.0</td>
      <td>740221</td>
      <td>Cohen</td>
      <td>Gael</td>
      <td>10/2/1984</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>05001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Dance Technique</td>
      <td>08/18/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>75</th>
      <td>977816.0</td>
      <td>mandrade</td>
      <td>Andrade</td>
      <td>Miles</td>
      <td>2/5/1977</td>
      <td>2019</td>
      <td>15016299025259C</td>
      <td>15016299025259C</td>
      <td>S1</td>
      <td>15001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Exploration of Public Service Careers</td>
      <td>09/12/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>76</th>
      <td>895419.0</td>
      <td>177249</td>
      <td>Mendez</td>
      <td>Gerardo</td>
      <td>10/25/1966</td>
      <td>2019</td>
      <td>15016299025284C</td>
      <td>15016299025284C</td>
      <td>S1</td>
      <td>20051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Truck and Bus Driving</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>79</th>
      <td>37583.0</td>
      <td>916908</td>
      <td>Nelson</td>
      <td>Alex</td>
      <td>1/18/1986</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>Q1</td>
      <td>19001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Human Services Career Exploration</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>83</th>
      <td>525917.0</td>
      <td>472069</td>
      <td>Fleming</td>
      <td>Jaylan</td>
      <td>5/19/1967</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>01051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Literature (freshmen and sophomores)</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>84</th>
      <td>759195.0</td>
      <td>fhines</td>
      <td>Hines</td>
      <td>Finley</td>
      <td>11/6/1969</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>22101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Leadership</td>
      <td>02/28/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>85</th>
      <td>169120.0</td>
      <td>660692</td>
      <td>Branch</td>
      <td>Kamren</td>
      <td>9/14/1983</td>
      <td>2019</td>
      <td>15016299025262C</td>
      <td>15016299025262C</td>
      <td>S1</td>
      <td>01101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Composition (freshmen and sophomores)</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>86</th>
      <td>314258.0</td>
      <td>902461</td>
      <td>Thomas</td>
      <td>Felipe</td>
      <td>10/22/1990</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>14101A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Dental Laboratory Technology</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>



### Remove nonnumeric values from TeacherID


```python
df = df[df['LocalTeacherID'].apply(lambda x: str(x).isdigit())]
```


```python
# Confirm values are correct
df['LocalTeacherID'].unique()
```




    array(['804678', '280726', '251432', ..., '46538', '760249', '70447'],
          dtype=object)



### Fix Date format


```python
# Use standard date
df['BirthDate'] = pd.to_datetime(df['BirthDate'])
df['BirthDate'] = df['BirthDate'].dt.strftime('%m/%d/%Y')
df['TeacherCourseStartDate'] = pd.to_datetime(df['TeacherCourseStartDate'])
df['TeacherCourseStartDate'] = df['TeacherCourseStartDate'].dt.strftime('%m/%d/%Y')
```

### Remove values for Term that aren't included in ISBE list


```python
df = df[df['Term'].str.len() >= 2]  
```


```python
# confirm unique values for Term are ISBE appropriate
df['Term'].unique()
```




    array(['S1', 'Y1', 'Q1'], dtype=object)




```python
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
      <th>IEIN</th>
      <th>LocalTeacherID</th>
      <th>LastName</th>
      <th>FirstName</th>
      <th>BirthDate</th>
      <th>SchoolYear</th>
      <th>ServingLocationRCDTS</th>
      <th>EmployerRCDTS</th>
      <th>Term</th>
      <th>StateCourseCode</th>
      <th>LocalCourseID</th>
      <th>LocalCourseTitle</th>
      <th>SectionNumber</th>
      <th>TeacherCourseStartDate</th>
      <th>EISPositionCode</th>
      <th>TeacherCommitment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>374529.0</td>
      <td>804678</td>
      <td>Bernard</td>
      <td>Jorge</td>
      <td>11/05/1990</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>04051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>World History-Overview</td>
      <td>08/20/2018</td>
      <td>201.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>369472.0</td>
      <td>280726</td>
      <td>Rios</td>
      <td>Teagan</td>
      <td>11/20/1971</td>
      <td>2019</td>
      <td>15016299025059C</td>
      <td>15016299025059C</td>
      <td>S1</td>
      <td>21051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Technological Literacy</td>
      <td>01/21/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>509509.0</td>
      <td>251432</td>
      <td>Morris</td>
      <td>Noel</td>
      <td>06/11/1962</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>02051A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Pre-Algebra</td>
      <td>01/13/2018</td>
      <td>200.0</td>
      <td>0.36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>922488.0</td>
      <td>303080</td>
      <td>Fernandez</td>
      <td>Dominique</td>
      <td>01/20/1982</td>
      <td>2019</td>
      <td>15016299025266C</td>
      <td>15016299025266C</td>
      <td>Y1</td>
      <td>01001A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>English/Language Arts I (9th grade)</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>689032.0</td>
      <td>344652</td>
      <td>Garrison</td>
      <td>Benjamin</td>
      <td>09/01/1974</td>
      <td>2019</td>
      <td>15016299025068C</td>
      <td>15016299025068C</td>
      <td>S1</td>
      <td>01151A000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Public Speaking</td>
      <td>08/20/2018</td>
      <td>200.0</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>



### Check EISPosition to make sure they are ISBE appropriate


```python
# Position codes from ISBE form
position_codes = [200,201,202,203,204,207,208,250,
                  251,310,601,602,603,604,605,606,
                  607,608,609,610,611,699]
```


```python
# Unique position codes from TeacherData
p_codes = df['EISPositionCode'].unique()
print(p_codes)
```

    [201. 200. 205. 203. 204. 202.]



```python
# Remove position codes that equal 205
df = df[df['EISPositionCode']!=205]
```


```python
# Confirm position codes
df['EISPositionCode'].unique()
```




    array([201., 200., 203., 204., 202.])



### Evaluate StateCourseCode and make sure compliant with ISBE


```python
df['StateCourseCode'].unique()
```




    array(['04051A000', '21051A000', '02051A000', '01001A000', '01151A000',
           '04001A000', '02133A000', '16001A000', '08001A000', '02101A000',
           '09151A000', '21001A000', '12001A000', '11051A000', '19101A000',
           '09051A000', '16051A000', '05001A000', '19001A000', '03051A000',
           '12051A000', '20151A000', '02001A000', '02151A000', '01101A000',
           '09101A000', '20051A000', '01051A000', '14101A000', '18149A000',
           '03101A000', '05148A000', '15001A000', '15051A000', '02049A000',
           '14151A000', '18101A000', '16101A000', '05101A000', '04151A000',
           '03151A000', '11101A000', '03049A000', '08051A000', '18051A000',
           '10051A000', '03001A000', '04149A000', '04101A000', '01148A000',
           '05051A000', '15151A000', '10151A000', '22101A000', '22051A000',
           '05151A000', '10101A000', '17051A000', '20001A000', '09001A000',
           '22001A000', '08151A000', '15147A000', '02141A000', '17048A000',
           '19051A000', '18001A000', '06101A000', '15101A000', '17001A000',
           '10001A000', '22151A000', '19151A000', '16151A000', '14051A000',
           '21101A000', '19101A001', '12151A000', '13149A000', '06133A000',
           '13101A000', '17101A000', '04048A000', '06146A000', '07001A000',
           '06151A000', '03001A001', '04147A000', '16147A000', '01051Å000',
           '13001A000', '11151A000', '20101A000', '06141A000', '06148A000',
           '17148A000', '02131A000', '12101A000', '14001A000', '12001Å000',
           '03148A000', '01149A000', '04148A000', '16148A000', '04047A000',
           '03149A000', '03048A000', '18148A000', '09101Å000', '03147A000',
           '21048A000', '06145A000', '02132A000', '02134A000', '14147A000'],
          dtype=object)




```python
# Check first two string values to make sure they match ISBE Course Codes
df[(df['StateCourseCode']).str.startswith('04')]['SectionNumber'].unique()

```




    array(['World History-Overview', 'World Geography',
           'U.S. Government-Comprehensive', 'U.S. History-Other',
           'U.S. History-Comprehensive', 'Geography-Workplace Experience',
           'U.S. History-Independent Study',
           'U.S. History-Workplace Experience', 'Geography-Independent Study'],
          dtype=object)



### Add Network Column to dataset


```python
# Rename column for merge function
count_df = count_df.rename(columns={"isbe_rcdts_code": "ServingLocationRCDTS"})
```


```python
# Merge datasets on ServingLocationRCDTS (like Vlookup)
df = pd.merge(df,count_df[['ServingLocationRCDTS','Network']],on='ServingLocationRCDTS', how='left')
```

### Drop Empy Columns


```python
df = df.drop(columns=['LocalCourseID','LocalCourseTitle'])
```

### Export to CSV


```python
df.to_csv('isbe.csv')
```
