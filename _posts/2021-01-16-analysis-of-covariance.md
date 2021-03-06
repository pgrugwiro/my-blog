---
layout: post
title: "ADFS: Analysis of Covariance"
date: 2021-01-16 19:50:22 +0000
always_allow_html: TRUE
output:
  md_document:
    variant: gfm
    preserve_yaml: TRUE
categories: jekyll update
image: /assets/feature_images/post3.jpeg
permalink: /:title:output_ext
---

Statistics is the true backbone of data science. The recent
technological advancements in computational infrastructures and data
analytics tools have revolutionized the way we carry out data analysis,
however the fundamental statistical principles remain the foundation of
everything data.

Today, we discuss a popular statistics topic: Analysis of Covariance,
commonly known by its acronym, ANCOVA.

ANCOVA is a linear model that contains at least one covariate
(continuous independent variable) and at least one categorical variable.
Since the response variable is continuous, Ancova could be seen as a
combination of a regression analysis (with respect to the continuous
independent variable(s)) and analysis of variance (with respect to the
categorical variable(s)). Another way to put this is that when we run
ANCOVA, we are running regression analyses for each level in the
categorical variable(s).

For example, imagine we are trying to determine the relationship between
the weight as a function of diet and age. In this example, our outcome,
weight, is a continuous variable and the two independent variables are
diet and age. Diet is a categorical variable, while age is a continuous
variable. In this scenario, ANCOVA is used to determine the function:
<p style = "text-align: center">
<i>weight = f(diet, age)</i>
</p> 

The linear model seeks to establish a
relationship between the response variable (or outcome) and the
independent variables (or inputs). A model that can describe this
relationship could potentially be used to make accurate data driven
decisions and to predict outcomes.

<br>
#### **ANCOVA in Practice**

This morning, as I was catching up with my mother, who is currently
sheltering at home with the rest of her household due to her recent
Covid-19 positive diagnosis, I got to learn that my 10 year old nephew
who lives with her has recently picked up a new hobby. I was very
concerned for him that he will become very frustrated and irritated by
his inability to leave home, to go to school and to be around other
children for the next couple of weeks. I was told not to worry about him
since he’s spending most of his time paying proper attention to his
young 10 chickens. He used to bother his parents about money to buy more
airtime and browse social media, but now he’s bothering them for money
to buy more chickenfeed. I was very moved by this young entrepreneur. He
told me that he’s only worried about lagging behind on computer lessons
from school and I promised him that he will get personal tutoring from
me. Now, in honor of my nephew’s chicken farming empire, let’s see
ANCOVA in practice by using a built-in dataset in R, **ChickWeight**, an
experiment on the effect of diet on the growth of young chicks.

``` r
data("ChickWeight")
```

The dataset contains four groups of chicks on different diets. Their
weight measurements (grams) were taken at birth and every other day
after until day 21.

Dataset preview:

<table class=" lightable-classic-2 table" style="font-family: Arial; width: auto !important; margin-left: auto; margin-right: auto; margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:right;">

weight

</th>

<th style="text-align:right;">

Time

</th>

<th style="text-align:left;">

Chick

</th>

<th style="text-align:left;">

Diet

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

42

</td>

<td style="text-align:right;">

0

</td>

<td style="text-align:left;">

1

</td>

<td style="text-align:left;">

1

</td>

</tr>

<tr>

<td style="text-align:right;">

51

</td>

<td style="text-align:right;">

2

</td>

<td style="text-align:left;">

1

</td>

<td style="text-align:left;">

1

</td>

</tr>

<tr>

<td style="text-align:right;">

59

</td>

<td style="text-align:right;">

4

</td>

<td style="text-align:left;">

1

</td>

<td style="text-align:left;">

1

</td>

</tr>

<tr>

<td style="text-align:right;">

64

</td>

<td style="text-align:right;">

6

</td>

<td style="text-align:left;">

1

</td>

<td style="text-align:left;">

1

</td>

</tr>

<tr>

<td style="text-align:right;">

76

</td>

<td style="text-align:right;">

8

</td>

<td style="text-align:left;">

1

</td>

<td style="text-align:left;">

1

</td>

</tr>

<tr>

<td style="text-align:right;">

93

</td>

<td style="text-align:right;">

10

</td>

<td style="text-align:left;">

1

</td>

<td style="text-align:left;">

1

</td>

</tr>

</tbody>

</table>

The boxplot of the data shows that diet 3 is slightly better than diet
4, and that diet 2 is slightly better than diet 1. ANCOVA analysis helps
determine if the true values of observed differences and whether or not
they are statistically significant.

<img src="/rmd_images/2021-01-16-analysis-of-covariance/unnamed-chunk-4-1.png" style="display: block; margin: auto;" />

<br>
#### **Running ANCOVA in R**

Running ANCOVA in R is very simple and straightforward.

