---
title: "Mrs. Powell's Breakfast Saturdays"
author: Adam Romriell
output: 
  html_document: 
    theme: cerulean
    code_folding: hide
    keep_md: yes
---



### Background

On September 19th, 2020, Mrs. Powell's in Rexburg started to provide a breakfast menu of biscuits and gravy and french toast made from our monkey bread. There have been people that have come in and rave about how good it is but are the words of a couple of people really indicative of the success of the new menu options? Are Saturdays with the new breakfast menu making more money than similar Saturdays at the same time as last year? 

Another question arose while trying to answer this question; is it even fair to judge the Saturdays in 2020 and early 2021 to the 2019 and early 2020 Saturdays when there was a pandemic that may or may not have affected revenue numbers. The question then became: is the comparison between the two Saturdays a fair comparison to make?

The following data table is all the data that was gathered to help answer these questions.

## {.tabset .tabset-pills}

### Summary

In summary, the results that I have found in my anaylsis are as follows:

1) There is no significant difference in the Saturdays of Sept. 21st, 2019 through Feb. 29th, 2020 and Sept. 19th, 2020 through Feb. 27, 2021. (see "Analysis Saturdays..." for further information)


```r
ggplot(Bfast.difference, aes(x=factor(Breakfast), y=NetSales))+
  geom_boxplot(fill = "skyblue", color = "black")+
  labs(title = "Net sales for Saturdays with and without the breakfast menu", y= "Net Sales ($)", x = "Breakfast present (0 = no, 1 = yes)")
```

![](BreakfastSaturdays_files/figure-html/unnamed-chunk-2-1.png)<!-- -->


```r
Bfast.difference %>% 
  group_by(Breakfast) %>% 
  summarise(Minimum =min(NetSales),
            Q1 = quantile(NetSales, 0.25),
            Median = median(NetSales),
            Q3 = quantile(NetSales, 0.75), 
            Maximum = max(NetSales)) %>% 
    pander()
```


--------------------------------------------------------
 Breakfast   Minimum    Q1     Median    Q3     Maximum 
----------- --------- ------- -------- ------- ---------
     0        497.1    688.6   816.6    968.8    1940   

     1        454.1    753.5   783.5    901.1    1339   
--------------------------------------------------------

2) While it is fair to compare the two groups of Saturdays despite the pandemic, the new breakfast menu has helped the Rexburg store to weather the storm! 

To see the fair comparison, look at where the red and green lines start. Look at the incline in the green line and the drastic decrease in the red line. 


```r
Bfast.lm <- lm(NetSales ~ TotalSumCases + Breakfast + TotalSumCases:Breakfast, data = Bfast.NetSales2)

b <- coef(Bfast.lm)

plot(NetSales~TotalSumCases, data = Bfast.NetSales2, col = c("red" ,"green")[as.factor(Breakfast)], pch = 16)

legend("topleft", legend=c("Baseline: no breakfast menu", "Changed-line: breakfast menu"), bty="n", lty=1, col=c("red","green3"), cex=0.8)


curve(b[1] + b[2]*x, col="red", lwd=2, add=TRUE)  
curve((b[1] + b[3]) + (b[2] + b[4])*x, col="green3", lwd=2, add=TRUE)
```

![](BreakfastSaturdays_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

(see "... Covid-19 pandemic affected..." for further information and analysis)

3) It may be helpful to see how things played out in real time. 

The black line shows net sales for a particular Saturday. The red line show total Covid cases for that Saturday. The vertical blue line shows the start of the breakfast menu at Mrs. Powell's in Rexburg. The days where the store was not open and had no net sales were removed to show the continuity. 


```r
plot(NetSales ~ RunningWeek, data=Bfast.Saturdays0, type="l")
lines(TotalSumCases ~ RunningWeek, data= Bfast.Saturdays0, col="red")
abline(v= 90, lwd=2, col="skyblue")
```

![](BreakfastSaturdays_files/figure-html/unnamed-chunk-5-1.png)<!-- -->



### Data Table

Breakfast is represented with a 0, no breakfast menu, and 1, breakfast menu present. Week number is the particular week in the year that the Saturday falls on. The plus reveals the total number of cases that Eastern Idaho Public Health (EIPH) had on record at the time. 

