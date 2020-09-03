# Background
to see there is association between Urban Rate and International Energy Agency

# Conclusion
- there is association between urbanrate and international agency
- positve correlation and the coefficient equals 0.47

# Analysing process
### 1. data interpretation: 
- urbanrate: 2008 urban population (% of total)
- relectricperperson: 2008 residential electricity consumption, per person (kWh)
### 2. results
- r equals 0.47 and p_value smaller than 0.05,which means there is association between urbanrate and international agency

![the association between urbanrate and international agency](./images/week_3_1.png)


```
import pandas
import numpy
import seaborn
import scipy
import matplotlib.pyplot as plt

data = pandas.read_csv('gapminder.csv', low_memory=False)

#setting variables you will be working with to numeric
data['relectricperperson'] = data['relectricperperson'].convert_objects(convert_numeric=True)
data['urbanrate'] = data['urbanrate'].convert_objects(convert_numeric=True)

data['relectricperperson']=data['relectricperperson'].replace(' ', numpy.nan)

scat1 = seaborn.regplot(x="urbanrate", y="relectricperperson", fit_reg=True, data=data)
plt.xlabel('Urban Rate')
plt.ylabel('International Energy Agency')
plt.title('Scatterplot for the Association Between Urban Rate and International Energy Agency')

data_clean=data.dropna()


print ('association between incomeperperson and relectricperperson')
print (scipy.stats.pearsonr(data_clean['urbanrate'], data_clean['relectricperperson']))

```