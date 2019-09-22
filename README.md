# Kickstarter_project

The aim of this project is to build a Classification Model to predict whether or not a new project on Kickstarter will likely succeed or fail.

Data was obtained from:
https://webrobots.io/kickstarter-datasets/
A total of 9338 points. Aim to keep 5000 or more (duplicates, Nans, etc...). The data had the following column:
['backers_count', 'blurb', 'category', 'converted_pledged_amount', 'country', 'created_at', 'creator', 'currency', 'currency_symbol', 'currency_trailing_code', 'current_currency', 'deadline', ‘'disable_communication', 'friends', 'fx_rate', 'goal', 'id', 'is_backing', 'is_starrable', 'is_starred', 'launched_at', 'location', 'name', 'permissions', 'photo', 'pledged', 'profile', 'slug', 'source_url', 'spotlight', 'staff_pick', 'state', 'state_changed_at', 'static_usd_rate', 'urls', 'usd_pledged', 'usd_type']

The final features chosen were:
- category: Film, Music, Fashion etc..
- location: country and state converted to continents to balance data
- created_at: data of starting the campaign
- deadline: deadline set for achieving the desired
- name: projects name
- staff_pick: projects highlighted on homepage
- goal: desired amount of money to succeed amount of money
The rest were eliminated due to repetation, high NAN numbers, potential of data leakage and based on domain knowledge and logic.

The target was 'state' which shows whether the project was  successful, failed, suspended, live, or cancelled. The live, cancelled and suspended states were eliminated as it was difficult to determine the reason for for cancellation/suspension and live projects have no outcome yet.

Out of the aforementioned features the following was created:
- Continuous Features:
    - Log(df['time_allowed']) and Log(df['goal'])
    - df['time_allowed'] = df['deadline']-df['created_at'] # in days
    - outliers = df[(df['time_allowed'] > 5000) & (df['goal'] > 2e6)]
- Categorical Features:
    - Converted to dummies: df[['category', 'staff_pick', 'country']]

![Features Pair PLot](plot_download\pairplots.png)


The following models were run:
- Baseline model of dummyclassifier used which gave 63% accuracy
- Random Forest with hyperparameter tuning using iterations and AUC vs Parameter range plots
- Logistic Regression with hyperparameter tuning in solver type, C parameter and penalty
- XGBoost with and without Gridsearch.
- All models were validated first with training data and then tested with the testing data.

Logistic Regression with l1 penalty, bilinear solver gave the best results. The following is a summary of accuracy, recall, percision and f1 socres of the top four models:

|Model                   |Accuracy        |Recall          |Precision       |F1              |
|------------------------|----------------|----------------|----------------|----------------|
|Random Forest Tuned     |77.2%           |85.9%           |79.5%           |82.6%           |
|LogReg                  |79.6%           |82.2%           |84.5%           |83.3%           |
|LogReg Tuned            |80.0%           |82.0%           |85.2%           |83.6%           |
|XGBoost with GridSearch |78.4%           |88.0%           |79.5%           |83.5%           |

The following can be used to improve on the model
- Quantify quality of the project’s presentation through recognising the use of videos, images and rewards.
- Monitor updates from project founders and number of backers/amount of pledges for first 10-20 days and quantify it as a feature.
- Work on better classifying project categories and make them more uniform
