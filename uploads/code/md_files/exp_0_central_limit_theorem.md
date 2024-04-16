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

## **Central Limit Theorem** 
- Experiment N° **0**  
- Link: [here](https://en.wikipedia.org/wiki/Central_limit_theorem)

#### Design

>*Empirical validation of CLT for different distributions.*

## 📚 Index

- [I. Section Title](#section-I)
- [II. Section Title](#section-II)
- [III. Section Title](#section-III)
- [IV. Section Title](#section-IV)

https://www.linkedin.com/feed/update/urn:li:activity:7048362427205201920/

https://towardsdatascience.com/verifying-the-assumptions-of-linear-regression-in-python-and-r-f4cd2907d4c0

# section-I
---


```python
import numpy as np
import matplotlib.pyplot as plt
```


```python
import os
os.getcwd()
```




    'C:\\Users\\juani\\Documents\\3_My_Jupiter_Notebooks\\4_Series_de_Tiempo\\0_2024\\exps_&_reps'




```python
import warnings
warnings.simplefilter(action='ignore', category= FutureWarning)

# general settings
class CFG:
    data_folder = '../input/tsdata-1/'
    img_dim1 = 20
    img_dim2 = 10
    style = 'fivethirtyeight'


    
# adjust the parameters for displayed figures    
plt.rcParams.update({'figure.figsize': (CFG.img_dim1,CFG.img_dim2)})    

plt.style.use(CFG.style) 
```


```python
import warnings
warnings.simplefilter(action='ignore', category= FutureWarning)

# general settings
class CFG:
    data_folder = '../input/tsdata-1/'
    img_dim1 = 20
    img_dim2 = 10
    style = 'fivethirtyeight'


    
# adjust the parameters for displayed figures    
plt.rcParams.update({'figure.figsize': (CFG.img_dim1,CFG.img_dim2)})    

plt.style.use(CFG.style) 
```

# section-II
---

### Normal Distribution


```python
np.random.seed(42)
population_size = 10000

mu, sigma = 0, 100 # mean and standard deviation
height_population =  np.random.normal(mu,sigma,population_size)
sample_size, no_of_samples = 10 , 10000


sample_means = []


for i in range(no_of_samples):
    sample = np.random.choice(height_population, sample_size)
    sample_mean = np.mean(sample)
    sample_means.append(sample_mean)
    
plt.hist(height_population, bins = 100, density = True, alpha= 0.8, label = 'Population')
plt.xlabel('Height') , plt.ylabel('Frequency')
plt.title('Population and Sample Means Distribution')

plt.hist(sample_means, bins = 100, density = True, alpha= 0.8, label = 'Sample Means')
plt.legend, plt.show()
```


    
![png](output_13_0.png)
    





    (<function matplotlib.pyplot.legend(*args, **kwargs)>, None)



### Uniform Distribution


```python
np.random.seed(42)
population_size = 10000


mini = -100
maxi = 100

height_population =  np.random.uniform(mini,maxi,population_size)
sample_size, no_of_samples = 10 , 10000


sample_means = []


for i in range(no_of_samples):
    sample = np.random.choice(height_population, sample_size)
    sample_mean = np.mean(sample)
    sample_means.append(sample_mean)
    
plt.hist(height_population, bins = 100, density = True, alpha= 0.8, label = 'Population')
plt.xlabel('Height') , plt.ylabel('Frequency')
plt.title('Population and Sample Means Distribution')

plt.hist(sample_means, bins = 100, density = True, alpha= 0.8, label = 'Sample Means')
plt.legend, plt.show()
```


    
![png](output_15_0.png)
    





    (<function matplotlib.pyplot.legend(*args, **kwargs)>, None)



### Beta 1 Distribution


```python
np.random.seed(42)
mini = 8
maxi = 2
population_size = 10000
height_population =  np.random.beta(mini,maxi,population_size)
sample_size, no_of_samples = 10 , 10000


sample_means = []


for i in range(no_of_samples):
    sample = np.random.choice(height_population, sample_size)
    sample_mean = np.mean(sample)
    sample_means.append(sample_mean)
    
plt.hist(height_population, bins = 100, density = True, alpha= 0.8, label = 'Population')
plt.xlabel('Height') , plt.ylabel('Frequency')
plt.title('Population and Sample Means Distribution')

plt.hist(sample_means, bins = 100, density = True, alpha= 0.8, label = 'Sample Means')
plt.legend, plt.show()
```


    
![png](output_17_0.png)
    





    (<function matplotlib.pyplot.legend(*args, **kwargs)>, None)



### Beta 2 Distribution


```python
np.random.seed(42)
mini = 2
maxi = 8
population_size = 10000
height_population =  np.random.beta(mini,maxi,population_size)
sample_size, no_of_samples = 10 , 10000


sample_means = []


for i in range(no_of_samples):
    sample = np.random.choice(height_population, sample_size)
    sample_mean = np.mean(sample)
    sample_means.append(sample_mean)
    
plt.hist(height_population, bins = 100, density = True, alpha= 0.8, label = 'Population')
plt.xlabel('Height') , plt.ylabel('Frequency')
plt.title('Population and Sample Means Distribution')

plt.hist(sample_means, bins = 100, density = True, alpha= 0.8, label = 'Sample Means')
plt.legend, plt.show()
```


    
![png](output_19_0.png)
    





    (<function matplotlib.pyplot.legend(*args, **kwargs)>, None)



### Beta 3 Distribution


```python
np.random.seed(42)
mini = 0.5
maxi = 0.5
population_size = 10000
height_population =  np.random.beta(mini,maxi,population_size)
sample_size, no_of_samples = 10 , 10000


sample_means = []


for i in range(no_of_samples):
    sample = np.random.choice(height_population, sample_size)
    sample_mean = np.mean(sample)
    sample_means.append(sample_mean)
    
plt.hist(height_population, bins = 100, density = True, alpha= 0.8, label = 'Population')
plt.xlabel('Height') , plt.ylabel('Frequency')
plt.title('Population and Sample Means Distribution')

plt.hist(sample_means, bins = 100, density = True, alpha= 0.8, label = 'Sample Means')
plt.legend, plt.show()
```


    
![png](output_21_0.png)
    





    (<function matplotlib.pyplot.legend(*args, **kwargs)>, None)



# section-III
---

# section-IV

---

Credits to [Snehil Raj Choubey](https://www.linkedin.com/in/snehilrc/).
