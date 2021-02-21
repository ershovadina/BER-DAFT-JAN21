# Lesson 7.1

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson to reintroduce the business case study we looked at in unit 01 and unit 04. This time we will look at the complete dataset that consists of over 400 features. We will revisit some of the steps in exploratory data analysis (EDA) and data cleaning process. We will later deep dive into more sophisticated methods of working with a high dimensional dataset as this one. 

---

### Learning Objectives: 
After this lesson, students will be able to: 

- Interpret the business scenario and identify opportunities
- Manipulate data using python
- Implement the EDA and data cleaning process 

--- 

### Lesson 1 key concepts
> :clock10: 20 min

- Reintroduce the business case study
- Formulate a strategy to solve the problem
- Read data into jupyter

<details>
<summary> Click for Description: Introduction to Case study </summary>

- Please refer to the case_study.md file to check the distribution. Talk about the business case scenario, dataset (information avaiable),  and the business objective. 

</details>

<details>
  <summary>Click for Code Sample: Reading the data</summary>

```python
import pandas as pd
import numpy as np
pd.set_option('display.max_columns', None)
import warnings
warnings.filterwarnings('ignore')
import matplotlib.pyplot as plt
import seaborn as sns 
```


```python
data = pd.read_csv('learningSet.csv')
print(data.shape)
data.head()

# Target variables 
data['TARGET_B'].value_counts()
data['TARGET_D']

```
</details>

<details>
  <summary>Click for Description: Formulating a strategy and Challenges</summary>

- Briefly talk about what we did in unit 01 and 04 when we worked with a linear regression model and logistic regression model.

- To solve this problem we will first build a classification model to predict who will more likely respond and then for those respondents, we will build a regression model to predict the donation amount. 

- Then we can use the cost matrix to calculate the total benefit from the donations

- Some of the challenges with the dataset are as follows:
    - Large number of features: The data set has over 450 features. Hence selecting the right features for the model is very critical and at the same time it is not easy as the same traditional ways of removing features is not effective given the large number of features. Apart from faeture selection, feature extraction (creating your own features using the existing features) is also not easy in this case.  
    
    - Sparsity of the dataset: There are a lot of features with a large number of null values. 

    - Data imbalance: For developing a classification, there is a huge imbalance in the training dataset with only approximately 5000 values for one category as compared to oover 95,000 instances for the other category. 

</details>

---

:coffee: __BREAK__

---

#### :pencil2: Check for Understanding - Class activity/quick quiz
> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- We understand the difference between regression problems and classification problems. Linear regression is one of the many machine learning regression algorithms that can be employed to solve regression problems while logistic regression is one of the many machine learning classification algorithms that can be employed to solve classification problems. 

List down two examples / case scenarios each for cases where you can use regression algorithm and where you can use classification algorithm. Describe the target variable and the kind of information available / features in the dataset, that would be used to make the prediction

Do you research to find such examples or develop logical examples on your own 
</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Ask each student to present at least on example each on regression and one on classification

</details>

---

:coffee: __BREAK__

---


### Lesson 2 key concepts
> :clock10: 20 min

- Review data cleaning process

- NOTE: Please mention to the students that the next few lessons, we will spend revisiting some of the code used earlier for data cleaning. Also, since in the last week there was no practice on python, it is important that we build slowly to the more advanced concepts and code implementations. 

<details>
<summary> Click for Code Sample </summary>

- There are a lot of columns that have a very high percentage of null values. It is a highly sparse dataset. First we will start by removing some of the columns that have a high percentage of null values 

- We can decide on a threshold and then remove those variables. There is no rule of thumb to decide on this 
threshold value. Sometimes it can as low as 25%-30%. And sometimes in some data sets you might find that even though there are more than 50% missing values in a column, you might have to include that variable in your analysis. A lot of it depends on the business context as well. In this case we will take this threshold to be 25% and then check the definitions of the columns filtered, to see if there is any column that we might want to keep

- Here we have decided a threshold of 25% ie the columns that have more than 25% null values, we will remove those columns. 

- Here in the code below we are trying to find the columns that have more null missing values than the threshold

