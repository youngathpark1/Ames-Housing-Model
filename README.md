# Project 2 - Ames Housing Data and Kaggle Challenge


### Problem Statement

In this analysis, I set out to build a linear regression model that can predict home prices in Ames, Iowa. The train data set that came as part of the Kaggle Challenge had detailed records of 2050 home sales between 2006 - 2010 with 79 features describing both exterior and interior characteristics of each home.

Given the wide scope of features to examine and consider, some of the difficult challenges with this analysis was figuring out the best method to select which features to focus on, how to properly clean and transform the data in a way that is compatible to fit in a linear regression model, and how to properly interpret the findings.

In essence, the overacrching theme and goal of this analysis is to answer these two key questions:

* Which features have the most impact determining housing prices?
* Which model is most optimal to predict housing prices?

As a side note, I chose to specifically focus on R2, MSE, and RMSE linear regression metrics throughout the analysis to evaluate my model. While I could have measured and compared other metrics, I felt these three scores would be sufficient enough to decipher whether the model is performing at a high level. A closer attention was focused on RMSE as the Kaggle competition was measured based on RMSE.


### Data Dictionary

|Feature|Type|Data/Table|Description|
|---|---|---|---|
|**has_garage**|int|ames|Binary 1 or 0 if home has a garage|
|**has_pool**|int|ames|Binary 1 or 0 if home has a pool|
|**has_fireplace**|int|ames|Binary 1 or 0 if home has a fireplace|
|**has_porch**|int|ames|Binary 1 or 0 if home has a porch|
|**has_basement**|int|ames|Binary 1 or 0 if home has a basement|
|**Bsmt Bath**|int|ames|Sum of Bsmt Full Bath + 0.5 * Bsmt Half Bath|
|**Total Bath**|int|ames|Sum of Full Bath + 0.5 * Half Bath|
|**Alpha**|float|Metrics Summary|Optimal Alpha Score for Ridge, Lasso, ElasticNet|
|**R2**|float|Metrics Summary|Coeffcient of Determination|
|**MSE**|float|Metrics Summary|Mean Squared Error (Log Scale)|
|**RMSE**|float|Metrics Summary|SAT Root Mean Squared Error (Log Scale)|
|**MSE_exp**|float|Metrics Summary|Mean Squared Error (Exponentiated)|
|**RMSE_exp**|float|Metrics Summary|Root Mean Squared Error (Exponentiated)|


To preserve the naming convention of the original train data set, I did not alter the feature names in any way. Any new features that were engineered as interative terms are combinations of the features from the original train data set. Details for those features can be found in the [Data Dictionary for the Ames Housing Data.](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt)


## Summary
I applied three linear models to help resolve my low bias, high variance effect and as well as to address multicolinearity within my features. In all three models, my results improved drastically across all metrics and all outperformed the baseline, which indicates the model is predicting with some high level of accuracy beyond just randomly picking predictions.

R2 scores between the train and test data were in close proximity, in the 0.2 range. MSE and RMSE metrics (both log and exponentiated) were pretty similar across all three models as well. There seems to be a pretty good balance between bias and variance as well given that while the metrics for the train data for all three models are higher, the difference isn't extreme. Models may be slightly overfit but given the model's performance prior to regularization, this is certainly a huge improvement.


Coefficients are harder to interpet given they have been scaled. However, the relationship to the target variable is still preserved and interpretable in terms of correlation strength. For instance, interative feature Year Remod/Add_Gr Liv Area has the highest coefficient of 0.06 followed by a series of other featuers related to either condition of the home, age of the home (when it was built), area/living space, and etc. These features seem to jive with common sense since factors like sq ft, number of bedrooms, quality of the home, age of the home are all important featuers that a typical homebuyer looks for.

Lastly, across all three models, assumptions required for linear regressions seem to be held. Linearity between some of the features and sale price has already been observed during EDA portion of the analysis. Independence is a bit subjective depending on how you look at it. While a sale price of one home doesn't necessary directly impact the sale price of another, there is usually some connection because real estate markets are evaluted based on proximity to certain locations (town/county, which street, school district, and etc.). However, for the purpose of this analysis, I think the independence assumption is reasonable to make. For normality of error, there is a fairly decent normal distribution of residuals.

One issue could be homoscedasticity. All three models seem to predict middle of the pack with more level of accuracy. While the presence of heteroscedasticity ceratinly does not mitigate the application of linear regression, it is an indication that there are still some fine-tuning that may be necessary.


## Conclusions and Recommendations
In this analysis, using the Ames housing prices data, I was able to build a linear regression model that can predict housing prices in Ames, Iowa with certain level of predictive power.

To set the stage for linear regression, I went through a round of feature selection and feature engineering to best strengthen signal and reduce noise.

I applied Ridge, Lasso, and ElasticNet to measure and compare their perfomance in R2, MSE, and RMSE. In terms of performance, Lasso performed the best with unseen test data. With Lasso, the model produced R2 score of 0.90 with unseen data. This essentially means that 90% of variability in sale prices around the mean can be explained based on features used in the model. To understand which features have the most significance, I found that the interactive term 'Gr Liv Area and Year Remod/Add' had the largest coefficient, followed by other interative terms related to the size, quality, age, and number of bedroom/bathrooms which seem to jive with common sense.


There are definitely additional room for improvements and enhancements. Here are some possible considerations that can help to increase performance and predictiability:

* Feature Engineering:
    
    - My feature selection and engineering process in this analysis was just one from many different options that I could have explored. Alternative route I could havek is to perhaps narrow down the list of feature early on in the process using a threshold such as R2 >0.50 but this required the assumption that other features with correlation less than 0.50 do not have predictive power when coupled with other features. I think it may be worth to revisit this assumption and try using a different thresholds early on in the process to potentially elminate noise as early as possible.


* Additional data:

    - There are many other features that could have been layered in outside of the data that was provided. Variables such as public safety of the neighborhood, public school ratings, environmental factors, property tax, and many others also contribute greatly to housing prices. By layering in industry-specific features, the model can definitely be improved and make better predictions.


* Ordinal variables:
    - I applied a hybrid approach with ordinal variables. Instead of convering all of them to numeric variables, I selectively chose only a handful or ordinal variables that I believed displayed "true" order in light of sale price. For others, I chose to conver tthem to dummy variables. To get more predictive power out of ordinal variables, perhaps I could have applied more industry-wide accpeted scale of rating rather than arbitrary choosing one. For instance, "Excellent" in the real estate market may carry more magnitute and impact and should weigh much more than "Good" or "Average". If there is a standarized scale that is used in the industry, that may have added more value and insight.
    
 
 * Generalization:
    - This model was created using housing prices data in Ames, Iowa. If I were to apply this model to predict housing prices in other geographic locations, this model most likely will not perform as well. To make the model scalable, it will require further data and complexity such as housing prices variance across different parts of the country, specific housing market trends, and etc. 
    
    
 * Heteroscedasticity:
    - There is an evidence of hereoscedasiticy across all three models. Ideally, the ideal goal would be to achieve homoscedasticity where there is equal variance of error across all predictions. While hereoscedasticy doesn't mitigate the usage of linear regression model, it is an indication that that are some more fine tuning that needs to be done. One potential recommendation here would be to explore which feature has a smilar behavior as the target variable in terms of skewedness and apply log transformation to account for those characteristics.
    
 
    
Lastly, for dsir-1019 Kaggle competition, my final submission of ElasticNet performed the best with RMSE score of 22,687.22.