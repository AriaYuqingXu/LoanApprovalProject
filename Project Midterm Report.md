# Project Midterm Report
--- 
From: Jing Zhang (wz223), Qiwen Li (ql257), Yuqing Xu (yx486)
To: Professor  Udell
Re: ORIE 4741 Project Midterm Report 
Date: 11/1/2021


## Project Question and Description
Credit scores help banks, loaners, and others justify or evaluate a person’s ability to return their debt, enabling them to make the right decisions in loan approval and helping disadvantaged individuals/groups to get the loan approved. Loan evaluation would help in minimizing loan risk, maximizing banks’ profits, and protecting margins. Our project aims to evaluate the fairness of the current loan system at Bank of America across the United States in 2018 with data provided by HMDA Data Browser. Another goal is to develop a new system that is fair, and it will penalize certain unfair features towards loan approval.

## Avoid over (and under-) fitting
Overall, to avoid overfitting/underfitting, we have split up the dataset into the training and testing set. We will apply validation methods such as examining MSE in training and testing data.
Considering that we have a large number of features, we are less concerned with underfitting. By looking at loan applications from all over the country, we are able to obtain a large sample size, and the large number of features would increase the model’s complexity, which prevents overfitting. Furthermore, we have selected certain models and methodology in our Exploratory Analysis to prevent overfitting.


## Test the effectiveness of the models you develop:
We are planning to use a validation set, k-fold-validation specifically to test effectiveness. We also plan to compare MSE, p-values, and R-squared values for different models to choose the optimal model.