```python

nulls_percent_df = pd.DataFrame(data.isna().sum()/len(data)).reset_index()
nulls_percent_df.columns = ['column_name', 'nulls_percentage']
# nulls_percentage.sort_values(by=['null_percentage'], ascending = False)
columns_above_threshold = nulls_percent_df[nulls_percent_df['nulls_percentage']>0.25]
print(len(columns_above_threshold['column_name']))
drop_columns_list = list(columns_above_threshold['column_name'])
print(drop_columns_list)
```
</details>


- Here we will show you a dummy example that you will use in the next lab. 
Lets say you have a given list and you want to remove some elements from it. You can do so using a remove method defined for lists, as shown below:
<details>
  <summary> Click for Code Sample </summary>

```python
x = [1,2,4,5]
x.remove(1)
print(x)
```
</details>



#### :pencil2: Check for Understanding - Class activity/quick quiz
> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Take a look at all the items in "drop_columns_list". These the columns that we want to drop from the dataframe. Now before removing all these columns, you discussed with your manager if there is any column that would still be important to keep in the analysis. Based on their subject matter expertise, you were told that the following columns are important -> wealth1, wealth2
We will remove these variables from the above list 
RDATE3, RAMNT_3 are important but they have too many null values 

<details>
  <summary> Click for Solution: Activity 2 solutions </summary>

```python
cols = ['WEALTH1', 'WEALTH2']
for item in cols:
    drop_columns_list.remove(item)  
data = data.drop(columns=drop_columns_list)
data.shape
```
</details>



---


:coffee: __BREAK__

---

### Lesson 3 key concepts
> :clock10: 20 min

- More data cleaning 

<details>
<summary> Click for Code Sample </summary>

```python
data.head()
```

- We can see that there are a lot of columns that have blank spaces which represent no value in this case. They were not identified as null values by python as they are empty spaces that are read as character values by python. We will replace those values by NaNs and repeat the analysis 

- But before you do that , please check the definition of the variable "MAILCODE" in the variables_description.md file. You will notice that blanks in that case mean that the adress is okay. So we will we will replace the blank values from the column "MAILCODE" by "A" which would mean the address is okay
# this is an important point to emphasize to the students: Checking the data dictionary like this is very very important

# For the next pieces of code, we have used apply function. Spend time on apply method again so that the students are comfortable using it. If the students are more comfortable using map function, you can show them how this can be performed with map as well. 

```python
data['MAILCODE'].value_counts() # To verify the blanks 
# We would actually drop this column later, but we just added this step in order to emphasize that it is important to go through data desctiption because, for eg in this case, a blank actually has a different meaning 

data['MAILCODE'] = data['MAILCODE'].apply(lambda x: x.replace(" ", "A"))

# Now we can replace the blanks in the dataset by np.NaN
data = data.apply(lambda x: x.replace(" ", np.NaN))
data.head()
```
</details>


- Since we have now replaced the empty characters with NaNs, we will carry the same exercise as in the previous lesson to remove columns that have a percentage of missing values more than the threshold. 


---

#### :pencil2: Check for Understanding - Class activity/quick quiz
> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

1. Use the same methodology to remove columns with null values more than a specified threshold
2. Now just like the last time we will discuss it other subject matter experts and keep the following columns -> wealth1, wealth2, VETERANS, SOLIH

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions: Part 1</summary>

```python
nulls_percent_df_2 = pd.DataFrame(data.isna().sum()/len(data)).reset_index()
nulls_percent_df_2.columns = ['column_name', 'nulls_percentage']
# nulls_percentage.sort_values(by=['null_percentage'], ascending = False)
columns_above_threshold_2 = nulls_percent_df_2[nulls_percent_df_2['nulls_percentage']>0.25]
print(len(columns_above_threshold_2['column_name']))
drop_columns_list_2 = list(columns_above_threshold_2['column_name'])
print(drop_columns_list_2)
```
</details>

<details>
  <summary>Click for Solution: Activity 3 solutions: Part 2</summary>

