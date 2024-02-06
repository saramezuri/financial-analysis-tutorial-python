# Financial Analysis with Python : A Comprehensive Tutorial

### Sara Mezuri

## Introduction

Python holds immense importance in the financial world due to its versatility, efficiency, and extensive ecosystem of libraries. Its readability and ease of integration make it a preferred language for rapidly developing and deploying sophisticated financial models, contributing significantly to the finance industry.
NumPy, Pandas, and Matplotlib play pivotal roles in the field of financial analysis by providing powerful tools for data manipulation, analysis, and visualization. 
NumPy's array-based computations help with numerical operations, and assist in the implementation of complex financial models.
Pandas, because of its DataFrame structure, help simplifying data manipulation and cleaning, and transforming financial datasets.
Matplotlib, as a plotting library, enables the creation of clear and insightful visualizations, essential for interpreting complex financial trends and patterns.

The primary goal of this project is to explore the world of financial analysis using Python, focusing on key libraries such as NumPy, Pandas and Matplotlib. The outcome of this project is to create a tutorial on the process of analyzing financial data and vizualizing the result as well. 

## Background

For this project, my focus is on conducting a comprehensive analysis of the impact of COVID-19 on the European Stock Market. 
To achieve this, I will perform a thorough financial analysis of various stock indexes across Europe, more precisely: 

* EURO STOXX 50 (The index holds stocks from eight eurozone countries: Belgium, Finland, France, Germany, Ireland, Italy, the Netherlands, and Spain.)
* FTSE 100 (The FTSE 100 Index is a index of the 100 most highly capitalized companies traded on the London Stock Exchange.)
* DAX (The DAX is stock market index consisting of the 40 major German companies trading on the Frankfurt Stock Exchange.)
* CAC40 (The CAC 40 is the index of the largest 40 companies listed in France)
* FTSE MIB (The Index consists of the 40 stocks listed on the Milano Borsa Italiana.)
* SMI (The index SMI is the most important stock index in Switzerland and comprises the 20 largest stocks.) 
* IBEX 35 (The IBEX 35 is the official index of the Spanish Continuous Exchange, comprised of the 35 most liquid stocks.)

![A representation of the world stock market](world-markets.jpg)

I plan to collect historical stock data from 2017 to 2022, aiming to recognize patterns and trends. Through a detailed analysis, I aim to extract valuable insights that can shed light on the impact of COVID-19 on the European Stock Market, providing a comprehensive understanding of the market dynamics during this period.

## Outline

* Importing the data
    - installing yfinance library
    - creating a dataframe
* Exploratory Data Analysis
* Data Visualization    
    - Missing Values Visualization
    - Line Charts
    - Candlestick Charts
* Financial Analysis 
    - Daily Returns
    - Average Daily Returns 
    - Volatility

### Import libraries


```python
import pandas as pd
import numpy as np
import seaborn as sns; sns.set()
import matplotlib.pyplot as plt

%matplotlib inline
```

## Importing the data

To import stock data, we will use a Python library called `yfinance`, which provides a convenient way for pulling different financial data from various online sources such as: Yahoo Finance, and putting all these data into a Pandas DataFrame. 

First, we must install `yfinance`. To do so, we type `pip install pandas yfinance` into the command prompt. 

 It should look something like this: 
    
    ` Downloading yfinance-0.2.33-py2.py3-none-any.whl (69 kB)
     |████████████████████████████████| 69 kB 1.4 MB/s
Requirement already satisfied: python-dateutil>=2.7.3 in c:\users\saram\anaconda3\lib\site-packages (from pandas) (2.8.2)
Requirement already satisfied: numpy>=1.17.3 in c:\users\saram\anaconda3\lib\site-packages (from pandas) (1.20.3)
Requirement already satisfied: pytz>=2017.3 in c:\users\saram\anaconda3\lib\site-packages (from pandas) (2021.3)
Collecting frozendict>=2.3.4 `


After sucessfully installing `yfinance`, let's import the libraries. 


```python
import datetime 
import yfinance as yf
```

After installing and importing `yfinance`, we are ready to import the data. 

We start by first specifying the **start** and the **end** dates using python datetime module. 


```python
# Set the start and end dates for your analysis. 
start_date = datetime.datetime(2017, 1, 1)
end_date = datetime.datetime(2022, 12, 31)
print(start_date)
print(end_date)
```

    2017-01-01 00:00:00
    2022-12-31 00:00:00
    

