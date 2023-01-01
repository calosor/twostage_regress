# 2SLS IV Regression
2SLS IV regression with Python

## Overview

This module allows you 2SLS IV regression estimation on Python. It provides also a final summary report where you can check first stage results, second stage results, and weak identification test for instruments. The estimates are executed using my ols linear regression module that i also attach in this repository. You can also find it in a standalone repository in my github profile.

##  Inputs
The 2SLS IV Regression model can be called by using the class *two_sls* in the main file. To call this class, you need the following inputs:

- *dataset* is the dataframe where you stored your data you want to use for the IV regression.
- *dependent* is name of the column of the dependent variable in your pandas dataset.
- *regressors* is the list of columns' name of exogenous regressors that you want to use for the regression.
**It is very important that you pass a list of strings even if there is just one variable for the category.**
- *endogenous* is the column name of the endogenous variable in the datagrame. This is not a list but just a string.
- *instruments* is the list of columns' name of instruments that you want to use in addition of exogenous regressors.
- *cons* is True by default, but if you want to regress without intercept, just declare *cons = False* .
- *fixed_eff* by default is an empty list. However, you can pass a list with the string of all variables you want to use for fixed effects. For example, if you have a panel dataset of countries years, you can pass a list of string of the variable that stores countries and a string for the variable that stores years.
**BY NOW IT DOESN'T WORK SINCE IT IS NOT IMPLEMENTED AND TRIED. PLEASE DO NOT CHANGE THIS PARAMETER**

## Features
- To summarize the results, just call "your object name" . *summary()*. The report includes first stage table of results, second stage table of results, and weak identification test for the instrument, reporting Cragg-Donald Wald F statistic and Stock-Yogo critical values. 

- Use *first_stage() to return a dictionary of usefull elements for the first stage regression like the ols_model (*'model'*), fitted values (*'fitted'*), and beta coefficients (*'betas'*)
- Use *second_stage().get('std')* to return a dictionary of usefull elements for the second stage regression. You can obtain beta coefficients (*'beta'*) and variance covariance matrix (*'var_matrix'*). Other elements can be obtain. Check the code at the second stage function to see dictionary's keys.
- Use *weak_id_test()* to get the Cragg-Donald Wald F statistic.
- Use *summary()* if you want a table form summary of the estimation.

## Very Important
- **For now, it is possible to use just one instrument for just one endogenous variable. Updates to complete the code are coming.**
- **To use the two stage module you will also need the ols linear regression module that you can find in this repository.**

## Example
To import the model 
```
import tsls_reg as tsls
```

Initialize the model 

```
model = tsls.two_sls(dataset=df, dependent = 'wage', regressors= ['exper'],
                endogenous= 'educ', instruments=['sibs'])
```

Print the results 

```
print(model.summary())
```

And here is the output:
```
First stage regression
-------------------------------------------------------------------------------
educ      coefficient         se       t    p_value     low 95    high 95
------  -------------  ---------  ------  ---------  ---------  ---------
exper       -0.221952  0.0142567  -15.57          0  -0.249895  -0.194009
sibs        -0.200841  0.0270426   -7.43          0  -0.253845  -0.147838
cons        16.6257    0.188911    88.01          0  16.2555    16.996
-------------------------------------------------------------------------------

Second stage regression
-------------------------------------------------------------------------------
wage      coefficient         se      t    p_value      low 95    high 95
------  -------------  ---------  -----  ---------  ----------  ---------
exper         32.1567    7.06488   4.55      0         18.3095    46.0038
educ         139.684    28.0369    4.98      0         84.7315   194.636
cons       -1295.23    453.262    -2.86      0.004  -2183.62    -406.834
-------------------------------------------------------------------------------

Weak instruments identification test
----------------------------------------------------------------------------
Cragg-Donald Wald F statistic: 55.158218554314
Stock Yogo weak ID critical values: 10%:16; 15%:9; 20%:7; 25%:6
Reference: Stock-Yogo (2005)

----------------------------------------------------------------------------
Instrumented: ['educ']
Included instruments: ['exper']
Excluded instruments: ['sibs']
----------------------------------------------------------------------------


Process finished with exit code 0
```

## If you want to use it, a citation is more than welcome


## References
- Stock J, Yogo M. Testing for Weak Instruments in Linear IV Regression. In: Andrews DWK Identification and Inference for Econometric Models. New York: Cambridge University Press ; 2005. pp. 80-108.
- Dataset from Wooldridge data sets: http://fmwww.bc.edu/ec-p/data/wooldridge/datasets.list.html
- Wooldridge, Jeffrey M. Econometric analysis of cross section and panel data. MIT press, 2010.