For the tests that will be performed, 2 weeks were removed. One week was the Saturday following Thanksgiving. This is due to the bakery being closed on that particular day. The other week that was removed was the week of Christmas. In 2020, the Saturday following Christmas, Boxing Day, the bakery was also closed. In 2019, however, the bakery was open on the Saturday following Christmas. 


```{=html}
<div class="datatables html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-1f3bf59fd12750312594" style="width:100%;height:auto;"></div>
<script type="application/json" data-for="htmlwidget-1f3bf59fd12750312594">{"x":{"filter":"none","vertical":false,"extensions":["Responsive"],"data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76"],["9/21/2019","9/28/2019","10/5/2019","10/12/2019","10/19/2019","10/26/2019","11/2/2019","11/9/2019","11/16/2019","11/23/2019","11/30/2019","12/7/2019","12/14/2019","12/21/2019","12/28/2019","1/4/2020","1/11/2020","1/18/2020","1/25/2020","2/1/2020","2/8/2020","2/15/2020","2/22/2020","2/29/2020","3/7/2020","3/14/2020","3/21/2020","3/28/2020","4/4/2020","4/11/2020","4/18/2020","4/25/2020","5/2/2020","5/9/2020","5/16/2020","5/23/2020","5/30/2020","6/6/2020","6/13/2020","6/20/2020","6/27/2020","7/4/2020","7/11/2020","7/18/2020","7/25/2020","8/1/2020","8/8/2020","8/15/2020","8/22/2020","8/29/2020","9/5/2020","9/12/2020","9/19/2020","9/26/2020","10/3/2020","10/10/2020","10/17/2020","10/24/2020","10/31/2020","11/7/2020","11/14/2020","11/21/2020","11/28/2020","12/5/2020","12/12/2020","12/19/2020","12/26/2020","1/2/2021","1/9/2021","1/16/2021","1/23/2021","1/30/2021","2/6/2021","2/13/2021","2/20/2021","2/27/2021"],[834.17,971.37,1540.08,925.26,961.08,1299.49,689.42,688.32,918.85,807.55,0,1939.84,1483.51,1276.63,525.35,577.51,738.48,825.61,750.47,550.69,673.46,497.06,686.61,709.29,706.42,996.2,365.69,290.43,574.66,590.34,306.65,330.71,485.92,650.77,303.22,519.56,371.25,574.16,529.46,824.12,456.35,0,325.47,438.06,99.74,466.44,497.96,519.09,505.26,542.91,591.24,839.84,828.42,603.14,1320.78,506.72,774.03,954.84,889.9,791.38,904.79,888.91,0,577.29,1271.34,1339.01,0,454.13,484.49,820.49,765.82,775.69,767.29,1041.46,767.64,749.33],[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],[38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,1,2,3,4,5,6,7,8],[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,4,9,17,20,6,2,7,11,10,0,21,30,24,22,54,66,88,171,197,200,319,341,333,365,341,309,441,603,547,611,626,395,306,489,614,1076,838,982,980,756,435,414,544,556,370,324,333,282,276,400]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>Dates.OnlySaturdays.<\/th>\n      <th>NetSales<\/th>\n      <th>Breakfast<\/th>\n      <th>WeekNumber<\/th>\n      <th>TotalSumCases<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"lengthMenu":[5,10,30],"columnDefs":[{"className":"dt-right","targets":[2,3,4,5]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"responsive":true}},"evals":[],"jsHooks":[]}</script>
```

### Analysis Saturdays with and without breakfast (t-test)

To test these questions, revenue data was gathered on the Saturdays in 2020 and 2021 that had the new breakfast menu. An analysis was done to look at the before, 2019 - early 2020, revenue and the after, 2020 - early 2021, revenue to see if there was a significant difference. 


#### Hypothesis tests

The hypothesis that I will use for the paired samples t-test will be:

$$
  H_0: \mu_d = 0
$$
$$
  H_a: \mu_d \neq 0
$$
The significance level will be set at: 

$$
  \alpha = 0.05
$$



```r
t.test(Bfast.difference1$NetSales, Bfast.difference0$NetSales, paired = TRUE, mu = 0 , alternative = "two.sided", conf.level = 0.95) %>% 
  pander(caption = "Paired t-test")
```


--------------------------------------------------------------------------
 Test statistic   df   P value   Alternative hypothesis   mean difference 