``` r
ancova_model <- lm(weight ~ Time+Diet, data = ChickWeight)
```

In the model above, we assumed no significant interactions between the
independent variables.

To view the results of the model, we call the summary function on the
model:

``` r
summary(ancova_model)
```

    ## 
    ## Call:
    ## lm(formula = weight ~ Time + Diet, data = ChickWeight)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -136.851  -17.151   -2.595   15.033  141.816 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  10.9244     3.3607   3.251  0.00122 ** 
    ## Time          8.7505     0.2218  39.451  < 2e-16 ***
    ## Diet2        16.1661     4.0858   3.957 8.56e-05 ***
    ## Diet3        36.4994     4.0858   8.933  < 2e-16 ***
    ## Diet4        30.2335     4.1075   7.361 6.39e-13 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 35.99 on 573 degrees of freedom
    ## Multiple R-squared:  0.7453, Adjusted R-squared:  0.7435 
    ## F-statistic: 419.2 on 4 and 573 DF,  p-value: < 2.2e-16

<br>
#### **Model interpretation**

  - the low p\_value of 2.2e-16 indicates that the model itself is very
    significant.
  - the low p\_values in the table indicate that each parameter in the
    model is statistically significant.
  - the R-squared value of 0.7453 indicates that the model explains
    nearly 75% of the variations observed in the data, which is very
    good.

What of the rest of the numbers in the table?

Earlier, we mentioned that when we run ANCOVA, we are in fact running
regression analyses for each level in our categorical variables. In our
example, we have 4 levels (diets) in our Diet variable. Therefore, we
are running 4 regression analyses and require 4 intercepts and 4 slopes.

Starting from the top (Intercept), the estimated value of 10.9244
indicates the intercept for the graph of chicks weight against time for
Diet 1. On row 3, the estimate value of 16.1661 is used to determine the
intercept for Diet 2. The actual intercept for the graph of chicks
weight against time for Diet 2 is 16.1661 + 10.9244 = 27.0905. On row 4,
the we get the intercept for Diet 3 as 36.4994 + 10.9244 = 47.4238. On
row 5, the intercept for Diet 4 is 30.2335 + 10.9244 = 41.1579.

All 4 graphs for diets share a common slope of 8.7505 shown on row 2.

<p style = "text-align: center">

<i>weight<sub>diet1</sub> = 10.9244 + 8.7505 * Time</i>
<br><i>weight<sub>diet2</sub> = 27.0905 + 8.7505 * Time</i>
<br><i>weight<sub>diet3</sub> = 47.4238 + 8.7505 * Time</i>
<br><i>weight<sub>diet4</sub> = 41.1579 + 8.7505 * Time</i>

</p>


<img src="/rmd_images/2021-01-16-analysis-of-covariance/unnamed-chunk-7-1.png" style="display: block; margin: auto;" />

There we have it. My nephew will be advised to use Diet 3 for his
chickenfeed.

This is a simplified version of ANCOVA in R. In this example, we only
had one continuous independent variable and one categorical variable
with 4 levels. We ran what is called a One-Way ANCOVA. In the real
world, we encounter datasets with multiple covariates and we run
Multi-Way ANCOVA. In addition, we may experience interactions between
independent variables. Example, imagine if Diet 2 works better from day
15 compared to days prior. That would be an indication that there exists
an interaction between the Diet variable and the Time variable. In our
exercise, we assumed that no such interactions exist. However, R
provides means to conduct analyses with interactions.
<br>

#### **Predicting with the model**

Our model above explains 75% of the variations. We can confidently use
it to make predictions of the weight of chicks.

Let’s say for example that we learn of a chick that was not part of the
experiment, i.e. her weight has not been recorded but we know she’s been
on Diet 3 the entire time. We have an improvised inspection and are
required to provide the weight values of all chicks at 13 days old.
Since we didn’t measure the weights on day 13, we feel comfortable
providing the weights from day 12 for all chicks. What do we do with
this newly discovered chick? One acceptable way would be to estimate the
weight average of all chicks on Diet 3 on day 12 and record that
measurement for this new chick. This would be:

``` r
mean(ChickWeight[ChickWeight$Time == 12 & ChickWeight$Diet == '3',]$weight)
```

    144.4 grams. 



Another acceptable way, and perhaps more intelligent way, would be to
use our model to estimate/predict the weight of this new chick.

``` r
new_chick <- data.frame(Time = 12, Diet = '3')
predict(ancova_model, new_chick)
```

``` 
152.4 grams.  
```



Even though the numbers are within close range, we are likely to be more accurate by using the model estimate than
average measurement.

As I write this, my mother is in good spirits, she’s determined to fight
off the Covid. She still can’t believe she caught the virus despite
having meticulously taken all the precautions. She’s known in her
village as a mask-evangeliser. This truly shows the vicious nature of
this virus and we must all do what we can to stop the spread. I will
report later on the progress of my nephews chicken empire.