Then we will select the stocks (or tickers) that we want to analyse.  
To find the stocks we are interested in, we go to Yahoo Finance [https://uk.finance.yahoo.com] and search each stock as shown below: 
![](fyahoo.jpg)

The highlighted name is the ticker for each stock index. 

Next, we want to import all these stock index tickers using `yfinance`, then we want to create a data frame containing all of them. 


```python
# First, let's define a list with all the stock tickers
stock_index = ['FTSEMIB.MI', '^GDAXI', '^FTSE', '^FCHI', '^IBEX', '^SSMI', '^STOXX50E']
print(stock_index)

# Then we create an empty data frame to store the data
stock_data = pd.DataFrame() 

# We create a loop that downloads each stock index individually and then we appends it to the dataframe
for stock in stock_index:
    data = yf.download(stock, start = start_date, end = end_date)
    column_names = [f'{stock}_{column}' for column in data.columns]
    data.columns = column_names
    # concatenates the dataframes horizontally based on columns
    stock_data = pd.concat([stock_data, data], axis=1)

# Show the first rows of the dataframe    
print(stock_data.head())

# Show the last rows of the dataframe
print(stock_data.tail())
```

    ['FTSEMIB.MI', '^GDAXI', '^FTSE', '^FCHI', '^IBEX', '^SSMI', '^STOXX50E']
    

    [*********************100%%**********************]  1 of 1 completed
    [*********************100%%**********************]  1 of 1 completed
    [*********************100%%**********************]  1 of 1 completed
    [*********************100%%**********************]  1 of 1 completed
    [*********************100%%**********************]  1 of 1 completed
    [*********************100%%**********************]  1 of 1 completed
    [*********************100%%**********************]  1 of 1 completed

                FTSEMIB.MI_Open  FTSEMIB.MI_High  FTSEMIB.MI_Low  \
    Date                                                           
    2017-01-02          19208.0          19579.0         19201.0   
    2017-01-03          19667.0          19811.0         19564.0   
    2017-01-04          19669.0          19735.0         19504.0   
    2017-01-05          19599.0          19733.0         19562.0   
    2017-01-06          19690.0          19715.0         19517.0   
    
                FTSEMIB.MI_Close  FTSEMIB.MI_Adj Close  FTSEMIB.MI_Volume  \
    Date                                                                    
    2017-01-02           19567.0               19567.0        408103000.0   
    2017-01-03           19573.0               19573.0        756855700.0   
    2017-01-04           19627.0               19627.0        542401500.0   
    2017-01-05           19643.0               19643.0        555689100.0   
    2017-01-06           19688.0               19688.0        461959000.0   
    
                 ^GDAXI_Open   ^GDAXI_High    ^GDAXI_Low  ^GDAXI_Close  ...  \
    Date                                                                ...   
    2017-01-02  11426.379883  11617.280273  11414.820312  11598.330078  ...   
    2017-01-03  11631.700195  11637.370117  11561.230469  11584.240234  ...   
    2017-01-04  11609.530273  11616.089844  11531.429688  11584.309570  ...   
    2017-01-05  11537.730469  11602.540039  11537.400391  11584.940430  ...   
    2017-01-06  11560.519531  11605.740234  11547.049805  11599.009766  ...   
    
                  ^SSMI_Low  ^SSMI_Close  ^SSMI_Adj Close  ^SSMI_Volume  \
    Date                                                                  
    2017-01-02          NaN          NaN              NaN           NaN   
    2017-01-03  8283.599609  8316.179688      8316.179688    62461200.0   
    2017-01-04  8309.070312  8354.809570      8354.809570    67007100.0   
    2017-01-05  8328.910156  8392.490234      8392.490234    52553700.0   
    2017-01-06  8373.000000  8417.459961      8417.459961    40413100.0   
    
                ^STOXX50E_Open  ^STOXX50E_High  ^STOXX50E_Low  ^STOXX50E_Close  \
    Date                                                                         
    2017-01-02             NaN             NaN            NaN              NaN   
    2017-01-03     3316.530029     3334.439941    3310.459961      3315.020020   
    2017-01-04     3316.780029     3325.500000    3302.989990      3317.520020   
    2017-01-05     3306.310059     3323.159912    3300.770020      3316.469971   
    2017-01-06     3311.350098     3322.600098    3299.659912      3321.169922   
    
                ^STOXX50E_Adj Close  ^STOXX50E_Volume  
    Date                                               
    2017-01-02                  NaN               NaN  
    2017-01-03          3315.020020        38886800.0  
    2017-01-04          3317.520020        39204000.0  
    2017-01-05          3316.469971        35162700.0  
    2017-01-06          3321.169922        27877300.0  
    
    [5 rows x 42 columns]
                FTSEMIB.MI_Open  FTSEMIB.MI_High  FTSEMIB.MI_Low  \
    Date                                                           
    2022-12-23          23830.0          23938.0         23773.0   
    2022-12-27          24033.0          24081.0         23808.0   
    2022-12-28          23850.0          23913.0         23721.0   
    2022-12-29          23689.0          24065.0         23645.0   
    2022-12-30          23924.0          23956.0         23702.0   
    
                FTSEMIB.MI_Close  FTSEMIB.MI_Adj Close  FTSEMIB.MI_Volume  \
    Date                                                                    
    2022-12-23           23878.0               23878.0        253043700.0   
    2022-12-27           23856.0               23856.0        216142900.0   
    2022-12-28           23770.0               23770.0        283942900.0   
    2022-12-29           24057.0               24057.0        287294200.0   
    2022-12-30           23707.0               23707.0        252891700.0   
    
                 ^GDAXI_Open   ^GDAXI_High    ^GDAXI_Low  ^GDAXI_Close  ...  \
    Date                                                                ...   
    2022-12-23  13945.589844  14000.679688  13874.500000  13940.929688  ...   
    2022-12-27  14047.419922  14063.139648  13966.349609  13995.099609  ...   
    2022-12-28  14013.719727  14018.469727  13914.620117  13925.599609  ...   
    2022-12-29  13890.809570  14071.719727  13871.320312  14071.719727  ...   
    2022-12-30  14005.839844  14008.969727  13922.549805  13923.589844  ...   
    
                   ^SSMI_Low   ^SSMI_Close  ^SSMI_Adj Close  ^SSMI_Volume  \
    Date                                                                    
    2022-12-23  10761.290039  10804.679688     10804.679688    29382200.0   
    2022-12-27  10817.480469  10839.219727     10839.219727    21239100.0   
    2022-12-28  10803.099609  10812.669922     10812.669922    27529400.0   
    2022-12-29  10737.900391  10857.349609     10857.349609    25220400.0   
    2022-12-30  10729.400391  10729.400391     10729.400391    32554800.0   
    
                ^STOXX50E_Open  ^STOXX50E_High  ^STOXX50E_Low  ^STOXX50E_Close  \
    Date                                                                         
    2022-12-23     3823.699951     3833.610107    3798.739990      3817.010010   
    2022-12-27     3826.989990     3854.610107    3826.989990      3832.889893   
    2022-12-28     3833.449951     3842.219971    3805.939941      3808.820068   
    2022-12-29     3803.239990     3851.389893    3793.250000      3850.070068   
    2022-12-30     3845.919922     3845.919922    3792.090088      3793.620117   
    
                ^STOXX50E_Adj Close  ^STOXX50E_Volume  
    Date                                               
    2022-12-23          3817.010010        14824600.0  
    2022-12-27          3832.889893        12768100.0  
    2022-12-28          3808.820068        13848100.0  
    2022-12-29          3850.070068        17579300.0  
    2022-12-30          3793.620117        16362700.0  
    
    [5 rows x 42 columns]
    

    
    

## Exploratory Data Anlysis
Now let's look at this DataFrame and get an idea of what our data looks like. 

First, let's use `shape` to get a better understanding of our dataframe. 


```python
stock_data.shape
```




    (1540, 42)



Then, we use `info()` method to get a summary of the DataFrame, including information about the data types, non-null values and its structure.


```python
stock_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    DatetimeIndex: 1540 entries, 2017-01-02 to 2022-12-30
    Data columns (total 42 columns):
     #   Column                Non-Null Count  Dtype  
    ---  ------                --------------  -----  
     0   FTSEMIB.MI_Open       1524 non-null   float64
     1   FTSEMIB.MI_High       1524 non-null   float64
     2   FTSEMIB.MI_Low        1524 non-null   float64
     3   FTSEMIB.MI_Close      1524 non-null   float64
     4   FTSEMIB.MI_Adj Close  1524 non-null   float64
     5   FTSEMIB.MI_Volume     1524 non-null   float64
     6   ^GDAXI_Open           1520 non-null   float64
     7   ^GDAXI_High           1520 non-null   float64
     8   ^GDAXI_Low            1520 non-null   float64
     9   ^GDAXI_Close          1520 non-null   float64
     10  ^GDAXI_Adj Close      1520 non-null   float64
     11  ^GDAXI_Volume         1520 non-null   float64
     12  ^FTSE_Open            1514 non-null   float64
     13  ^FTSE_High            1514 non-null   float64
     14  ^FTSE_Low             1514 non-null   float64
     15  ^FTSE_Close           1514 non-null   float64
     16  ^FTSE_Adj Close       1514 non-null   float64
     17  ^FTSE_Volume          1514 non-null   float64
     18  ^FCHI_Open            1537 non-null   float64
     19  ^FCHI_High            1537 non-null   float64
     20  ^FCHI_Low             1537 non-null   float64
     21  ^FCHI_Close           1537 non-null   float64
     22  ^FCHI_Adj Close       1537 non-null   float64
     23  ^FCHI_Volume          1537 non-null   float64
     24  ^IBEX_Open            1534 non-null   float64
     25  ^IBEX_High            1534 non-null   float64
     26  ^IBEX_Low             1534 non-null   float64
     27  ^IBEX_Close           1534 non-null   float64
     28  ^IBEX_Adj Close       1534 non-null   float64
     29  ^IBEX_Volume          1534 non-null   float64
     30  ^SSMI_Open            1498 non-null   float64
     31  ^SSMI_High            1498 non-null   float64
     32  ^SSMI_Low             1498 non-null   float64
     33  ^SSMI_Close           1498 non-null   float64
     34  ^SSMI_Adj Close       1498 non-null   float64
     35  ^SSMI_Volume          1498 non-null   float64
     36  ^STOXX50E_Open        1509 non-null   float64
     37  ^STOXX50E_High        1509 non-null   float64
     38  ^STOXX50E_Low         1509 non-null   float64
     39  ^STOXX50E_Close       1509 non-null   float64
     40  ^STOXX50E_Adj Close   1509 non-null   float64
     41  ^STOXX50E_Volume      1509 non-null   float64
    dtypes: float64(42)
    memory usage: 517.3 KB
    

Here, I can tell that I do not have the same amount of data for all the stock indices, but since it is not a significant difference, it will not give us any issues. However, it might suggest that we might have some missing data. 


```python
# The code below will print the columns of the dataframe

stock_data.columns
```




    Index(['FTSEMIB.MI_Open', 'FTSEMIB.MI_High', 'FTSEMIB.MI_Low',
           'FTSEMIB.MI_Close', 'FTSEMIB.MI_Adj Close', 'FTSEMIB.MI_Volume',
           '^GDAXI_Open', '^GDAXI_High', '^GDAXI_Low', '^GDAXI_Close',
           '^GDAXI_Adj Close', '^GDAXI_Volume', '^FTSE_Open', '^FTSE_High',
           '^FTSE_Low', '^FTSE_Close', '^FTSE_Adj Close', '^FTSE_Volume',
           '^FCHI_Open', '^FCHI_High', '^FCHI_Low', '^FCHI_Close',
           '^FCHI_Adj Close', '^FCHI_Volume', '^IBEX_Open', '^IBEX_High',
           '^IBEX_Low', '^IBEX_Close', '^IBEX_Adj Close', '^IBEX_Volume',
           '^SSMI_Open', '^SSMI_High', '^SSMI_Low', '^SSMI_Close',
           '^SSMI_Adj Close', '^SSMI_Volume', '^STOXX50E_Open', '^STOXX50E_High',
           '^STOXX50E_Low', '^STOXX50E_Close', '^STOXX50E_Adj Close',
           '^STOXX50E_Volume'],
          dtype='object')




```python
# The code below will print the index of the dataframe

stock_data.index
```




    DatetimeIndex(['2017-01-02', '2017-01-03', '2017-01-04', '2017-01-05',
                   '2017-01-06', '2017-01-09', '2017-01-10', '2017-01-11',
                   '2017-01-12', '2017-01-13',
                   ...
                   '2022-12-16', '2022-12-19', '2022-12-20', '2022-12-21',
                   '2022-12-22', '2022-12-23', '2022-12-27', '2022-12-28',
                   '2022-12-29', '2022-12-30'],
                  dtype='datetime64[ns]', name='Date', length=1540, freq=None)



Then, let's use `describe()` method to generate a descriptive statistics of the numerical columns. It It provides information such as mean, standard deviation, minimum, maximum, and quartile values.


```python
stock_data.describe()
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
      <th>FTSEMIB.MI_Open</th>
      <th>FTSEMIB.MI_High</th>
      <th>FTSEMIB.MI_Low</th>
      <th>FTSEMIB.MI_Close</th>
      <th>FTSEMIB.MI_Adj Close</th>
      <th>FTSEMIB.MI_Volume</th>
      <th>^GDAXI_Open</th>
      <th>^GDAXI_High</th>
      <th>^GDAXI_Low</th>
      <th>^GDAXI_Close</th>
      <th>...</th>
      <th>^SSMI_Low</th>
      <th>^SSMI_Close</th>
      <th>^SSMI_Adj Close</th>
      <th>^SSMI_Volume</th>
      <th>^STOXX50E_Open</th>
      <th>^STOXX50E_High</th>
      <th>^STOXX50E_Low</th>
      <th>^STOXX50E_Close</th>
      <th>^STOXX50E_Adj Close</th>
      <th>^STOXX50E_Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1524.000000</td>
      <td>1524.000000</td>
      <td>1524.000000</td>
      <td>1524.000000</td>
      <td>1524.000000</td>
      <td>1.524000e+03</td>
      <td>1520.000000</td>
      <td>1520.000000</td>
      <td>1520.000000</td>
      <td>1520.000000</td>
      <td>...</td>
      <td>1498.000000</td>
      <td>1498.000000</td>
      <td>1498.000000</td>
      <td>1.498000e+03</td>
      <td>1509.000000</td>
      <td>1509.000000</td>
      <td>1509.000000</td>
      <td>1509.000000</td>
      <td>1509.000000</td>
      <td>1.509000e+03</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>22165.286745</td>
      <td>22316.681759</td>
      <td>21996.854331</td>
      <td>22158.506562</td>
      <td>22158.506562</td>
      <td>4.438851e+08</td>
      <td>13048.353456</td>
      <td>13126.502371</td>
      <td>12962.562415</td>
      <td>13046.671414</td>
      <td>...</td>
      <td>10068.975457</td>
      <td>10123.528242</td>
      <td>10123.528242</td>
      <td>5.520845e+07</td>
      <td>3562.367132</td>
      <td>3584.884931</td>
      <td>3538.547337</td>
      <td>3562.256634</td>
      <td>3562.256634</td>
      <td>3.785120e+07</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2401.877479</td>
      <td>2387.254117</td>
      <td>2411.846756</td>
      <td>2405.728795</td>
      <td>2405.728795</td>
      <td>1.672094e+08</td>
      <td>1402.537753</td>
      <td>1398.201068</td>
      <td>1405.530556</td>
      <td>1402.963366</td>
      <td>...</td>
      <td>1194.873390</td>
      <td>1199.596384</td>
      <td>1199.596384</td>
      <td>2.464872e+07</td>
      <td>333.027441</td>
      <td>330.983543</td>
      <td>335.195952</td>
      <td>333.325720</td>
      <td>333.325720</td>
      <td>1.584098e+07</td>
    </tr>
    <tr>
      <th>min</th>
      <td>14986.000000</td>
      <td>15438.000000</td>
      <td>14153.000000</td>
      <td>14894.000000</td>
      <td>14894.000000</td>
      <td>0.000000e+00</td>
      <td>8495.940430</td>
      <td>8668.480469</td>
      <td>8255.650391</td>
      <td>8441.709961</td>
      <td>...</td>
      <td>7650.229980</td>
      <td>8160.790039</td>
      <td>8160.790039</td>
      <td>0.000000e+00</td>
      <td>2388.939941</td>
      <td>2461.570068</td>
      <td>2302.840088</td>
      <td>2385.820068</td>
      <td>2385.820068</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>20549.750000</td>
      <td>20708.750000</td>
      <td>20406.500000</td>
      <td>20560.000000</td>
      <td>20560.000000</td>
      <td>3.428951e+08</td>
      <td>12153.147705</td>
      <td>12245.000244</td>
      <td>12097.270264</td>
      <td>12173.789795</td>
      <td>...</td>
      <td>8995.064941</td>
      <td>9031.487549</td>
      <td>9031.487549</td>
      <td>4.097042e+07</td>
      <td>3359.709961</td>
      <td>3381.629883</td>
      <td>3334.239990</td>
      <td>3361.050049</td>
      <td>3361.050049</td>
      <td>2.910260e+07</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>21995.500000</td>
      <td>22158.000000</td>
      <td>21837.000000</td>
      <td>21984.500000</td>
      <td>21984.500000</td>
      <td>4.097870e+08</td>
      <td>12812.750000</td>
      <td>12882.709961</td>
      <td>12729.360352</td>
      <td>12817.654785</td>
      <td>...</td>
      <td>9944.794922</td>
      <td>9992.759766</td>
      <td>9992.759766</td>
      <td>5.043045e+07</td>
      <td>3512.040039</td>
      <td>3531.530029</td>
      <td>3493.030029</td>
      <td>3514.320068</td>
      <td>3514.320068</td>
      <td>3.486370e+07</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>23850.750000</td>
      <td>23965.250000</td>
      <td>23682.000000</td>
      <td>23830.500000</td>
      <td>23830.500000</td>
      <td>4.958832e+08</td>
      <td>13837.052246</td>
      <td>13933.479980</td>
      <td>13712.452637</td>
      <td>13838.352539</td>
      <td>...</td>
      <td>10894.772217</td>
      <td>10959.402344</td>
      <td>10959.402344</td>
      <td>6.247490e+07</td>
      <td>3738.909912</td>
      <td>3766.020020</td>
      <td>3711.550049</td>
      <td>3736.850098</td>
      <td>3736.850098</td>
      <td>4.267960e+07</td>
    </tr>
    <tr>
      <th>max</th>
      <td>28002.000000</td>
      <td>28213.000000</td>
      <td>27951.000000</td>
      <td>28163.000000</td>
      <td>28163.000000</td>
      <td>2.015242e+09</td>
      <td>16269.219727</td>
      <td>16290.190430</td>
      <td>16240.509766</td>
      <td>16271.750000</td>
      <td>...</td>
      <td>12905.530273</td>
      <td>12970.530273</td>
      <td>12970.530273</td>
      <td>2.657352e+08</td>
      <td>4400.729980</td>
      <td>4415.229980</td>
      <td>4399.250000</td>
      <td>4401.490234</td>
      <td>4401.490234</td>
      <td>1.673299e+08</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 42 columns</p>
</div>



Let's assume that we would like to look say at all the adjusted closing prices for all of these stock indices. 


```python
# We create a new DataFrame containing only the adjusted closing prices
ajd_close_prices_data = stock_data.filter(like='_Adj Close')

print(ajd_close_prices_data.head())
print(ajd_close_prices_data.tail())
```

                FTSEMIB.MI_Adj Close  ^GDAXI_Adj Close  ^FTSE_Adj Close  \
    Date                                                                  
    2017-01-02               19567.0      11598.330078              NaN   
    2017-01-03               19573.0      11584.240234      7177.899902   
    2017-01-04               19627.0      11584.309570      7189.700195   
    2017-01-05               19643.0      11584.940430      7195.299805   
    2017-01-06               19688.0      11599.009766      7210.100098   
    
                ^FCHI_Adj Close  ^IBEX_Adj Close  ^SSMI_Adj Close  \
    Date                                                            
    2017-01-02      4882.379883              NaN              NaN   
    2017-01-03      4899.330078      9494.700195      8316.179688   
    2017-01-04      4899.399902      9462.900391      8354.809570   
    2017-01-05      4900.640137      9488.200195      8392.490234   
    2017-01-06      4909.839844      9515.900391      8417.459961   
    
                ^STOXX50E_Adj Close  
    Date                             
    2017-01-02                  NaN  
    2017-01-03          3315.020020  
    2017-01-04          3317.520020  
    2017-01-05          3316.469971  
    2017-01-06          3321.169922  
                FTSEMIB.MI_Adj Close  ^GDAXI_Adj Close  ^FTSE_Adj Close  \
    Date                                                                  
    2022-12-23               23878.0      13940.929688      7473.000000   
    2022-12-27               23856.0      13995.099609              NaN   
    2022-12-28               23770.0      13925.599609      7497.200195   
    2022-12-29               24057.0      14071.719727      7512.700195   
    2022-12-30               23707.0      13923.589844      7451.700195   
    
                ^FCHI_Adj Close  ^IBEX_Adj Close  ^SSMI_Adj Close  \
    Date                                                            
    2022-12-23      6504.899902      8269.099609     10804.679688   
    2022-12-27      6550.660156      8270.099609     10839.219727   
    2022-12-28      6510.490234      8258.500000     10812.669922   
    2022-12-29      6573.470215      8318.299805     10857.349609   
    2022-12-30      6473.759766      8229.099609     10729.400391   
    
                ^STOXX50E_Adj Close  
    Date                             
    2022-12-23          3817.010010  
    2022-12-27          3832.889893  
    2022-12-28          3808.820068  
    2022-12-29          3850.070068  
    2022-12-30          3793.620117  
    

There is a reason why we need the dataframes created above, and we will see that in a minute.

## Data Visualization

**Data visualization** is really important to comprehend the large datasets, and its significance lies in its ability to make complex information more accessible and understandable through charts and graphs. 

Also visualization helps in identifying patterns, trends, and relationships within the data that might not be immediately apparent in tabular form. It also helps in identifing data quality issues such as missing values, outliers, or inconsistencies. 

Python's data visualization advantages comes from its useful libraries like Matplotlib, Seaborn, and Plotly. These tools work well with other data tools, let you customize your charts, and even create interactive ones.

### Missing Values Visualization

Now, referring to the results we got above, we might have some missing data in our data frame. We can check this by using a Python library called `missingno`. 

Just like we did above, let's install the library first by typing `pip install missingno` in a prompt shell. 


```python
# Import the library 
import missingno as msno

# We plot the missing data
msno.matrix(stock_data, color=(1, 0, 0)) # Red color 

plt.show()
```


    
![png](output_31_0.png)
    


As we can see, we do not have many data missing, so we are good to go. 

If you want to get a better view of the missing data for each stock index, then create a data frame for each of them and then use `missingno` again. For example, let's see what the missing data looks like for the **Milano Italia Borsa (FTSE MIB)**: 


```python
# We create the MIB dataframe
MIB_data = stock_data.filter(like='FTSEMIB.MI')

MIB_data.head()

# Plot the missing data
msno.matrix(MIB_data, color=(0, 0, 1)) # Blue color 

plt.show()
```


    
![png](output_33_0.png)
    

We can repeat this for each stock index. 

### Line Charts

Continuing forward, our next objective is to generate some line charts that show the movement of our data over time. This aims to explore potential trends or shifts in price movements. 

We can use the previously generated data frames containing adjusted closing prices to create individual plot. 


```python
# Using Seaborn, we create a plot for the adjusted closing price
sns.set(style="whitegrid")

# Plotting all adj closing prices with Seaborn
plt.figure(figsize=(10, 6))
sns.lineplot(data=ajd_close_prices_data)
plt.title('Adjusted Closing Prices Over Time')
plt.xlabel('Date')
plt.ylabel('Adjusted Closing Price')
plt.legend(title='Stock Index')
plt.show()

```


    
![png](output_35_0.png)
    


Another way to do this, can be by using `Matplotlib`, as below:


```python
# Plot of closing prices using Matplotlib

plt.figure(figsize=(10, 6))
plt.plot(stock_data['FTSEMIB.MI_Close'], label='FTSEMIB')
plt.plot(stock_data['^FTSE_Close'], label='FTSE100')
plt.plot(stock_data['^GDAXI_Close'], label='DAX')
plt.plot(stock_data['^FCHI_Close'], label='CAC40')
plt.plot(stock_data['^IBEX_Close'], label='IBEX35')
plt.plot(stock_data['^SSMI_Close'], label='SSMI')
plt.plot(stock_data['^STOXX50E_Close'], label='EURO STOXX 50')
plt.title('Closing Stock Prices Over Time')
plt.xlabel('Date')
plt.ylabel('Closing Price')
plt.legend()
plt.show()

```


    
![png](output_37_0.png)
    


These line charts are quite effective for identifying trends or irregularities.

For example, we observe a substantial decline in prices across all stock indices during the March 2020 timeframe. This corresponds to the period when the COVID-19 pandemic had a significant impact on the global economy, leading to a widespread sell-off of stocks.

Furthermore, it is evident from the chart that the Milano Italia Borsa index experienced a more significant decrease. Considering the context of the COVID-19 crisis in Italy during that period, it aligns with the observation that the Italian economy was among the most affected in Europe. 

The attached graph illustrates the peak in daily deaths around March-April 2020 in Italy, providing additional context to the situation. 

![Daily Deaths in Italy](covid_italy.jpg)

The data source for this information is available on the linked website.

https://www.worldometers.info/coronavirus/country/italy/


### Candlestick Charts

Another visualization tool is the candlestick chart.

Candlestick charts are a type of financial chart used to represent the price movements of stocks, over a specific time period. They provide a visual representation of price fluctuations and are really useful in identifying trends, and potential future price movements. Each "candlestick" on the chart represents the open, close, high, and low prices for a given time interval, typically a day.

The candlestick has two main components:

Body: The rectangular area between the open and close prices. If the close price is higher than the open price, the body is often filled or colored. If the close price is lower, the body may be empty or a different color.

Wicks or Shadows: The thin lines, called wicks or shadows, extend from the top and bottom of the body and represent the high and low prices during the time period.

![Candlestick Chart](candlestick_chart.jpg)

Candlestick charts are so popular in financial analysis because they provide a comprehensive and visually way to interpret price action in financial markets. There are different patterns that candlestick charts display, which help in analysing the market trends, but I will not talk about them in details. 

The picture above can be found at https://www.investopedia.com/trading/candlestick-charting-what-is-it/

Now, let's create candlestick charts for our stock indices. First we need to make sure we have the `plotly` library installed. If not, ` pip install plotly` will do the job. 


```python
# Import plotly library as shown
import plotly.graph_objects as go

# Create candlestick chart for Milano Italia Borsa
fig = go.Figure(data=[go.Candlestick(x=stock_data.index,
                open=stock_data['FTSEMIB.MI_Open'],
                high=stock_data['FTSEMIB.MI_High'],
                low=stock_data['FTSEMIB.MI_Low'],
                close=stock_data['FTSEMIB.MI_Close'])])

fig.update_layout(title='Candlestick Chart of Milano Italia Borsa Prices',
                  xaxis_title='Date',
                  yaxis_title='Price')

fig.show()

```

When the closing price surpasses the opening price, the candlestick is depicted in green. Otherwise, if the price declines throughout the day, the candlestick appears in red. A brief observation of the candlestick lengths reveals that 2020 marked a period when prices consistently declined.

If we want to take a look closely at a specific time frame, we can use `.loc()` to filter the index (which in our case is the date) and then repeat the steps above.  


```python
s_date = '2020-03-01'
e_date = '2020-03-31'

# Filter DataFrame for March 2020
march_data = stock_data.loc[(stock_data.index >= s_date) & (stock_data.index <= e_date)]

# Create candlestick chart for March 2020
fig = go.Figure(data=[go.Candlestick(x=march_data.index,
                open=march_data['FTSEMIB.MI_Open'],
                high=march_data['FTSEMIB.MI_High'],
                low=march_data['FTSEMIB.MI_Low'],
                close=march_data['FTSEMIB.MI_Close'])])

fig.update_layout(title='Candlestick Chart of Milano Italia Borsa Prices - March 2020',
                  xaxis_title='Date',
                  yaxis_title='Price')

fig.show()

```


```python
# List all my stocks
stock_index = ['FTSEMIB.MI', '^GDAXI', '^FTSE', '^FCHI', '^IBEX', '^SSMI', '^STOXX50E']

# Set the start and end date
s_date = '2020-03-01'
e_date = '2020-03-31'

# Filter DataFrame for March 2020
march_data = stock_data.loc[(stock_data.index >= s_date) & (stock_data.index <= e_date)]

# Plot candlestick charts for each stock
fig = go.Figure()

for stock in stock_index:
    fig.add_trace(go.Candlestick(x=march_data.index,
                    open=stock_data[f'{stock}_Open'],
                    high=stock_data[f'{stock}_High'],
                    low=stock_data[f'{stock}_Low'],
                    close=stock_data[f'{stock}_Close'],
                    name=stock))

fig.update_layout(title='Candlestick Charts for Selected Stocks',
                  xaxis_title='Date',
                  yaxis_title='Price')

fig.show()
```

## Financial Analysis 

### Return Analysis

Return analysis of the stock market includes the evaluation and examination of the performance of an investment by assessing the returns it generates over a specific period. 

This analysis includes calculating absolute and annualized returns, comparing performance against indices, assessing risk-adjusted returns through ratios like the Sharpe and Treynor ratios, and considering factors like dividend yield. 

Additionally, return analysis provides insights into the consistency of the performance of the market in general, and by analyzing returns, we try to get a better idea of the success of the investments. 

Now, let's calculate the return of our stock indices for each closing price. For this, we can use `.pct_change()` function is a method provided by pandas, a popular data manipulation library in Python. This function is specifically used with time series data, such as stock prices or other sequential data, and it calculates the percentage change between the current and a prior element in the series.

```python
# List my stock index
stock_index = ['FTSEMIB.MI', '^GDAXI', '^FTSE', '^FCHI', '^IBEX', '^SSMI', '^STOXX50E']

# Extract the closing prices for all stocks
closing_prices = stock_data[[f'{stock}_Adj Close' for stock in stock_index]]

# Calculate daily returns for all stocks
returns_data = closing_prices.pct_change()

# Print the first and last rows of the returns
print(returns_data.head())
print(returns_data.tail())
```

                FTSEMIB.MI_Adj Close  ^GDAXI_Adj Close  ^FTSE_Adj Close  \
    Date                                                                  
    2017-01-02                   NaN               NaN              NaN   
    2017-01-03              0.000307         -0.001215              NaN   
    2017-01-04              0.002759          0.000006         0.001644   
    2017-01-05              0.000815          0.000054         0.000779   
    2017-01-06              0.002291          0.001214         0.002057   
    
                ^FCHI_Adj Close  ^IBEX_Adj Close  ^SSMI_Adj Close  \
    Date                                                            
    2017-01-02              NaN              NaN              NaN   
    2017-01-03         0.003472              NaN              NaN   
    2017-01-04         0.000014        -0.003349         0.004645   
    2017-01-05         0.000253         0.002674         0.004510   
    2017-01-06         0.001877         0.002919         0.002975   
    
                ^STOXX50E_Adj Close  
    Date                             
    2017-01-02                  NaN  
    2017-01-03                  NaN  
    2017-01-04             0.000754  
    2017-01-05            -0.000317  
    2017-01-06             0.001417  
                FTSEMIB.MI_Adj Close  ^GDAXI_Adj Close  ^FTSE_Adj Close  \
    Date                                                                  
    2022-12-23              0.002730          0.001930         0.000495   
    2022-12-27             -0.000921          0.003886         0.000000   
    2022-12-28             -0.003605         -0.004966         0.003238   
    2022-12-29              0.012074          0.010493         0.002067   
    2022-12-30             -0.014549         -0.010527        -0.008120   
    
                ^FCHI_Adj Close  ^IBEX_Adj Close  ^SSMI_Adj Close  \
    Date                                                            
    2022-12-23        -0.002005        -0.000363         0.002788   
    2022-12-27         0.007035         0.000121         0.003197   
    2022-12-28        -0.006132        -0.001403        -0.002449   
    2022-12-29         0.009674         0.007241         0.004132   
    2022-12-30        -0.015169        -0.010723        -0.011785   
    
                ^STOXX50E_Adj Close  
    Date                             
    2022-12-23            -0.001643  
    2022-12-27             0.004160  
    2022-12-28            -0.006280  
    2022-12-29             0.010830  
    2022-12-30            -0.014662  
    


```python
# To see the return values for the first week of March 2020 we filter the data as given below

# List all my stocks
stock_index = ['FTSEMIB.MI', '^GDAXI', '^FTSE', '^FCHI', '^IBEX', '^SSMI', '^STOXX50E']

# Set the start and end date
s_date = '2020-03-01'
e_date = '2020-03-8'

# Filter DataFrame for the first week of March 2020
first_week_data = stock_data.loc[(stock_data.index >= s_date) & (stock_data.index <= e_date)]

# Extract the closing prices for all stocks
closing_prices_fweek = first_week_data[[f'{stock}_Adj Close' for stock in stock_index]]

# Calculate daily returns for all stocks
returns_data_fweek = closing_prices_fweek.pct_change()

# Print the returns
print(returns_data_fweek.head(5))
```

                FTSEMIB.MI_Adj Close  ^GDAXI_Adj Close  ^FTSE_Adj Close  \
    Date                                                                  
    2020-03-02                   NaN               NaN              NaN   
    2020-03-03              0.004295          0.010754         0.009512   
    2020-03-04              0.009104          0.011873         0.014498   
    2020-03-05             -0.017816         -0.015087        -0.016169   
    2020-03-06             -0.035027         -0.033726        -0.036210   
    
                ^FCHI_Adj Close  ^IBEX_Adj Close  ^SSMI_Adj Close  \
    Date                                                            
    2020-03-02              NaN              NaN              NaN   
    2020-03-03         0.011184         0.008019         0.013725   
    2020-03-04         0.013298         0.011167         0.016257   
    2020-03-05        -0.018992        -0.025477        -0.010518   
    2020-03-06        -0.041408        -0.035403        -0.040099   
    
                ^STOXX50E_Adj Close  
    Date                             
    2020-03-02                  NaN  
    2020-03-03             0.009926  
    2020-03-04             0.014410  
    2020-03-05            -0.016658  
    2020-03-06            -0.039098  
    


```python
returns_data_fweek.shape
```




    (5, 7)



By using the data frame, we can see how much we gain or loss the first week of March 2020. The positive values represent gains and the negative values represent losses. 

For example, FTSE MIB gained 0.43 % on March 3rd and 0.91% on March 4th, but lost 1.78% on the 5th and 3.502% on the 6th, resulting in a loss overall. 

To get a better understanding, we can use Average Daily Returns.The average returns is the expected value of an investment’s change over time. To calculate the average daily return, we use the formula: 

**ADR = Sum of Daily Returns / Number of Trading Days**.

We can calculate the ADR in Python simply by using the `.mean()` function. 


```python
# Calculate the ADR
returns_data.mean()
```




    FTSEMIB.MI_Adj Close    0.000219
    ^GDAXI_Adj Close        0.000197
    ^FTSE_Adj Close         0.000079
    ^FCHI_Adj Close         0.000257
    ^IBEX_Adj Close        -0.000017
    ^SSMI_Adj Close         0.000210
    ^STOXX50E_Adj Close     0.000162
    dtype: float64



The above results are the average daily returns over 5 years. In other words, it seems that the value of the stock market in Italy has increased by 0.022% each day.
While, IBEX, the stock index for the Spanish market seem to have decreased by 0.0017% daily, indicating that the Spanish economy was struggling even before Covid, and Covid impacted it even more.

Another way to analyse our data, would be by calculating the **Best** and the **Worst** day returns. For this, we simply need to find what is the minimum and the maximum return for each stock index. 


```python
# Find the minimum and maximum returns for each stock and their corresponding dates
min_returns_dates = returns_data.idxmin(axis=0)
max_returns_dates = returns_data.idxmax(axis=0)
min_return = returns_data.min(axis=0)
max_return = returns_data.max(axis=0)

# Create a new DataFrame to store the results
result_df = pd.DataFrame({
    'Stock': stock_index,
    'Worst Day Return': min_returns_dates,
    'Minimum Value': min_return,
    'Best Day Return': max_returns_dates,
    'Maximum Value': max_return
})

# Print the resulting DataFrame
print(result_df)
```

                               Stock Worst Day Return  Minimum Value  \
    FTSEMIB.MI_Adj Close  FTSEMIB.MI       2020-03-12      -0.169279   
    ^GDAXI_Adj Close          ^GDAXI       2020-03-12      -0.122386   
    ^FTSE_Adj Close            ^FTSE       2020-03-12      -0.108738   
    ^FCHI_Adj Close            ^FCHI       2020-03-12      -0.122768   
    ^IBEX_Adj Close            ^IBEX       2020-03-12      -0.140592   
    ^SSMI_Adj Close            ^SSMI       2020-03-12      -0.096374   
    ^STOXX50E_Adj Close    ^STOXX50E       2020-03-12      -0.124014   
    
                         Best Day Return  Maximum Value  
    FTSEMIB.MI_Adj Close      2020-03-24       0.089267  
    ^GDAXI_Adj Close          2020-03-24       0.109759  
    ^FTSE_Adj Close           2020-03-24       0.090530  
    ^FCHI_Adj Close           2020-03-24       0.083895  
    ^IBEX_Adj Close           2020-11-09       0.085730  
    ^SSMI_Adj Close           2020-03-24       0.070156  
    ^STOXX50E_Adj Close       2020-03-24       0.092362  
    

It is not a surprise that the Worst Day Returns are during March 2020 (peak of pandemic). 


```python
# Plot returns
fig = go.Figure()

for stock in stock_index:
    fig.add_trace(go.Scatter(x=returns_data.index, y=returns_data[f'{stock}_Adj Close'],
                             mode='lines', name=stock))

fig.update_layout(title='Daily Returns Analysis',
                  xaxis_title='Date',
                  yaxis_title='Daily Returns')

fig.show()
```


The plot of the returns seem to be quite messy. So let's create some subplots. To do so, Plotly's `make_subplots` function comes in handy.

The code below creates a subplot for each stock, arranging them vertically. The `make_subplots` function is used to create a subplot grid, and each stock's returns are plotted in a separate subplot. 


```python
from plotly.subplots import make_subplots

# Create subplots
fig = make_subplots(rows=len(stock_index), cols=1, shared_xaxes=True, subplot_titles=stock_index)

# Plot returns in separate subplots
for i, stock in enumerate(stock_index, start=1):
    fig.add_trace(go.Scatter(x=returns_data.index, y=returns_data[f'{stock}_Adj Close'],
                             mode='lines', name=stock),
                  row=i, col=1)

# Update layout
fig.update_layout(title='Daily Returns Analysis',
                  xaxis_title='Date',
                  showlegend=False)

# Update subplot titles
for i, stock in enumerate(stock_index, start=1):
    fig.update_yaxes(title_text=f'{stock} Returns', row=i, col=1)

fig.show()
```


We can also plot the distribution plot of each stock index as shown below: 


```python
# Calculate daily returns
returns_data_1 = stock_data['FTSEMIB.MI_Adj Close'].pct_change().dropna()

# Plot distribution plot for returns
plt.figure(figsize=(10, 6))
sns.histplot(returns_data_1, kde=True, color='skyblue', bins=30)
plt.title(f'Distribution of Daily Returns for FTSEMIB.MI_Adj Close')
plt.xlabel('Daily Returns')
plt.ylabel('Frequency')
plt.show()

```

    
![png](output_58_0.png)
    

From these charts, it is evident that the lowest returns occurred in March 2020, demonstrating again substantial impact of the pandemic on the market. Once again, the data highlights the profound influence of global events on financial markets, with March 2020 reflecting a period of particularly diminished returns. 

And lastly, let's look at the **Volatility** of these stock index.

Volatility refers to the degree of variation or fluctuation in the price of a stock. It is a statistical measure that quantifies the dispersion of returns for a given security or market index.

High volatility implies that the price of the asset can change dramatically in a short period, while low volatility suggests more stable and gradual price movements. We are interested in the **Historical Volatility**, which measures how much the price of an asset has deviated from its average price over a specific period in the past. Historical volatility is often calculated as the standard deviation of the daily returns.


```python
# Calculate volatility (standard deviation of returns)
volatility = returns_data.std()
volatility
```




    FTSEMIB.MI_Adj Close    0.013609
    ^GDAXI_Adj Close        0.012492
    ^FTSE_Adj Close         0.010403
    ^FCHI_Adj Close         0.012093
    ^IBEX_Adj Close         0.012275
    ^SSMI_Adj Close         0.009419
    ^STOXX50E_Adj Close     0.012186
    dtype: float64



It seems like the SSMI has the lowest standard deviation, suggesting that the Swiss market might have been the most stable and safe market. 

## Conclusions

In conclusion, this comprehensive tutorial has provided a foundational understanding of a simple financial analysis using Python, offering a step-by-step guide to leveraging powerful libraries such as yfinance, pandas, and Plotly. 

We started by fetching historical stock data for a diverse set of European indices, examining the impact of major events like the COVID-19 pandemic on stock markets. 

We explored key financial metrics, including stock prices, returns, and daily volatility, to uncover trends and anomalies. 

The tutorial also demonstrated how to create insightful visualizations, such as candlestick charts and line plots, to enhance data interpretation. 

Additionally, we conducted a return analysis, calculated average daily returns and volatility. By employing practical examples and hands-on coding exercises, this tutorial equips you with the essential tools and skills to conduct a simple financial analysis, helping you to get some information regarding different investments.
