# Statistics_Notebook

The author intended to present some advanced statistical methods in transportation areas. In the first section, the author planned to talk about panel data and give some examples with popular econometric softwares such as Stata, R and Python.

## 1. Panel data

### 1.1 Linear regression models

In general, panel data included two perspectives: the cross-sectional level and the time series level, including more information than traditional linear regression models. Thus, some researchers proposed fixed effect and random effect models to deal with unobserved heterogeneity problems, with relative to the pooled ols model.

#### Stata code

***

```Stata
**pooled ols with robust error
reg y x1 x2 x3, vce(cluster id)
estimates store ols

**Fixed Effects
xtreg y x1 x2 x3,fe robust

**FE with time fixed effects
xtreg y x1 x2 x3 i.year, fe robust
estimates store fe

**Random Effects
xtreg y x1 x2 x3, re robust theta
**theta is the parameter for the LSDV method or
xtreg y x1 x2 x3, re mle

**statistical test methods

*OLS or RE H0: ui=0
xttest0

*RE or FE
hausman fe re, constant sigmamore or sigmaless

*Time Effects
testparm _Iyear*

*The fixed effect models could not estimate variables that kept unchanged over time, such as gender, education degree. The approximate solution was to add the intersection between the time variable and the unchanged variables.
*heteroskedasticity：xttest3 robust; autocorrelated：xttest2 cluster；cross-sectional correlation: xtserial xtscc y x, fe or fe

*format the output
esttab ols fe using C:\Users\LAYZ\Desktop, replace b(%12.3f) sec(%12.3f) star(0.1,0.05,0.01)
```

***

#### R code

***

```R
library(plm)
mydata<-read.csv("file path")
ols<-lm(y~x1+x2+x3,data=mydata)

FE<-lm(y~x1+x2+x3+factor(year)+factor(individual_id)-1,data=mydata)
#or FE<-plm(y~x1+x2+x3,index=c("year","individual_id),model="within")

#testing for fixed effects
pFtest(FE,ols)

RE<-plm(y~x1+x2+x3,index=c("year","individual_id"),model="random")

#FE or RE
phtest(FE.RE)

```

***

### Python code

***

updating

***

### Reference

[Panel Data Analysis Fixed and Random Effects using Stata](https://www.princeton.edu/~otorres/Panel101.pdf)

[Getting Started in Fixed/Random Effects Models using R](https://www.princeton.edu/~otorres/Panel101R.pdf)

[Introduction to Econometrics with R](https://www.econometrics-with-r.org/10-3-fixed-effects-regression.html)
