<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap">
</head>

<body>

<div style="background-color: #65BF6F; color: black; text-align: center; padding: 10px; font-family: 'Roboto', sans-serif; font-size: 24px">
    

<div style="text-align:center;">
    <img src="Jaki_Charrua_gatodelespacio.jpg" alt="Drawing" style="width:150px; border-radius:90%;"/>
    
    
</div> 


<body>


    
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://use.typekit.net/your-typekit-id.css">
</head>
<body>
    
    
<div style="background-color: #000000; color: white; text-align: center; padding: 10px; font-family: 'Proxima Nova', sans-serif; font-size: 24px;">
    Replication Lab  <br>
    GEC0319 | Time Series <br>
    
    
</div>



    
    
</body>
</html>

## **Trends and Random Walks in Macroeconomic Time Series: Some Evidence and Implications** 
- Author(s): **Charles R Nelson,  Charles I Plosser**  
- First Published: **1982**
- Link: [here](https://drive.google.com/file/d/107fS9qPNKZd0OhEciAUfAI1VWbvr2PLl/view?usp=sharing)


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.stattools import acf, kpss
from statsmodels.formula.api import ols
```


```python
datos = pd.read_stata(r'C:\Users\juani\Documents\3_My_Jupiter_Notebooks\4_Series_de_Tiempo\0_2024\stata_data\NelsonPlosserData.dta')
datos.set_index('year',inplace=True)
datos.index = datos.index.year

```


```python
titulos = [f'r{i}' for i in range(1,7)] # para rotular unas tablas abajo
```


```python
variables = {'lrgnp':'Real GNP',
           'lgnp':'Nominal GNP',
           'lpcrgnp':'Real per capita GNP',
           'lip':'Industrial production',
           'lemp':'Employment', 
           'lun':'Unemployment rate',
           'lprgnp':'GNP deflator',
           'lcpi':'Consumer prices',
           'lwg':'Wages',
           'lrwg':'Real wages', 
           'lm':'Money stock', 
           'lvel':'Velocity',
           'bnd':'Bond yield',
           'lsp500':'Common stock prices'}
```


```python
datos = datos[variables.keys()].rename(columns=variables)
datos.plot(subplots=True, figsize=[15,15], layout=[-1,3]);

```


    
![png](output_6_0.png)
    



```python
def acf6lags(nombre_variable, d):
    """
    Calcula los primeros 6 coeficientes de autocorrelación
    
    Args:
        nombre_variable: str, el nombre de una columna de la tabla "datos"
        d: número de veces que hay que diferenciar la serie (0 o 1 en la práctica)

    Returns:
        np.array, los 6 coeficientes de autocorrelación, redondeados a dos decimales
    """
    serie = datos[nombre_variable].diff(d) if d>0 else datos[nombre_variable]
    return acf(serie.dropna(), nlags=6, fft=False)[1:].round(2)
```


```python
def tabla_acf(d):
    """
    Crea una tabla con las primeras 6 autocorrelaciones de todas las series en "datos"

    Args:
        d:  int, número de veces que hay que diferenciar la serie (0 o 1 en la práctica)

    Returns:
        una tabla de pandas, series en las filas, número de rezagos en las columnas
    """
    return pd.DataFrame([acf6lags(serie,d) for serie in datos], index=variables.values(), columns=titulos)
```

### Serie en nivel


```python
tabla2 = tabla_acf(0)
tabla2
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
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Real GNP</th>
      <td>0.95</td>
      <td>0.90</td>
      <td>0.84</td>
      <td>0.79</td>
      <td>0.74</td>
      <td>0.69</td>
    </tr>
    <tr>
      <th>Nominal GNP</th>
      <td>0.95</td>
      <td>0.89</td>
      <td>0.83</td>
      <td>0.77</td>
      <td>0.72</td>
      <td>0.67</td>
    </tr>
    <tr>
      <th>Real per capita GNP</th>
      <td>0.95</td>
      <td>0.88</td>
      <td>0.81</td>
      <td>0.75</td>
      <td>0.70</td>
      <td>0.65</td>
    </tr>
    <tr>
      <th>Industrial production</th>
      <td>0.97</td>
      <td>0.94</td>
      <td>0.90</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.81</td>
    </tr>
    <tr>
      <th>Employment</th>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.81</td>
      <td>0.76</td>
      <td>0.71</td>
    </tr>
    <tr>
      <th>Unemployment rate</th>
      <td>0.75</td>
      <td>0.47</td>
      <td>0.32</td>
      <td>0.17</td>
      <td>0.04</td>
      <td>-0.01</td>
    </tr>
    <tr>
      <th>GNP deflator</th>
      <td>0.96</td>
      <td>0.93</td>
      <td>0.89</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.76</td>
    </tr>
    <tr>
      <th>Consumer prices</th>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.77</td>
    </tr>
    <tr>
      <th>Wages</th>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.82</td>
      <td>0.77</td>
      <td>0.73</td>
    </tr>
    <tr>
      <th>Real wages</th>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.75</td>
    </tr>
    <tr>
      <th>Money stock</th>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.89</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.77</td>
    </tr>
    <tr>
      <th>Velocity</th>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.79</td>
    </tr>
    <tr>
      <th>Bond yield</th>
      <td>0.84</td>
      <td>0.72</td>
      <td>0.60</td>
      <td>0.52</td>
      <td>0.46</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>Common stock prices</th>
      <td>0.96</td>
      <td>0.90</td>
      <td>0.85</td>
      <td>0.79</td>
      <td>0.75</td>
      <td>0.71</td>
    </tr>
  </tbody>
</table>
</div>




```python
def rango_datos(ser):
    """
    Determina el rango de los datos disponibles de una serie
    Args:
        ser: una serie de pandas

    Returns:
        str, primera observación -- última observación
    """
    return f'{ser.first_valid_index()} -- {ser.last_valid_index()}'

```


```python
datos.apply(rango_datos)
```




    Real GNP                 1909 -- 1970
    Nominal GNP              1909 -- 1970
    Real per capita GNP      1909 -- 1970
    Industrial production    1860 -- 1970
    Employment               1890 -- 1970
    Unemployment rate        1890 -- 1970
    GNP deflator             1889 -- 1970
    Consumer prices          1860 -- 1970
    Wages                    1900 -- 1970
    Real wages               1900 -- 1970
    Money stock              1889 -- 1970
    Velocity                 1869 -- 1970
    Bond yield               1900 -- 1970
    Common stock prices      1871 -- 1970
    dtype: object




```python
tabla2.insert(0,'Period', datos.apply(rango_datos))
tabla2.insert(1, 'T', datos.apply(lambda ser: ser.dropna().count()))
tabla2
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
      <th>Period</th>
      <th>T</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Real GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.90</td>
      <td>0.84</td>
      <td>0.79</td>
      <td>0.74</td>
      <td>0.69</td>
    </tr>
    <tr>
      <th>Nominal GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.89</td>
      <td>0.83</td>
      <td>0.77</td>
      <td>0.72</td>
      <td>0.67</td>
    </tr>
    <tr>
      <th>Real per capita GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.88</td>
      <td>0.81</td>
      <td>0.75</td>
      <td>0.70</td>
      <td>0.65</td>
    </tr>
    <tr>
      <th>Industrial production</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.97</td>
      <td>0.94</td>
      <td>0.90</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.81</td>
    </tr>
    <tr>
      <th>Employment</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.81</td>
      <td>0.76</td>
      <td>0.71</td>
    </tr>
    <tr>
      <th>Unemployment rate</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.75</td>
      <td>0.47</td>
      <td>0.32</td>
      <td>0.17</td>
      <td>0.04</td>
      <td>-0.01</td>
    </tr>
    <tr>
      <th>GNP deflator</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.96</td>
      <td>0.93</td>
      <td>0.89</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.76</td>
    </tr>
    <tr>
      <th>Consumer prices</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.77</td>
    </tr>
    <tr>
      <th>Wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.82</td>
      <td>0.77</td>
      <td>0.73</td>
    </tr>
    <tr>
      <th>Real wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.75</td>
    </tr>
    <tr>
      <th>Money stock</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.89</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.77</td>
    </tr>
    <tr>
      <th>Velocity</th>
      <td>1869 -- 1970</td>
      <td>102</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.79</td>
    </tr>
    <tr>
      <th>Bond yield</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.84</td>
      <td>0.72</td>
      <td>0.60</td>
      <td>0.52</td>
      <td>0.46</td>
      <td>0.40</td>
    </tr>
    <tr>
      <th>Common stock prices</th>
      <td>1871 -- 1970</td>
      <td>100</td>
      <td>0.96</td>
      <td>0.90</td>
      <td>0.85</td>
      <td>0.79</td>
      <td>0.75</td>
      <td>0.71</td>
    </tr>
  </tbody>
</table>
</div>



### Serie en primera diferencia

Nelson Plosser 1982 tabla 3



```python
tabla3 = tabla_acf(1)
tabla3.insert(0,'Period', datos.apply(rango_datos))
tabla3.insert(1, 'T', datos.apply(lambda ser: ser.dropna().count()))
tabla3
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
      <th>Period</th>
      <th>T</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Real GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.34</td>
      <td>0.04</td>
      <td>-0.18</td>
      <td>-0.23</td>
      <td>-0.19</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Nominal GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.44</td>
      <td>0.08</td>
      <td>-0.12</td>
      <td>-0.24</td>
      <td>-0.07</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>Real per capita GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.33</td>
      <td>0.04</td>
      <td>-0.17</td>
      <td>-0.21</td>
      <td>-0.18</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>Industrial production</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.03</td>
      <td>-0.11</td>
      <td>-0.00</td>
      <td>-0.11</td>
      <td>-0.28</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>Employment</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.32</td>
      <td>-0.05</td>
      <td>-0.08</td>
      <td>-0.17</td>
      <td>-0.20</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>Unemployment rate</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.09</td>
      <td>-0.29</td>
      <td>0.03</td>
      <td>-0.03</td>
      <td>-0.19</td>
      <td>0.01</td>
    </tr>
    <tr>
      <th>GNP deflator</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.43</td>
      <td>0.20</td>
      <td>0.07</td>
      <td>-0.06</td>
      <td>0.03</td>
      <td>0.02</td>
    </tr>
    <tr>
      <th>Consumer prices</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.58</td>
      <td>0.16</td>
      <td>0.02</td>
      <td>-0.00</td>
      <td>0.05</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>Wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.46</td>
      <td>0.10</td>
      <td>-0.03</td>
      <td>-0.09</td>
      <td>-0.09</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>Real wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.19</td>
      <td>-0.03</td>
      <td>-0.07</td>
      <td>-0.11</td>
      <td>-0.18</td>
      <td>-0.15</td>
    </tr>
    <tr>
      <th>Money stock</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.62</td>
      <td>0.30</td>
      <td>0.13</td>
      <td>-0.01</td>
      <td>-0.07</td>
      <td>-0.04</td>
    </tr>
    <tr>
      <th>Velocity</th>
      <td>1869 -- 1970</td>
      <td>102</td>
      <td>0.11</td>
      <td>-0.04</td>
      <td>-0.16</td>
      <td>-0.15</td>
      <td>-0.11</td>
      <td>0.11</td>
    </tr>
    <tr>
      <th>Bond yield</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.18</td>
      <td>0.31</td>
      <td>0.15</td>
      <td>0.04</td>
      <td>0.06</td>
      <td>0.05</td>
    </tr>
    <tr>
      <th>Common stock prices</th>
      <td>1871 -- 1970</td>
      <td>100</td>
      <td>0.22</td>
      <td>-0.13</td>
      <td>-0.08</td>
      <td>-0.18</td>
      <td>-0.23</td>
      <td>0.02</td>
    </tr>
  </tbody>
</table>
</div>



### Desviaciones respecto a una tendencia lineal

![Nelson Plosser 1982 tabla 4](../imgs/NelsonPlosser-table4.png)
* Fuente: Nelson y Plosser 1982 Trends and Random Walks in Macroeconomic Time Series: Some evidence and implications. Journal of Monetary Economics 10, pp.139-162


```python
def acf_deviation_from_trend(nombre_variable):
    """
    Calcular las primeras 6 autocorrelaciones de la desviación de una serie respecto a 
    su tendencia lineal.
    
    Se estima por mínimos cuadrados ordinarios una regresión de la forma
        y = intercepto + time
    
    y se calcula la autocorrelación de los residuos.
    Args:
        nombre_variable: str, nombre de una variable de la tabla "datos" 

    Returns:
        np.array, los 6 coeficientes de autocorrelación, redondeados a dos decimales
    """
    temp = datos[[nombre_variable]].dropna()
    temp.columns = ['y']
    temp['t'] = np.arange(temp.shape[0]) 
    resid = ols('y ~ t', temp).fit().resid
    return acf(resid, nlags=6, fft=False)[1:].round(2)
```


```python
tabla4 = pd.DataFrame([acf_deviation_from_trend(ser) for ser in datos], index=variables.values(), columns=titulos)
tabla4.insert(0,'Period', datos.apply(rango_datos))
tabla4.insert(1, 'T', datos.apply(lambda ser: ser.dropna().count()))
tabla4
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
      <th>Period</th>
      <th>T</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Real GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.87</td>
      <td>0.66</td>
      <td>0.44</td>
      <td>0.26</td>
      <td>0.13</td>
      <td>0.07</td>
    </tr>
    <tr>
      <th>Nominal GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.93</td>
      <td>0.79</td>
      <td>0.65</td>
      <td>0.52</td>
      <td>0.43</td>
      <td>0.35</td>
    </tr>
    <tr>
      <th>Real per capita GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.87</td>
      <td>0.65</td>
      <td>0.43</td>
      <td>0.24</td>
      <td>0.11</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>Industrial production</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.84</td>
      <td>0.67</td>
      <td>0.53</td>
      <td>0.40</td>
      <td>0.29</td>
      <td>0.28</td>
    </tr>
    <tr>
      <th>Employment</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.89</td>
      <td>0.71</td>
      <td>0.55</td>
      <td>0.39</td>
      <td>0.25</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>Unemployment rate</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.75</td>
      <td>0.46</td>
      <td>0.30</td>
      <td>0.15</td>
      <td>0.03</td>
      <td>-0.01</td>
    </tr>
    <tr>
      <th>GNP deflator</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.92</td>
      <td>0.81</td>
      <td>0.67</td>
      <td>0.54</td>
      <td>0.42</td>
      <td>0.30</td>
    </tr>
    <tr>
      <th>Consumer prices</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.97</td>
      <td>0.91</td>
      <td>0.84</td>
      <td>0.78</td>
      <td>0.71</td>
      <td>0.63</td>
    </tr>
    <tr>
      <th>Wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.93</td>
      <td>0.81</td>
      <td>0.67</td>
      <td>0.54</td>
      <td>0.42</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>Real wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.87</td>
      <td>0.69</td>
      <td>0.52</td>
      <td>0.38</td>
      <td>0.26</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>Money stock</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.95</td>
      <td>0.83</td>
      <td>0.69</td>
      <td>0.53</td>
      <td>0.37</td>
      <td>0.21</td>
    </tr>
    <tr>
      <th>Velocity</th>
      <td>1869 -- 1970</td>
      <td>102</td>
      <td>0.91</td>
      <td>0.81</td>
      <td>0.72</td>
      <td>0.65</td>
      <td>0.59</td>
      <td>0.56</td>
    </tr>
    <tr>
      <th>Bond yield</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.85</td>
      <td>0.73</td>
      <td>0.62</td>
      <td>0.55</td>
      <td>0.49</td>
      <td>0.43</td>
    </tr>
    <tr>
      <th>Common stock prices</th>
      <td>1871 -- 1970</td>
      <td>100</td>
      <td>0.90</td>
      <td>0.76</td>
      <td>0.64</td>
      <td>0.53</td>
      <td>0.46</td>
      <td>0.43</td>
    </tr>
  </tbody>
</table>
</div>



### Mostrar las tablas 2 a 4 en una sola


```python
pd.concat([tabla2, tabla3, tabla4], axis=1)
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
      <th>Period</th>
      <th>T</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
      <th>Period</th>
      <th>T</th>
      <th>...</th>
      <th>r5</th>
      <th>r6</th>
      <th>Period</th>
      <th>T</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Real GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.90</td>
      <td>0.84</td>
      <td>0.79</td>
      <td>0.74</td>
      <td>0.69</td>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>...</td>
      <td>-0.19</td>
      <td>0.01</td>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.87</td>
      <td>0.66</td>
      <td>0.44</td>
      <td>0.26</td>
      <td>0.13</td>
      <td>0.07</td>
    </tr>
    <tr>
      <th>Nominal GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.89</td>
      <td>0.83</td>
      <td>0.77</td>
      <td>0.72</td>
      <td>0.67</td>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>...</td>
      <td>-0.07</td>
      <td>0.15</td>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.93</td>
      <td>0.79</td>
      <td>0.65</td>
      <td>0.52</td>
      <td>0.43</td>
      <td>0.35</td>
    </tr>
    <tr>
      <th>Real per capita GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.88</td>
      <td>0.81</td>
      <td>0.75</td>
      <td>0.70</td>
      <td>0.65</td>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>...</td>
      <td>-0.18</td>
      <td>0.02</td>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.87</td>
      <td>0.65</td>
      <td>0.43</td>
      <td>0.24</td>
      <td>0.11</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>Industrial production</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.97</td>
      <td>0.94</td>
      <td>0.90</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.81</td>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>...</td>
      <td>-0.28</td>
      <td>0.05</td>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.84</td>
      <td>0.67</td>
      <td>0.53</td>
      <td>0.40</td>
      <td>0.29</td>
      <td>0.28</td>
    </tr>
    <tr>
      <th>Employment</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.81</td>
      <td>0.76</td>
      <td>0.71</td>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>...</td>
      <td>-0.20</td>
      <td>0.01</td>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.89</td>
      <td>0.71</td>
      <td>0.55</td>
      <td>0.39</td>
      <td>0.25</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>Unemployment rate</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.75</td>
      <td>0.47</td>
      <td>0.32</td>
      <td>0.17</td>
      <td>0.04</td>
      <td>-0.01</td>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>...</td>
      <td>-0.19</td>
      <td>0.01</td>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.75</td>
      <td>0.46</td>
      <td>0.30</td>
      <td>0.15</td>
      <td>0.03</td>
      <td>-0.01</td>
    </tr>
    <tr>
      <th>GNP deflator</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.96</td>
      <td>0.93</td>
      <td>0.89</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.76</td>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>...</td>
      <td>0.03</td>
      <td>0.02</td>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.92</td>
      <td>0.81</td>
      <td>0.67</td>
      <td>0.54</td>
      <td>0.42</td>
      <td>0.30</td>
    </tr>
    <tr>
      <th>Consumer prices</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.77</td>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>...</td>
      <td>0.05</td>
      <td>0.03</td>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.97</td>
      <td>0.91</td>
      <td>0.84</td>
      <td>0.78</td>
      <td>0.71</td>
      <td>0.63</td>
    </tr>
    <tr>
      <th>Wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.82</td>
      <td>0.77</td>
      <td>0.73</td>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>...</td>
      <td>-0.09</td>
      <td>0.08</td>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.93</td>
      <td>0.81</td>
      <td>0.67</td>
      <td>0.54</td>
      <td>0.42</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>Real wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.75</td>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>...</td>
      <td>-0.18</td>
      <td>-0.15</td>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.87</td>
      <td>0.69</td>
      <td>0.52</td>
      <td>0.38</td>
      <td>0.26</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>Money stock</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.89</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.77</td>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>...</td>
      <td>-0.07</td>
      <td>-0.04</td>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.95</td>
      <td>0.83</td>
      <td>0.69</td>
      <td>0.53</td>
      <td>0.37</td>
      <td>0.21</td>
    </tr>
    <tr>
      <th>Velocity</th>
      <td>1869 -- 1970</td>
      <td>102</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.79</td>
      <td>1869 -- 1970</td>
      <td>102</td>
      <td>...</td>
      <td>-0.11</td>
      <td>0.11</td>
      <td>1869 -- 1970</td>
      <td>102</td>
      <td>0.91</td>
      <td>0.81</td>
      <td>0.72</td>
      <td>0.65</td>
      <td>0.59</td>
      <td>0.56</td>
    </tr>
    <tr>
      <th>Bond yield</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.84</td>
      <td>0.72</td>
      <td>0.60</td>
      <td>0.52</td>
      <td>0.46</td>
      <td>0.40</td>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>...</td>
      <td>0.06</td>
      <td>0.05</td>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.85</td>
      <td>0.73</td>
      <td>0.62</td>
      <td>0.55</td>
      <td>0.49</td>
      <td>0.43</td>
    </tr>
    <tr>
      <th>Common stock prices</th>
      <td>1871 -- 1970</td>
      <td>100</td>
      <td>0.96</td>
      <td>0.90</td>
      <td>0.85</td>
      <td>0.79</td>
      <td>0.75</td>
      <td>0.71</td>
      <td>1871 -- 1970</td>
      <td>100</td>
      <td>...</td>
      <td>-0.23</td>
      <td>0.02</td>
      <td>1871 -- 1970</td>
      <td>100</td>
      <td>0.90</td>
      <td>0.76</td>
      <td>0.64</td>
      <td>0.53</td>
      <td>0.46</td>
      <td>0.43</td>
    </tr>
  </tbody>
</table>
<p>14 rows × 24 columns</p>
</div>




```python
pd.concat([tabla2, tabla3.iloc[:,-6:], tabla4.iloc[:,-6:]], axis=1, keys=['Niveles','Diferencias','Desviación de tendencia'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="8" halign="left">Niveles</th>
      <th colspan="6" halign="left">Diferencias</th>
      <th colspan="6" halign="left">Desviación de tendencia</th>
    </tr>
    <tr>
      <th></th>
      <th>Period</th>
      <th>T</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
      <th>r1</th>
      <th>r2</th>
      <th>r3</th>
      <th>r4</th>
      <th>r5</th>
      <th>r6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Real GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.90</td>
      <td>0.84</td>
      <td>0.79</td>
      <td>0.74</td>
      <td>0.69</td>
      <td>0.34</td>
      <td>0.04</td>
      <td>-0.18</td>
      <td>-0.23</td>
      <td>-0.19</td>
      <td>0.01</td>
      <td>0.87</td>
      <td>0.66</td>
      <td>0.44</td>
      <td>0.26</td>
      <td>0.13</td>
      <td>0.07</td>
    </tr>
    <tr>
      <th>Nominal GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.89</td>
      <td>0.83</td>
      <td>0.77</td>
      <td>0.72</td>
      <td>0.67</td>
      <td>0.44</td>
      <td>0.08</td>
      <td>-0.12</td>
      <td>-0.24</td>
      <td>-0.07</td>
      <td>0.15</td>
      <td>0.93</td>
      <td>0.79</td>
      <td>0.65</td>
      <td>0.52</td>
      <td>0.43</td>
      <td>0.35</td>
    </tr>
    <tr>
      <th>Real per capita GNP</th>
      <td>1909 -- 1970</td>
      <td>62</td>
      <td>0.95</td>
      <td>0.88</td>
      <td>0.81</td>
      <td>0.75</td>
      <td>0.70</td>
      <td>0.65</td>
      <td>0.33</td>
      <td>0.04</td>
      <td>-0.17</td>
      <td>-0.21</td>
      <td>-0.18</td>
      <td>0.02</td>
      <td>0.87</td>
      <td>0.65</td>
      <td>0.43</td>
      <td>0.24</td>
      <td>0.11</td>
      <td>0.04</td>
    </tr>
    <tr>
      <th>Industrial production</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.97</td>
      <td>0.94</td>
      <td>0.90</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.81</td>
      <td>0.03</td>
      <td>-0.11</td>
      <td>-0.00</td>
      <td>-0.11</td>
      <td>-0.28</td>
      <td>0.05</td>
      <td>0.84</td>
      <td>0.67</td>
      <td>0.53</td>
      <td>0.40</td>
      <td>0.29</td>
      <td>0.28</td>
    </tr>
    <tr>
      <th>Employment</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.81</td>
      <td>0.76</td>
      <td>0.71</td>
      <td>0.32</td>
      <td>-0.05</td>
      <td>-0.08</td>
      <td>-0.17</td>
      <td>-0.20</td>
      <td>0.01</td>
      <td>0.89</td>
      <td>0.71</td>
      <td>0.55</td>
      <td>0.39</td>
      <td>0.25</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>Unemployment rate</th>
      <td>1890 -- 1970</td>
      <td>81</td>
      <td>0.75</td>
      <td>0.47</td>
      <td>0.32</td>
      <td>0.17</td>
      <td>0.04</td>
      <td>-0.01</td>
      <td>0.09</td>
      <td>-0.29</td>
      <td>0.03</td>
      <td>-0.03</td>
      <td>-0.19</td>
      <td>0.01</td>
      <td>0.75</td>
      <td>0.46</td>
      <td>0.30</td>
      <td>0.15</td>
      <td>0.03</td>
      <td>-0.01</td>
    </tr>
    <tr>
      <th>GNP deflator</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.96</td>
      <td>0.93</td>
      <td>0.89</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.76</td>
      <td>0.43</td>
      <td>0.20</td>
      <td>0.07</td>
      <td>-0.06</td>
      <td>0.03</td>
      <td>0.02</td>
      <td>0.92</td>
      <td>0.81</td>
      <td>0.67</td>
      <td>0.54</td>
      <td>0.42</td>
      <td>0.30</td>
    </tr>
    <tr>
      <th>Consumer prices</th>
      <td>1860 -- 1970</td>
      <td>111</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.87</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.77</td>
      <td>0.58</td>
      <td>0.16</td>
      <td>0.02</td>
      <td>-0.00</td>
      <td>0.05</td>
      <td>0.03</td>
      <td>0.97</td>
      <td>0.91</td>
      <td>0.84</td>
      <td>0.78</td>
      <td>0.71</td>
      <td>0.63</td>
    </tr>
    <tr>
      <th>Wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.96</td>
      <td>0.91</td>
      <td>0.86</td>
      <td>0.82</td>
      <td>0.77</td>
      <td>0.73</td>
      <td>0.46</td>
      <td>0.10</td>
      <td>-0.03</td>
      <td>-0.09</td>
      <td>-0.09</td>
      <td>0.08</td>
      <td>0.93</td>
      <td>0.81</td>
      <td>0.67</td>
      <td>0.54</td>
      <td>0.42</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>Real wages</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.84</td>
      <td>0.80</td>
      <td>0.75</td>
      <td>0.19</td>
      <td>-0.03</td>
      <td>-0.07</td>
      <td>-0.11</td>
      <td>-0.18</td>
      <td>-0.15</td>
      <td>0.87</td>
      <td>0.69</td>
      <td>0.52</td>
      <td>0.38</td>
      <td>0.26</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>Money stock</th>
      <td>1889 -- 1970</td>
      <td>82</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.89</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.77</td>
      <td>0.62</td>
      <td>0.30</td>
      <td>0.13</td>
      <td>-0.01</td>
      <td>-0.07</td>
      <td>-0.04</td>
      <td>0.95</td>
      <td>0.83</td>
      <td>0.69</td>
      <td>0.53</td>
      <td>0.37</td>
      <td>0.21</td>
    </tr>
    <tr>
      <th>Velocity</th>
      <td>1869 -- 1970</td>
      <td>102</td>
      <td>0.96</td>
      <td>0.92</td>
      <td>0.88</td>
      <td>0.85</td>
      <td>0.81</td>
      <td>0.79</td>
      <td>0.11</td>
      <td>-0.04</td>
      <td>-0.16</td>
      <td>-0.15</td>
      <td>-0.11</td>
      <td>0.11</td>
      <td>0.91</td>
      <td>0.81</td>
      <td>0.72</td>
      <td>0.65</td>
      <td>0.59</td>
      <td>0.56</td>
    </tr>
    <tr>
      <th>Bond yield</th>
      <td>1900 -- 1970</td>
      <td>71</td>
      <td>0.84</td>
      <td>0.72</td>
      <td>0.60</td>
      <td>0.52</td>
      <td>0.46</td>
      <td>0.40</td>
      <td>0.18</td>
      <td>0.31</td>
      <td>0.15</td>
      <td>0.04</td>
      <td>0.06</td>
      <td>0.05</td>
      <td>0.85</td>
      <td>0.73</td>
      <td>0.62</td>
      <td>0.55</td>
      <td>0.49</td>
      <td>0.43</td>
    </tr>
    <tr>
      <th>Common stock prices</th>
      <td>1871 -- 1970</td>
      <td>100</td>
      <td>0.96</td>
      <td>0.90</td>
      <td>0.85</td>
      <td>0.79</td>
      <td>0.75</td>
      <td>0.71</td>
      <td>0.22</td>
      <td>-0.13</td>
      <td>-0.08</td>
      <td>-0.18</td>
      <td>-0.23</td>
      <td>0.02</td>
      <td>0.90</td>
      <td>0.76</td>
      <td>0.64</td>
      <td>0.53</td>
      <td>0.46</td>
      <td>0.43</td>
    </tr>
  </tbody>
</table>
</div>



## Estimar la regresión Dickey-Fuller, "a pie"

Nelson Plosser 1982 tabla 5



```python
def ADFregression(nombre_variable, k):
    """
    Estima la regresión necesaria para la prueba aumentada de Dickey-Fuller
    Args:
        nombre_variable: str, nombre de una variable de la tabla "datos"
        k: Número de rezagos del proceso AR subyacente (1 + rezagos en regresión)

    Returns:
        Un objeto de resultados estimados de statsmodels
    """
    temp = datos[[nombre_variable]].dropna()
    temp.columns=['Y']
    temp['DY'] = temp['Y'].diff()
    temp['LY'] = temp['Y'].shift()
    temp['t'] = np.arange(temp.shape[0])    
    for j in range(1, k):
        temp[f'D{j}Y'] = temp['DY'].shift(j)
        regresores = ' + '.join(temp.columns[2:])
    print(regresores)
    frml = 'DY ~ ' + regresores
    return ols(frml, temp).fit()
    
```


```python
ADFregression('Real GNP', 4).summary()
```

    LY + t + D1Y + D2Y + D3Y
    




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>           <td>DY</td>        <th>  R-squared:         </th> <td>   0.265</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.194</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   3.753</td>
</tr>
<tr>
  <th>Date:</th>             <td>Mon, 04 Mar 2024</td> <th>  Prob (F-statistic):</th>  <td>0.00563</td>
</tr>
<tr>
  <th>Time:</th>                 <td>00:04:26</td>     <th>  Log-Likelihood:    </th> <td>  84.601</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>    58</td>      <th>  AIC:               </th> <td>  -157.2</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>    52</td>      <th>  BIC:               </th> <td>  -144.8</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     5</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
      <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th> <td>    0.8645</td> <td>    0.317</td> <td>    2.723</td> <td> 0.009</td> <td>    0.227</td> <td>    1.501</td>
</tr>
<tr>
  <th>LY</th>        <td>   -0.1880</td> <td>    0.070</td> <td>   -2.687</td> <td> 0.010</td> <td>   -0.328</td> <td>   -0.048</td>
</tr>
<tr>
  <th>t</th>         <td>    0.0062</td> <td>    0.002</td> <td>    2.803</td> <td> 0.007</td> <td>    0.002</td> <td>    0.011</td>
</tr>
<tr>
  <th>D1Y</th>       <td>    0.3991</td> <td>    0.129</td> <td>    3.091</td> <td> 0.003</td> <td>    0.140</td> <td>    0.658</td>
</tr>
<tr>
  <th>D2Y</th>       <td>    0.0741</td> <td>    0.140</td> <td>    0.531</td> <td> 0.598</td> <td>   -0.206</td> <td>    0.354</td>
</tr>
<tr>
  <th>D3Y</th>       <td>   -0.0693</td> <td>    0.136</td> <td>   -0.509</td> <td> 0.613</td> <td>   -0.343</td> <td>    0.204</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 3.110</td> <th>  Durbin-Watson:     </th> <td>   2.010</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.211</td> <th>  Jarque-Bera (JB):  </th> <td>   2.243</td>
</tr>
<tr>
  <th>Skew:</th>          <td>-0.313</td> <th>  Prob(JB):          </th> <td>   0.326</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 3.732</td> <th>  Cond. No.          </th> <td>1.57e+03</td>
</tr>
</table><br/><br/>Notes:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.<br/>[2] The condition number is large, 1.57e+03. This might indicate that there are<br/>strong multicollinearity or other numerical problems.




```python
def NelsonPlosser(ser, k):
    """
    Calcula varios estadísticos de la regresión de la prueba aumentada de Dickey-Fuller, 
    para reproducir los resultados reportados en la tabla 5 de Nelson y Plosser (1982)
    
    Args:
        ser: str, nombre abreviado de una variable de la tabla "datos"
        k: Número de rezagos del proceso AR subyacente (1 + rezagos en regresión)

    Returns:
        dict, estadísticos calculados        
    """
    nombre_variable = variables[ser]
    fuller5pct = -3.45
    model = ADFregression(nombre_variable, k)
    
    estadisticos = {
        'mu': np.round(model.params['Intercept'], 3),
        't(mu)': np.round(model.tvalues['Intercept'], 2),
        'gamma': np.round(model.params['t'], 3),
        't(gamma)': np.round(model.tvalues['t'], 2),
        'rho': np.round(model.params['LY'] + 1, 3),
        't(rho)': np.round(model.tvalues['LY'], 2),
        's(u)': np.round(np.sqrt(model.mse_resid),3),
        'r1': acf(model.resid, nlags=1, fft=False)[1].round(2),
        'resultado': '* estacionaria' if model.tvalues['LY'] < fuller5pct else 'raiz unitaria'
    }
    
    return estadisticos
    
```


```python
NelsonPlosser('lrgnp', 3)
```

    LY + t + D1Y + D2Y
    




    {'mu': 0.872,
     't(mu)': 2.98,
     'gamma': 0.006,
     't(gamma)': 2.99,
     'rho': 0.811,
     't(rho)': -2.94,
     's(u)': 0.059,
     'r1': -0.01,
     'resultado': 'raiz unitaria'}




```python
rezagos = {'lrgnp':2, 'lgnp':2, 'lpcrgnp':2,'lip':6, 'lemp':3, 'lun':4, 'lprgnp':2, 'lcpi':4, 'lwg':3, 'lrwg':2, 'lm':2, 'lvel':4,'bnd':3,'lsp500':3}
```


```python
tabla5 = pd.DataFrame([NelsonPlosser(ser, lags) for ser, lags in rezagos.items()], index=rezagos.keys())
tabla5
```

    LY + t + D1Y
    LY + t + D1Y
    LY + t + D1Y
    LY + t + D1Y + D2Y + D3Y + D4Y + D5Y
    LY + t + D1Y + D2Y
    LY + t + D1Y + D2Y + D3Y
    LY + t + D1Y
    LY + t + D1Y + D2Y + D3Y
    LY + t + D1Y + D2Y
    LY + t + D1Y
    LY + t + D1Y
    LY + t + D1Y + D2Y + D3Y
    LY + t + D1Y + D2Y
    LY + t + D1Y + D2Y
    




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
      <th>mu</th>
      <th>t(mu)</th>
      <th>gamma</th>
      <th>t(gamma)</th>
      <th>rho</th>
      <th>t(rho)</th>
      <th>s(u)</th>
      <th>r1</th>
      <th>resultado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>lrgnp</th>
      <td>0.813</td>
      <td>3.04</td>
      <td>0.006</td>
      <td>3.03</td>
      <td>0.825</td>
      <td>-2.99</td>
      <td>0.058</td>
      <td>-0.03</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lgnp</th>
      <td>1.056</td>
      <td>2.37</td>
      <td>0.006</td>
      <td>2.34</td>
      <td>0.899</td>
      <td>-2.32</td>
      <td>0.087</td>
      <td>0.03</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lpcrgnp</th>
      <td>1.274</td>
      <td>3.05</td>
      <td>0.004</td>
      <td>3.01</td>
      <td>0.818</td>
      <td>-3.05</td>
      <td>0.059</td>
      <td>-0.03</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lip</th>
      <td>0.070</td>
      <td>2.95</td>
      <td>0.007</td>
      <td>2.44</td>
      <td>0.835</td>
      <td>-2.53</td>
      <td>0.097</td>
      <td>0.03</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lemp</th>
      <td>1.414</td>
      <td>2.68</td>
      <td>0.002</td>
      <td>2.54</td>
      <td>0.861</td>
      <td>-2.66</td>
      <td>0.035</td>
      <td>0.03</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lun</th>
      <td>0.515</td>
      <td>2.76</td>
      <td>-0.000</td>
      <td>-0.23</td>
      <td>0.706</td>
      <td>-3.55</td>
      <td>0.407</td>
      <td>0.02</td>
      <td>* estacionaria</td>
    </tr>
    <tr>
      <th>lprgnp</th>
      <td>0.258</td>
      <td>2.55</td>
      <td>0.002</td>
      <td>2.65</td>
      <td>0.915</td>
      <td>-2.52</td>
      <td>0.046</td>
      <td>-0.04</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lcpi</th>
      <td>0.088</td>
      <td>1.74</td>
      <td>0.001</td>
      <td>2.84</td>
      <td>0.968</td>
      <td>-1.97</td>
      <td>0.042</td>
      <td>-0.14</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lwg</th>
      <td>0.558</td>
      <td>2.30</td>
      <td>0.004</td>
      <td>2.30</td>
      <td>0.910</td>
      <td>-2.24</td>
      <td>0.060</td>
      <td>0.00</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lrwg</th>
      <td>0.484</td>
      <td>3.10</td>
      <td>0.004</td>
      <td>3.14</td>
      <td>0.831</td>
      <td>-3.05</td>
      <td>0.035</td>
      <td>-0.02</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lm</th>
      <td>0.128</td>
      <td>3.53</td>
      <td>0.005</td>
      <td>3.03</td>
      <td>0.916</td>
      <td>-3.08</td>
      <td>0.047</td>
      <td>0.03</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lvel</th>
      <td>0.042</td>
      <td>0.72</td>
      <td>-0.000</td>
      <td>-0.40</td>
      <td>0.946</td>
      <td>-1.40</td>
      <td>0.066</td>
      <td>-0.02</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>bnd</th>
      <td>-0.193</td>
      <td>-0.97</td>
      <td>0.003</td>
      <td>1.75</td>
      <td>1.032</td>
      <td>0.69</td>
      <td>0.284</td>
      <td>-0.05</td>
      <td>raiz unitaria</td>
    </tr>
    <tr>
      <th>lsp500</th>
      <td>0.089</td>
      <td>1.63</td>
      <td>0.003</td>
      <td>2.39</td>
      <td>0.908</td>
      <td>-2.12</td>
      <td>0.154</td>
      <td>0.01</td>
      <td>raiz unitaria</td>
    </tr>
  </tbody>
</table>
</div>



**NOTA:** Una versión anterior de este archivo tenía la réplica del trabajo de KPSS. Ese código ahora está en el cuaderno KPSS.ipynb.
