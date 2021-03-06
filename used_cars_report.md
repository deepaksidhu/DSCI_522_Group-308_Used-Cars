---
title: "Predicting Used Car Prices"
author: "Andrés Pitta, Braden Tam, Serhiy Pokrovskyy </br>"
date: "2020/01/25 (updated: 2020-01-30)"
always_allow_html: yes
output: 
  github_document:
    toc: true
bibliography: used_cars.bib
---







# Summary

In this project we attempt to build a regression model which can predict the price of used cars based on numerous features of the car. We tested the following models: support vector regression, stochastic gradient descent regression, linear regression, K-nearest neighbour regression, and random forest regression. data set is not linearly separable, more clustered. We found that support vector regression had the best results, having a score of `INPUT TRAIN SCORE` on the training set and a score of `INPUT TEST SCORE` on the test set. Given that the dataset was imbalanced, this led to poor prediction of the classes that were quite sparse because the model was not able to learn enough about those classes in order to give good predictions on unseen data. 

1.360594 &times; 10<sup>4</sup>


# Introduction

Websites such as Craigslist, Kijiji, and eBay have tons of users that create a wide array of used good markets. Typically people looking to save some money use these website to purchase second hand items. The problem with these websites is that the user determines the price of their used good. This can either be a good or bad thing, depending on whether or not the user is trying to scam the buyer or give the buyer a good deal. For the average individual who is not familiar with prices of the used market, it is especially difficult to gauge what the price of a used good should be. Being able to predict used car prices based on data on a whole market will gives users the ability to evaluate whether a used car listing is consistent with the market so that they know they are not getting ripped off. 

# Methods