```python
cols_2 = ['WEALTH1', 'WEALTH2', 'VETERANS', 'SOLIH']
for item in cols_2:
    drop_columns_list_2.remove(item) 

data = data.drop(columns=drop_columns_list_2)
print(data.shape)
data.head()
```
</details>


---

:coffee: __BREAK__

---

### Lesson 4 key concepts
> :clock10: 20 min

- Separating data into Y, numerical features and categorical features
- Working with categorical features 


<details>
  <summary>Click for Description: Separating data </summary>

- Since we have a huge number of featuers, it would be easier to work independently on numerical features and categorical features 

- Clarify to the students that this is not the train test split which we looked at earlier.  

- For target variables, for now we will retain them both together. But later, we will build a classification model first where we would need the column "TARGET_B" only

</details>

<details>
<summary> Click for Code Sample: Separating data</summary>

# Also note that the operations that we will perform now are operating column wise 

```python
Y = data[['TARGET_B', 'TARGET_D']]
Y.head()

numerical = data.select_dtypes(np.number)
numerical = numerical.drop(columns = ['TARGET_B', 'TARGET_D'])
numerical.head()


categorical = data.select_dtypes(np.object)
categorical.head()
```
</details>


</details>

<details>
  <summary>Click for Description: Working with categorical variables </summary>

- We will work with the categorical features first. Look at the columns one by one. Some of the operations which we will perform are: 
    - Replace null values with the most occuring categories 
    - Reduce the number of categories in a column by grouping

- It is important to note that these are the columns that are read by python by default as categorical / object types. There might be other columns that would have been read as numerical but we would want them as categorical. We will take a look at them later when we work on numerical types 

</details>


<details>
  <summary> Click for Code Sample: Categorical Variables </summary>

- Here we will try to reduce the number of categories. An ideal way would have been to group the states into regions. But in this case we will group all the states with counts less than 2500 into one category "other"

```python
df = pd.DataFrame(categorical['STATE'].value_counts()).reset_index()
df.columns = ['state', 'count']
other_states = list(df[df['count']<2500]['state'])

def clean_state(x):
    if x in other_states:
        return 'other'
    else:
        return x
categorical['STATE'] = list(map(clean_state, categorical['STATE']))
# categorical['STATE'].value_counts()
```

</details>
---


### :pencil2: Practice on key concepts - Lab
> :clock10: 30 min 

<details>
  <summary> Click for Instructions: Lab </summary>

- Complete the following steps on the categroical columns in the dataset
  - Check for null values in all the columns
  - Exclude the following variables by looking at the definitions. Reasons for their exclusion is provided as well. Create a new empty list called drop_list. We will append this list and then drop all the columns in this list later 
      - OSOURCE - Symbol definitions not provided, too many categories 
      - ZIP CODE - we are including state already 
  - Identifying columns that over 85% missing values
  - Remove those columsn from the dataframe 
  - Reduce the number of categories in the column "GENDER". The column should only have either "M" for males, "F" for females, and "other" for all the rest
    - Note that there are a few null values in the column. We will first replace those null values using the code below:

```python
    print(categorical['GENDER'].value_counts())
    categorical['GENDER'] = categorical['GENDER'].fillna('F')
```
</details>

<details>
  <summary> Click for Code Sample: Working with Categorical variables </summary>

```python
#Check for null values in all the columns
categorical.isna().sum()

## Exclude variables by looking at the definitions 
# OSOURCE - Symbol definitions not provided, too many categories 
# ZIP CODE - we are including state already 

# We will use this list to store column names that we will drop later
drop_list = []
drop_list.append('OSOURCE') 
drop_list.append('ZIP')

# Identifying columns that over 85% missing values 

df = pd.DataFrame(categorical.isna().sum()/len(categorical)).reset_index().sort_values(by=[0], ascending = False)
list(df[df[0]>.85]['index'].values)

# Adding those columns to list 
drop_list = drop_list + list(df[df[0]>.85]['index'].values)


# Reducing number of categories in column "GENDER"
def clean_gender(x):
    if x not in ['F', 'M']:
        return 'other'
    else:
        return x
categorical['GENDER'] = list(map(clean_gender, categorical['GENDER']))

```
</details>

---

:sandwich: __LUNCH BREAK__

---