## Data Cleaning and Transformation
When we first examined our dataset, among the 99 features, we decided to first manually remove 63 features that we considered as irrelevant or duplicate information based on the feature definition and possible values of each feature presented on the data field [website](https://ffiec.cfpb.gov/documentation/2018/lar-data-fields/).
Next, we convert the data type for categorical features into string type values in order to input them into different algorithms in the future. Then, we treat categorical features that contain only two possible values as boolean values corresponding to the values of 0 and 1. For categorical features with multiple values, we applied one hot encoding. Furthermore, we decided to drop some of the outliers that we have identified in our data exploratory phase and also looked through the columns with missing values.

 **Columns with Missing Values**
<p align="center">
<img src=images/missing.png width="250" height="120">
</p>

we decided to drop some of the insignificant rows that are missing in value in various columns, like conforming_loan_limit, loan_term, property_value, debt_to_income_ratio, and applicant_race-1, since there are less than 2% of our data don’t have these values. For missing values in the applicant_age_above_62 column, we considered them as “not applicable” because the other two values are 1(yes) and 2(no) so we replaced the missing ones with value 3, indicating “not applicable”. Lastly, we realized that the income column is also partially absent. So we created another feature, ‘missing_income’, that states if the value in the income column is missing which we thought would be interesting to examine later on. In order to fix the problem of missing income, we looked at correlations between other variables and income and we found out that loan amount is the highest correlated variable. With the loan amount, we applied the linear regression model to predict the missing income values based on the loan amount. 
For our target feature, “action taken”, which tells whether the loan application is approved or not, we transform the feature value to 1 if it’s approved, and 0 otherwise.


### Number of features and examples presented
Now, the dataset contains documentation of 371,930 samples of loan applications with 31 features that we determined as valuable and insightful in determining our loan approval models. Some examples of the features are derived_race, debt_to_income_ratio, loan_amount, and income. 


## Histogram and boxplots for various important features about the dataset
In order to better understand our dataset, we created data visualizations regarding some of the relevant features that could be insightful. We constructed boxplots to see the spread of the data based on different features by loan approval status with the percentile distribution; it helps us identify outliers which were removed in later data-cleaning processes.


<p align="center">
<img src=images/loan.png width="550" height="190">
<img src=images/income.png width="550" height="190">
<img src=images/property.png width="550" height="190">
</p>

The above Box Plot confirms the presence of outliers/extreme values, which indicates the loan amount, income, and property_value disparities among all the loan applications.

Further more, we would like to investigate if there are bias hidden in the loan approval algorithms by plotting histograms to see the variability of the numbers of loan applications that are approved or disapproved based on some features that we considered as bias which are age, race, sex, and ethnicicity. Thus, we used Histogram to see the variability of the numbers of loan applications that are approved or disapproved based on some features that we considered as bias which are age, race, sex, and ethnicicity. 

<p align="center">
<img src=images/hist1.png width="550" height="200">
</p> 

In the first histogram, we have "1" stands for "Male", "3" stands for "Female", and "2" stands for "Joint." We can see that that the proportion of male(1) applicants who got approved is higher than the proportion of female(3) applicants who got approved, which indicates that the current system might have a gender bias.
Besides, the second histogram conveys that the current loan approval system favors younger customers more than those older, especially those whose ages are greater than 74, which indicates that age discrimination might exist in the current system.

<p align="center">  
<img src=images/hist2.png width="500" height="270">
</p>

The third histogram shows that the system favors Not Hispanic or Latino since the proportion of Not Hispanic or Latino applicants who got approved is significantly higher than that of others, which tells us that ethnicity discrimination is not unlikely to happen in the current loan approval system.
From the fourth histogram, we spot a large difference between numbers of loan approved and disapproved for white applicants, while small differences for other races. It shows that racism might be a factor in the loan approval system.

Overall, the number of applications approved is lower for underrepresented groups among each classification of sex, age, race, and ethnicity. According to the histograms, the loan approval system appears to favor male, younger customers, and people who are not Hispanic or Latino. 


## Exploratory Data Analysis (preliminary analyses)

### Analyses Overview:
We are exploring the most correlated features associated with the target feature, and investigating whether those features are significant by performing regression (linear regression and logistic regression), and random forest. We split the features into grouped features with real-valued, booleans, and categorical features. After data preprocessing, we divided the dataset into two parts, with 80% of the data as the training set and the rest 20% as the test set.  We used linear regression for features with numerical values, and we used logistic regression on features with categorical values and boolean values. We performed random forest to further investigate the top 15 important features of loan approval.white, and younger customer base.

#### Linear Regression
A model that we have implemented is Regression, we applied linear regression to real value variables and logistic regression to categorical and boolean variables. Overall, Linear Regression and Logistic Regression works in a similar way. Linear regression uses a linear function to predict y that minimizes the sum of square residuals and returns numerical values associated with the target feature. By running a linear model on features with real valued data, we can see that the total_units has the most significant impact on the loan approval process since it has the highest coefficient.

<p align="center">  
<img src=images/ols.png width="350" height="240">
</p>

#### Logistic Regression
We also applied a logistics model on categorical and boolean features. After obtaining the model summary from each feature, we decided to take features with coefficient < 0.01 out of consideration regarding what factors can potentially influence the loan approval status. By comparing the coefficients from the summaries of different features, we found that missing_income, Applicant_sex, applicant_age_above_62 are the important features among all the input features for the logistic model. 
Below is an example of the model summary of feature missing_income.
<p align="center">  
<img src=images/logit.png width="450" height="220"> 
</p>

#### Random Forest
Another model that we have implemented is Random Forest. Random Forest performs a bootstrap aggregated ensemble model of trees containing random subsets of features and it can be applied to both regression and classification. The benefits of Random Forest include: reduce overfitting, improve accuracy, efficiency in handling large dataset, and other. The most important attribute of Random Forest is its ability to rank feature importance which would provide massive insight toward our research question to determine the key predictive features that are used to determine loan approval in the current BOA loan approval process. From the result of the random forest model, it illustrates that debt_to_income_ratio and loan_amount are the most important factors toward loan approval. However, applicant_age_above_62 and co-appliacnt_ethinicity also play a role in determining whether a loan should be approved, indicating that the bank does take in unfair elements into consideration and their loan approval system does appear as biased.

<p align="center">
<img src=images/randomforest.png width="400" height="180">
</p>

## Plan for developing the project over the rest of the semester
With the current models that we have constructed so far, we are able to determine the features with stronger significance in predicting loan approval. In the future, we are planning to perform regularization to prevent overfitting, and decrease variance. Another reason for using regularization is that we aim to develop a new system that is fair, and it will penalize certain unfair features towards loan approval to create a better model.