---------------- ---- --------- ------------------------ -----------------
     -1.224       21   0.2346          two.sided              -93.99      
--------------------------------------------------------------------------

Table: Paired t-test
Because the p-value is less than the significance level we fail to reject the null hypothesis. This means that there is insufficient evidence to suggest that breakfast has made a significant difference on net sales. 


This plot shows if the data points are normal. If the points fall within the dotted blue lines, that means that the data is normal and a t-test is appropriate. It looks like 11, is outside the dotted blue lines which would suggest that particular data point may be an outlier. 

```r
qqPlot(Bfast.difference1$NetSales - Bfast.difference0$NetSales)
```

![](BreakfastSaturdays_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```
## [1] 11 20
```

To be safe, a similar test will be run called the Wilcoxon Signed-Rank test. 


```r
wilcox.test(Bfast.difference1$NetSales, Bfast.difference0$NetSales, paired = TRUE, mu = 0 , alternative = "two.sided", conf.level = 0.95) %>% 
  pander(caption = "Wilcoxon signed-rank test")
```


---------------------------------------------------
 Test statistic   P value   Alternative hypothesis 
---------------- --------- ------------------------
       99          0.388          two.sided        
---------------------------------------------------

Table: Wilcoxon signed-rank test

Again, the p-value is is greater than the significance level and we fail to rejcet the null hypothesis. This again means that there is insufficient evidence to suggest that there is a significant difference between 


#### Analysis of the hypothesis tests


```r
ggplot(Bfast.difference, aes(x=factor(Breakfast), y=NetSales))+
  geom_boxplot(fill = "skyblue", color = "black")+
  labs(title = "Net sales for Saturdays with and without the breakfast menu", y= "Net Sales ($)", x = "Breakfast present (0 = no, 1 = yes)")
```

![](BreakfastSaturdays_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


```r
Bfast.difference %>% 
  group_by(Breakfast) %>% 
  summarise(Minimum =min(NetSales),
            Q1 = quantile(NetSales, 0.25),
            Median = median(NetSales),
            Q3 = quantile(NetSales, 0.75), 
            Maximum = max(NetSales)) %>% 
    pander()
```


--------------------------------------------------------
 Breakfast   Minimum    Q1     Median    Q3     Maximum 
----------- --------- ------- -------- ------- ---------
     0        497.1    688.6   816.6    968.8    1940   

     1        454.1    753.5   783.5    901.1    1339   
--------------------------------------------------------

While the medians are different there is no statistical difference between the two groups of Saturdays. There also looks to be a bit more consistency in the breakfast group than the non-breakfast group.

This then leads to the question previously posed; is the comparison between the two Saturdays a fair comparison to make? Has the Covid-19 pandemic affected net sales on Saturdays?

### Has the Covid-19 pandemic affected sales on Saturdays? (Multi-linear Regression)

This analysis will be used to find out if the comparison between the two groups of Saturdays is a fair comparison to make. This will be evaluated by looking at the difference in y-intercepts. If the y-intercepts are significantly different, then the comparison between the groups of Saturdays will be unfair. The other question that we will be testing is if the breakfast menu served on Saturdays has helped or hindered revenue on Saturdays. 

The model that will be used for this linear regression is the following. 

$$
  \underbrace{Y_i}_\text{Net Sales (in dollars)} = \beta_0 + \beta_1 \underbrace{X_{1i}}_\text{Total Sum of Cases} + \beta_2\underbrace{X_{2i}}_\text{Breakfast or not(1 and 0)} +  \beta_3 \underbrace{X_{1i}X_{2i}}_\text{Total sum of cases:Breakfast} + \epsilon_i
$$

My hypothesis to test the fairness of the comparison will be:

$$
  H_0: \beta_2 = 0
$$

$$
  H_a: \beta_2 \neq 0
$$
My hypothesis to test if breakfast helped revenue numbers during the pandemic is:

$$
  H_0: \beta_3 = 0
$$

$$
  H_a: \beta_3 \neq 0
$$


We will establish the following significance level.

$$
  \alpha = 0.05
$$



```r
Bfast.lm <- lm(NetSales ~ TotalSumCases + Breakfast + TotalSumCases:Breakfast, data = Bfast.NetSales2)
summary(Bfast.lm) %>% 
  pander()
```


----------------------------------------------------------------------------
           &nbsp;              Estimate   Std. Error   t value    Pr(>|t|)  
----------------------------- ---------- ------------ ---------- -----------
       **(Intercept)**          730.2       51.88       14.07     7.946e-22 

      **TotalSumCases**        -0.8295       0.41       -2.023     0.04699  

        **Breakfast**           -3.414      186.8      -0.01827    0.9855   

 **TotalSumCases:Breakfast**    1.021       0.5114      1.997      0.0498   
----------------------------------------------------------------------------


--------------------------------------------------------------
 Observations   Residual Std. Error   $R^2$    Adjusted $R^2$ 
-------------- --------------------- -------- ----------------
      72                324           0.1024      0.06277     
--------------------------------------------------------------

Table: Fitting linear model: NetSales ~ TotalSumCases + Breakfast + TotalSumCases:Breakfast

Looking at the breakfast row, the p-value (Pr(>|t|)) = 0.9855, which is greater than our significance level. This means that we fail to reject the null hypothesis. This also means that there is insufficient evidence to show that the y-intercepts ($\beta_2$) are different when the total number of cases is zero. This goes to show that the comparison is fair to make even though there is no statistical differenct between the two sections of Saturdays. 

The second hypothesis, $\beta_3$, the p-value for TotalSumCases:Breakfast is less than 0.05. This means that we reject the null hypothesis and that there is sufficient evidence to show that the new breakfast menu has helped to increase revenue during the pandemic. In other words, the breakfast menu is a success in Mrs. Powell's in Rexburg!


Another interesting observation is that once cases started to be recorded in Eastern Idaho Public Health (EIPH), net sales on Saturdays without breakfast dropped almost $0.83 on average per Covid-19 case. On the contrary, for every case that was being taken care of in EIPH area net sales on Saturdays with breakfast increased on average by roughly 19 cents. 

Additionally, this 

The following formula is the equation for the graph below. 

$$
 \overbrace{\hat{Y}_i}^\text{Net Sales(in dollars)} = 
  \underbrace{(730.2 + -3.414)}_\text{Green y-int = 726.786} + \underbrace{(-0.8295 + 1.021)}_\text{Green slope = 0.1915} \overbrace {X_\text{1i}}^ \text{Total Sum of Cases}
$$


```r
b <- coef(Bfast.lm)

plot(NetSales~TotalSumCases, data = Bfast.NetSales2, col = c("red" ,"green")[as.factor(Breakfast)], pch = 16)

legend("topleft", legend=c("Baseline: no breakfast menu", "Changed-line: breakfast menu"), bty="n", lty=1, col=c("red","green3"), cex=0.8)


curve(b[1] + b[2]*x, col="red", lwd=2, add=TRUE)  
curve((b[1] + b[3]) + (b[2] + b[4])*x, col="green3", lwd=2, add=TRUE)
```

![](BreakfastSaturdays_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

This graph shows how initial sales were effected by the pandemic, in red. It also shows the slow increase in net sales towards the end of last year and the early part of this year. 


#### Assumptions of a linear regression

It is important to look at the assumptions for the linear regression. If the assumptions are met then there is credence for what was analyzed. If not then a linear regression is not appropriate.


```r
par(mfrow = c(1,3))
plot(Bfast.lm,which = 1:2)
plot(Bfast.lm$residuals)
```

![](BreakfastSaturdays_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

First the graph on the right is to show that there is a linear relationship between net sales and total number of covid cases and if the variance in the points is constant. The linear test may be fulfilled but there is a spike in the middle of the data and then a decrease on both sides of the middle portion. No discernible pattern is desirable for the second assumption which there seems to be a pattern. This assumption then fails. 

Next the graph in the middle. It is best to have the data line up as much as possible across the dotted line. This shows, again, that the data is normal. For the most part, the data line up on the line. For this graph, it passes the third assumption. 

Finally, the graph on the right. This checks to see if the error terms, how far a data point is from it's corresponding line, are independent. It is best to have no clear pattern. This is clearly not the case. There is a sort of V shape to this graph. Therefore, this assumption is not met. 

Despite the plots not meeting the assumptions for the linear regression, an expert statistician said, "that the stability of the green dots is what makes [it] okay to use the regression." He further stated, "there is a more in-depth analysis that you could do to make the full regression more meaningful, but it will end up giving similar conclusions."