## Data
The data set used in this project is Used Cars Dataset created by Austin Reese. It was collected from Kaggle.com [@reese_2020] and can be found [here](https://www.kaggle.com/austinreese/craigslist-carstrucks-data). This data consists of used car listings in the US scraped from Craigslist that contains information such as listed price, manufacturer, model, listed condition, fuel type, odometer, type of car, and which state it's being sold in. 

## Analysis
As it was mentioned, our original data holds half a million observations with a few dozen features, most categorical, so accurate feature selection and model selection were extremely important. Especially because model training took significant amount of computational resources.

Since we could not efficiently use automated feature selection like RFE or FFE (because of time / resources constraint), we had to perform manual feature selection. As we had some intuition in the target area as well as some practical experience, we were able to prune our feature list to just 12 most important on our opinion:

- 10 categorical features:
    - manufacturer (brand)
    - transmission type
    - fuel type
    - paint color
    - number of cylinders
    - drive type (AWD / FWD / RWD)
    - size
    - condition
    - title_status
    - state

- 2 continuous features:
    - year
    - odometer
    
<img src="../results/figures/manufacturer.png" title="Figure 2. adfansdflansdfk" alt="Figure 2. adfansdflansdfk" width="80%" />

<img src="../results/figures/map_price.png" title="Figure 2." alt="Figure 2." width="80%" />

<img src="../results/figures/corrplot.png" title="Figure 2." alt="Figure 2." width="80%" />
 
 
For each model we performed 5-fold-cross-validated grid search involving a range of most important model-specific hyper-parameters.

The R and Python programming languages [@R; @Python] and the following R and Python packages were used to perform the analysis: docopt [@docopt], knitr [@knitr], tidyverse [@tidyverse], readr [@readr] docopt [@docoptpython], altair [@Altair2018], plotly [@plotly], selenium [@seleniumhq_2020], pandas [@mckinney-proc-scipy-2010], numpy [@oliphant2006guide], statsmodel [@seabold2010statsmodels]. scikit-learn [@sklearn_api]. 

The code used to perform the analysis and create this report can be found [here](https://github.com/UBC-MDS/DSCI_522_Group-308_Used-Cars)

# Results & Discussion


Based on our EDA and assumptions, we picked a number of models to fit our train data. Since training and validating took a lot of resources, we performed it on a gradually increasing subsets of training data in the hopes that we find an optimal trade-off between computational time and model performance. See the results below, sorted by validation score (increasing):

Model | Training Score | Validation Score
--- | --- | ---
Linear Regression | 0.555803 | 0.526354
Stochastic Gradient Descent | 0.550439 | 0.528612
kNN | 0.638008 | 0.626848
Random Forests | 0.964447 | 0.734342
Gradient Boosted Trees | 0.803595 | 0.736818
**Support Vector Machines** | **0.840271** | **0.813724**

Since SVM shown the best results from the very beginning, we performed a thorough adaptive grid search on a bigger subset of 200,000 observations (running for 4 hours) resulting in 81.3% accuracy on validation data. Eventually we ran the model on the **test data** containing more than 40,000 observations, which confirmed the model with even better **accuracy of 81.5%**. The good sign was also that it did not overfit greatly on train set, which was a good sign to perform further testing. 

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> odometer </th>
   <th style="text-align:left;"> manufacturer </th>
   <th style="text-align:left;"> condition </th>
   <th style="text-align:left;"> title_status </th>
   <th style="text-align:right;"> price </th>
   <th style="text-align:right;"> prediction </th>
   <th style="text-align:right;"> abs_error </th>
   <th style="text-align:right;"> abs_error_pct </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 89662 </td>
   <td style="text-align:left;"> lincoln </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 13995 </td>
   <td style="text-align:right;"> 13605.94 </td>
   <td style="text-align:right;"> 389.06 </td>
   <td style="text-align:right;"> 2.78 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 143724 </td>
   <td style="text-align:left;"> ford </td>
   <td style="text-align:left;"> good </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 6588 </td>
   <td style="text-align:right;"> 8246.61 </td>
   <td style="text-align:right;"> 1658.61 </td>
   <td style="text-align:right;"> 25.18 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 35370 </td>
   <td style="text-align:left;"> ford </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 27990 </td>
   <td style="text-align:right;"> 30395.27 </td>
   <td style="text-align:right;"> 2405.27 </td>
   <td style="text-align:right;"> 8.59 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 157000 </td>
   <td style="text-align:left;"> dodge </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 2500 </td>
   <td style="text-align:right;"> 2780.30 </td>
   <td style="text-align:right;"> 280.30 </td>
   <td style="text-align:right;"> 11.21 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 88564 </td>
   <td style="text-align:left;"> toyota </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:left;"> rebuilt </td>
   <td style="text-align:right;"> 19900 </td>
   <td style="text-align:right;"> 11858.45 </td>
   <td style="text-align:right;"> 8041.55 </td>
   <td style="text-align:right;"> 40.41 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 199683 </td>
   <td style="text-align:left;"> jeep </td>
   <td style="text-align:left;"> good </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 3500 </td>
   <td style="text-align:right;"> 2957.85 </td>
   <td style="text-align:right;"> 542.15 </td>
   <td style="text-align:right;"> 15.49 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 39026 </td>
   <td style="text-align:left;"> chevrolet </td>
   <td style="text-align:left;"> excellent </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 23400 </td>
   <td style="text-align:right;"> 24538.64 </td>
   <td style="text-align:right;"> 1138.64 </td>
   <td style="text-align:right;"> 4.87 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 178450 </td>
   <td style="text-align:left;"> hyundai </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 4950 </td>
   <td style="text-align:right;"> 4859.28 </td>
   <td style="text-align:right;"> 90.72 </td>
   <td style="text-align:right;"> 1.83 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 66765 </td>
   <td style="text-align:left;"> dodge </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:right;"> 22900 </td>
   <td style="text-align:right;"> 23366.48 </td>
   <td style="text-align:right;"> 466.48 </td>
   <td style="text-align:right;"> 2.04 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 203516 </td>
   <td style="text-align:left;"> gmc </td>
   <td style="text-align:left;"> No value </td>
   <td style="text-align:left;"> clean </td>
   <td style="text-align:right;"> 9988 </td>
   <td style="text-align:right;"> 9497.48 </td>
   <td style="text-align:right;"> 490.52 </td>
   <td style="text-align:right;"> 4.91 </td>
  </tr>
</tbody>
</table>


# Further Directions

To further imrpove the accuracy of this model we can aleviate the problem of imbalanced classes by grouping manufacturers by region (American, Germnan, Italian, Japanese, British, etc.) and status type (luxery vs economy). 

Although we achieved a nice accuracy of 81.5%, we can now observe some other metrics. Eg., having an RMSE almost twice higher than MAE suggests that there is a good number of observations where the error is big (the more RMSE differs from MAE, the higher is the variance) This is something we may want to improve by finding features and clusters in data space that introduce more variance in the predictions. Eg. the model predicting clean car price may greatly differ from the model predicting salvage (damage / total loss) car price. This comes from getting deeper expertise in the area, and we will try to play with this further more.

We may also want to use a different scoring function for our model - eg. some custom implementation of MSE of relative error, since we have high variance of price in the original dataset.

Lastly, due to time / resources limitations we only trained the model on half the training data, so we should try to run it on all training data and see how this changes our model (this would take approximately 16 hours). So far we have only seen improvements to the score as we increased the sample size.

The ultimate end goal is to eventually create a command-line tool for the end-user to interactively request vehicle details and output expected price with a precision interval.


# References
