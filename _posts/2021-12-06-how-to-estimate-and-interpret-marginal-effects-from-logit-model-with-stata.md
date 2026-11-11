---
layout: post
title: "How to estimate and interpret marginal effects from the logit model with STATA?"
date: 2021-12-06
permalink: "/how-to-estimate-and-interpret-marginal-effects-from-logit-model-with-stata/"
categories:
  - "Blog"
tags:
  - "Stata"
  - "logit"
  - "margins"
  - "probability"
  - "estimate"
  - "logistic"
  - "data anlysis"
---


**Logistic regression**, also known as **logit regression**, **logit model**, or just **logit**, is one of the most regression analyses taught at universities and used in data analysis. It is a non-linear model which predicts the outcome of a categorical dependent variable with respect to a vector of independent variables. When performing a logit regression with a statistical package, such as Stata, R or Python, the coefficients are usually provided by log-odds scale. In short, this means that point estimates are complicated to interpret, however the sign and the confidence interval of estimates can be interpreted. For that reason, it is interesting to interpret the logit model in the probability scale, i.e. as probabilities.

In this post, I will **explain how to compute logit estimates with the probability scale with the command `margins` in *`STATA`***. This allows getting the point estimates interpretable as probabilities or margins and are easier to interpret. For clarity, I will use a binary dependent variable (binary logit model) and focus only on one independent variable. However, the same can be done for several independent variables, all of them, or for a categorical dependent variable with more than two values.

As a non-linear estimator, the relation between a given independent variable and the dependent variable is not linear. The expected change in a probability depends on the value of the independent variable of interest and the values of the other independent variables. Therefore, besides having clear how to code it in `STATA`, it is essential to understand what to analyse and how to interpret it. To sum up, I will explain how to obtain:

1.  The predicted margin or probability at a specific value (or values) of an indepenent variable
2.  The average marginal effect of an indepenent variable
3.  The marginal effect of one independent variable at the means of the other independent variables

## 0) Example: load the database and regress the model

Let's start with an example to see this. First, load the following dataset from the Stata webpage. This is a subset of the National Longitudinal Survey, and it contains socioeconomic variables from young women who were 14-46 years old over the period 1968-1988.

```stata
webuse nlswork.dta, clear
```

Imagine that we want to predict whether a woman in the database has earned a college graduate (binary dependent variable) depending on the age, the race, whether she lives at the city centre, whether she lives in the south and year of the interview fixed-effects (FE) with robust standard errors (I know this regression is really simple, but it is just to take an example). The Stata code to perform this regression would be:

```stata
logit collgrad age i.race i.c_city i.south i.year, robust
```

However, in the case of applying the command `margins` is crucial to indicate whether each independent variable is discrete or continuous. In this way, `STATA` will compute the margins correctly. In case of not telling it, `STATA` will assume the independent variable as continuous.

**Important note:** in this dataset the variable age is defined as a discrete variable (a discrete jump of one year). However, I will treat it as a continuous variable. I hope that anyone gets upset with that :).

## 1) The predicted margin or probability at a specific value or values

First of all, we might be interested in obtaining the predicted probability of being graduated at college at specific value of age. For example, at 30 years old or 40 years old (independently):

```stata
margins, at(age=30)
margins, at(age=40)
```

Therefore, the predicted probability that a 30-years-old woman has a college graduate is *0.165* and for a 40-years-old woman is *0.218*. It makes sense that the predicted probability is higher at 40 years old than at 30. In each case, the margins are computed at the value of the variable age indicated and the other covariates set to their observed values. Also, one might be interested in knowing the predicted probability along with the age distribution; this is for several ages. For instance, at ages 25, 30, 35, 40 and 45:

```stata
margins, at(age=(25(5)45))
```

The output gives the predicted probability for each age indicated and, the higher is the age, the higher is the predicted probability.

Moreover, values from different independent variables can be indicated at the same time. For instance:

```stata
margins, at(age=25 race=2)
```

In this case, the predicted probability that a black 25-years-old woman has a college graduate is *0.0997* . Again, with the other covariates set to their observed values.

## 2) The average marginal effect

Given a continuous independent variable, the marginal effect of a change (partial derivative) varies along with this variable distribution (remember the non-linearity of the logit function). Thus, the coefficient, which indicates the relationship between the dependent variable and the independent variable, may vary along with the distribution of the independent variable. In our case, it might be interesting to get the partial derivative of the variable age or, in other words, the marginal effect. This allows understanding how a change in the variable age (one more year) affects the expected probability of having a college graduate. This average marginal effect is computed as the average of all the marginal effects from each observation in the sample and the code is as follows:

```stata
margins, dydx(age)
```

This output, *0.005*, indicates that with an increase of one year in the age of a woman (in the model stated before), the probability of having a college graduate increases *0.005* percentage points. Coming back to the predicted probabilities, an approximation of the marginal effect can be seen in the following way (just as a way to know how this works):

```stata
margins, at(age=(30(1)35))
```

Given these six predicted probabilities, we can check that by subtracting the predicted probability at a given age with that of the previous age; we get around 0.005. This is, the marginal effect of increasing one year the age of a woman. For instance, *Pr(college graduate \| 31) - Pr(college graduate \| 30) = 0.0048* or *Pr(college graduate \| 35) - Pr(college graduate \| 34) = 0.0051*.

More interesting, we can **estimate the same model by OLS** and perform the same exercise:

```stata
reg collgrad age i.race i.c_city i.south i.year, robust
margins, dydx(age)
margins, at(age=(30(1)35))
```

Then, we can get the following take home messages:

- The coefficient `age` is the same as the marginal effect in `margins, dydx(age)`.
- This marginal effect is similar to the logit one, but not equal; small differences arise.
- The differences between the predicted probabilities given in `margins, dydx(age(30(1)35)` are exactly the same than the coefficient `age` and the `margins, dydx(age)`. This is due to the linearity assumption of the OLS.

## 3) The marginal effect at the means

We might also be interested in obtaining the marginal effect of a given covariate when the other independent variables have their values at their means. Before, in the average marginal effect, the other covariates were set as their observed values, while now they are set at the sample mean. In our example, this is how the probability of having a college graduate changes when the age of a woman increases by one year and all the other independent variables have the values equal at their means. In some way, this is the marginal effect of an "average" woman in our sample.

```stata
margins, dydx(age) atmeans
```

------------------------------------------------------------------------

Finally, note that the `margins` command offers more options and functions. I invite you to keep "playing" with this sample and model in order to learn more about this fascinating command. For any comment or feedback, please don't hesitate to write a comment below or send me an e-mail.
