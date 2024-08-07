# Predicting Sales using Upgini and CatBoost

In this project, we will try to predict the total sales of a store using past 5 years of data.

We will be using `upgini` to enrich our dataset with more features and `CatBoostRegressor` model to train and predict the sales.

**Upgini** is known as a "feature search and enrichment" service. It aims to improve the performance of machine learning models by automatically searching for and adding relevant external features to the datasets used for training models.

`CatBoostRegressor` is a machine learning algorithm designed specifically for regression tasks, part of the CatBoost. CatBoost is a high-performance, open-source gradient boosting on decision trees library that is particularly powerful for datasets with categorical features.

(This was not the ideal project to follow as a beginner project 🫠)

## Notes :-

### 1. Installing upgini and catboost
First, we need to install these 2 modules using `!pip install -Uq upgini catboost"`

### 2. Sampling Our Data
Since our data contains a lot of values, more than needed for this basic project, we randomly select 12000 rows from the Dataset using `DataFrame.sample()`. We need to specify the number of rows that we want to fetch along with any `random_state`.

### 3. Sorting Our Dataset
We can sort DF using `DataFrame.sort_values()`. Inside, we have to pass the column name according to which we want to sort our data.

### 4. Splitting Our Dataset
First, we create the feature DataFrame by dropping the target column, which is **sales ** from the original DataFrame and saving it in a new DF. Then we create a target Series by only selecting the target column and saving it in a new DF.

Then we use the Seaborn's function `.train_test_split()` to split the dataset into training dataset and testing dataset.

### 5. Enriching Our Features
Having only 3 features to predict a target value is not ideal. Hence, we will use **upgini** to create new relevant features.

First, we create an object of `FeaturesEnricher` class. This object is configured to enhance your dataset with additional features, particularly for time series data. Inside, `search_keys` parameter is a dictionary that specifies the keys used to search for relevant external features. In our case, it maps "date" to `SearchKey.DATE`. `SearchKey.DATE` is a predefined key in Upgini that tells the enricher to use the date information to search for relevant features.

Secondly, `cv` parameter specifies the cross-validation strategy to be used. `CVType.time_series` indicates that the cross-validation type is specific to time series data. Time series cross-validation is essential for time-dependent data to ensure that the training and validation sets respect the temporal order, avoiding look-ahead bias.

**Look-ahead bias**, also known as **data snooping bias** or **forward-looking bias**, occurs in time series analysis and forecasting when information from the future is inadvertently used to predict past events. This leads to overly optimistic performance estimates because the model benefits from information that would not have been available in a real-world scenario at the time of prediction.

Then we call the `.fit()` method of the `enricher` object. The `.fit()` method is used to train the model or, in this case, to enrich the dataset with additional features based on the provided training data.

(This section........🫠🫠)

### 6. Model Training
To train our model, we will first create an object of the class `CatBoostRegressor()`. Inside, `verbose=False` means that the training process will not output detailed logs or progress messages to the console.

Then the function, `.enricher.calculate_metrics()` is used to calculate and evaluate the performance metrics of the model after enriching the dataset with additional features. 

`enricher.transform()` are used to transform the original feature sets by adding enriched features. 

(This section........🫠🫠🫠)

Then finally, we train our model using `model.fit()` and predict the target data using `model.predict()`. 

We use `eval_metric()` to evaluate the predictions. **"SMAPE"** parameter is a string specifying the metric to be used for evaluation. In this case, "SMAPE" stands for Symmetric Mean Absolute Percentage Error.

The lower the **SMAPE**, the better the model did at predicting the data. In our case, model did better prediction when trained on enriched dataset rather than on original dataset.

<br>

---------
