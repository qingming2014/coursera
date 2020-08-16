# 分析背景
基于方差分析，探索青少年父母抑郁人数与自身抑郁症是否有相关性

# 分析结论
1.青少年父母抑郁的人数与自身是否抑郁相关

2.不同种族的青少年父母抑郁人数不全相等 

# 分析过程
1.显著性水平 < 0.05，拒绝原假设，即认为青少年父母抑郁人数与自身抑郁相关

2.不同种族的青少年父母抑郁人数均相等：显著性水平 < 0.05，拒绝原假设，即认为不同种族青少年父母抑郁人数均值不全相等

3.进一步地，Black & Asian/Native Hawaiian/Pacific Islander、Asian/Native Hawaiian/Pacific Islander & Hispanic or Latino  种族父母抑郁人数均值相等

```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Tue Aug 11 22:30:36 2020

@author: kimmywang
"""

import numpy
import pandas
import statsmodels.formula.api as smf
import statsmodels.stats.multicomp as multi 

data = pandas.read_csv('nesarc.csv', low_memory=False)

#replace the status to numbers
data['S4BQ1'] = data['S4BQ1'].replace(2, 0)
data['S4BQ2'] = data['S4BQ2'].replace(2, 0)

#setting variables you will be working with to numeric
data['S4BQ1'] = data['S4BQ1'].apply(pandas.to_numeric, errors='coerce')
data['S4BQ2'] = data['S4BQ2'].apply(pandas.to_numeric, errors='coerce')

#subset data to the specific family depression history
sub1=data[(data['S4BQ1']!=9) & (data['S4BQ2']!=9)]

#add to get the total numbers
sub1['USFAMDEP']= sub1['S4BQ1'] + sub1['S4BQ2']

#converting new variable USFREQMMO to numeric
sub1['USFAMDEP']= sub1['USFAMDEP'].apply(pandas.to_numeric, errors='coerce')


ct1 = sub1.groupby('USFAMDEP').size()
print (ct1)

# using ols function for calculating the F-statistic and associated p value
model1 = smf.ols(formula='USFAMDEP ~ C(MAJORDEPLIFE)', data=sub1)
results1 = model1.fit()
print (results1.summary())

sub2 = sub1[['USFAMDEP', 'MAJORDEPLIFE']].dropna()

print ('means for numfamilies by major depression status')
m1= sub2.groupby('MAJORDEPLIFE').mean()
print (m1)

print ('standard deviations for numcigmo_est by major depression status')
sd1 = sub2.groupby('MAJORDEPLIFE').std()
print (sd1)

#i will call it sub3
sub3 = sub1[['USFAMDEP', 'ETHRACE2A']].dropna()

model2 = smf.ols(formula='USFAMDEP ~ C(ETHRACE2A)', data=sub3).fit()
print (model2.summary())

print ('means for numcigmo_est by major depression status')
m2= sub3.groupby('ETHRACE2A').mean()
print (m2)

print ('standard deviations for numcigmo_est by major depression status')
sd2 = sub3.groupby('ETHRACE2A').std()
print (sd2)

mc1 = multi.MultiComparison(sub3['USFAMDEP'], sub3['ETHRACE2A'])
res1 = mc1.tukeyhsd()
print(res1.summary())

```