---
title: Cross-Sectional Models
notebook: cross_sectional_models.ipynb
nav_include: 3
---

## Contents
{:.no_toc}
*  
{: toc}



## Model 1: Prediction using all baseline features

We began by developing a basic multiclass logistic regression model to predict baseline diagnosis from data available at that initial visit to get a rough understanding of the strength of the available predictors, as well as to compare how well they perform in predicting cross-sectional diagnosis versus longitudinal change in diagnosis.

The one-versus-all logistic regression classifier achieves a training score of 80.6% and a test score of 78.3% (based on a 25% train-test split stratified by diagnosis). The confusion matrices for the training and test sets show that the model does a good job discerning between the three classes, without always defaulting to -- though clearly biased toward -- the dominant group. (48.8% of observations were classified as MCI, 28.2% as CN, and 23.0% as AD).




















    (0.80623973727422, 0.7832512315270936)












![png](cross_sectional_models_files/cross_sectional_models_11_0.png)


## MODEL 2 - Prediction only using MMSE and CDRSB

Rather than predicting baseline diagnosis with all available predictors, we also sought to determine the predictive accuracy of two cognitive tests -- the MMSE (Mini-Mental State Examination) and CDRSB (Clinical Dementia Rating Scaled Response) -- that our EDA suggested correlate highly with baseline diagnosis. Importantly, cognitive tests such as the MMSE and CDRSB are very low-cost to administer, and are therefore the first method typically used to identify cognitive decline among patients. This model therefore serves to measure the misdiagnosis rate liable to occur by using these low-cost methods, and to identify any systematic biases that they may suffer from.

This logistic model was therefore purposefully rudimentary, performed without any regularization or boosting. Nevertheless, it achieves a strikingly high training score of 93.4% and a test score of 92.6%. A confusion matrix of its predictions demonstrates the ease with which the three classes are distinguished using just two predictors. Noteably, this parsimonious model performed significantly better than the model with all readily-available predictors that was considered above (not just on the test set, which would have suggested overfitting, but surprisingly on the training set as well), and generated predictions that were less biased toward the dominant MCI class than the previous model as well.

The decision boundaries of this model are illustrated below (using jitter to illustrate density of points belonging to the CN class), and show that there is clear separability between the three classes based on MMSE and CDRSB scores.



```python
X_train2 = X_train[['MMSE', 'CDRSB']]
X_test2 = X_test[['MMSE', 'CDRSB']]
baseline_logreg = LogisticRegression(solver='liblinear', multi_class='ovr', C=100000).fit(X_train2, y_train)
#baseline_logreg.score(X_train2, y_train), baseline_logreg.score(X_test2, y_test)
```





    (0.9343185550082101, 0.9261083743842364)








![png](cross_sectional_models_files/cross_sectional_models_14_0.png)







![png](cross_sectional_models_files/cross_sectional_models_15_0.png)




```python

```