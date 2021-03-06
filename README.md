# Neural Network Charity Analysis
## Overview of the analysis: Explain the purpose of this analysis.
Alphabet Soup is an organization dedicated in helping organizations that take care of the enviroment, improve people wellbeing and unify the world, they have invested over 10 Billion dollars in the past years, in order to analyze the impact of their donations, they want us to analyze which organizations are worth donation to and which ones are of high risk.

From Alphabet Soup’s business team, we have received a CSV containing more than 34,000 organizations that have received funding from Alphabet Soup over the years. Within this dataset are a number of columns that capture metadata about each organization, such as the following:
* **EIN** and **NAME**—Identification columns
* **APPLICATION_TYPE**—Alphabet Soup application type
* **AFFILIATION—Affiliated** sector of industry
* **CLASSIFICATION**—Government organization classification
* **USE_CASE**—Use case for funding
* **ORGANIZATION**—Organization type
* **STATUS**—Active status
* **INCOME_AMT**—Income classification
* **SPECIAL_CONSIDERATIONS**—Special consideration for application
* **ASK_AMT**—Funding amount requested
* **IS_SUCCESSFUL**—Was the money used effectively

## Resources:
* Data Source: <code>charity_data.csv</code>
* Software: <code>Python 3.9</code>, <code>Jupyter Notebook 6.1.4</code>, <code>Tensorflow 2.6.0</code> and <code>Pandas</code>
## Results: 

### Data Preprocessing
From the variables we have, we defined the variable <code>IS_SUCCESSFUL</code> as the target of our model, then the next steps were followed:
* The following variable(s) were removed from input and data: <code>NAME</code> and <code>EIN</code>
* Categorical variables with a number of unique values higher than then were bucketed as follows:
  * <code>APPLICATION_TYPE</code> with less than <code>200</code> value counts were grouped into a new category <code>Other</code> reducing the number of unique values from **17** to <code>9</code>
  * <code>CLASSIFICATION</code> with less than <code>1000</code> value counts were grouped into the category <code>Other</code> reducing the number of unique values from **71** to <code>6</code>
* Encode all categorical variables with <code>OneHotEncoder</code>
* Merge the one-hot encoded DataFrame with the original DataFrame and drop the original categorical variables.
* Drop our target variable <code>IS_SUCCESSFUL</code> and mantain the remaining as features.
* Split the data into training <code>75%</code> and testing <code>25%</code> sets with the sklearn function <code>train_test_split</code>
* Scale the data with the sklearn function <code>StandardScaler()</code>

### Compiling, Training, and Evaluating the Model
Using tensorflow and keras we defined a Neural Network model with two Dense layers of <code>28</code> hidden nodes and <code>15</code> hidden nodes respectively, using the activation function <code>reLu</code> and an Output layer with the sigmoid activation layer as shown in the image below: 

<p align="center">
  <img src="./Resources/model_original.png" alt="Sublime's custom image"/>
</p>

With this model, we obtained an accuracy of <code>72.43%</code> and it's performance is shown in the confusion matrix below: 

<p align="center">
  <img src="./Resources/Original_ConfusionMatrix.png" alt="Sublime's custom image"/>
</p>

### Optimizing the model
In order to improve the performance of the Neural Network model, multiple approaches were attempted, the approaches were as follows:
#### Increase the number of Neurons:
Since there were 43 features for 43 neurons in our first model, an attempt to increase the accuracy of the model by duplicating the number of neurons was performed. The number of neurons for the first Dense Layer is <code>58</code> and for the second layer <code>28</code> as shown in the image below:

<p align="center">
  <img src="./Resources/model_attempt1.png" alt="Sublime's custom image"/>
</p>

And the performance obtained from this model was <code>72.51%</code> increasing only <code>0.08%</code> form the original. The confusion matrix for the attempt 1 is shown below:

<p align="center">
  <img src="./Resources/Attempt1_ConfusionMatrix.png" alt="Sublime's custom image"/>
</p>

#### Increase number of layers, add Dropout layers and change Activation Functions:
Since the accuracy obtained in the training epochs was a little bit higher on the training epochs, we added dropout layers that randomly sets input units to 0 during a certain rate preventing overfitting. Also another Dense layer was added in order to extract more complex features, and interchanging the activation function between <code>reLu</code> and <code>softmax</code>

<p align="center">
  <img src="./Resources/model_attempt2.png" alt="Sublime's custom image"/>
</p>

The accuracy obtained from this model was <code>72.57%</code> which means an increase of <code>0.06%</code> with respect to the previous optimization attempt and an increase of <code>1.4%</code>

<p align="center">
  <img src="./Resources/Attempt2_ConfusionMatrix.png" alt="Sublime's custom image"/>
</p>

#### Auto Optimization
Using the tool <code>Keras Tuner</code> we can try multiple activation functions, layers or neurons and find out the best hyperparameter values for our model. In this attempt we used the <code>Hyperband</code> algorithm to optimize our model. The model is shown below:

<p align="center">
  <img src="./Resources/model_attempt3.png" alt="Sublime's custom image"/>
</p>

The accuracy obtained from this model was <code>72.9%</code> with an increase of <code>0.33%</code> with respect to the previous attempt and <code>0.47%</code> with the original model.

<p align="center">
  <img src="./Resources/Attempt3_ConfusionMatrix.png" alt="Sublime's custom image"/>
</p>

## Summary: 

From the results obtained from the last attempt we can conclude that further time in data preprocessing needs to be taken, since the auto-optimization model returns probably the best model with the given features. I will recommend to try out different bucketing sizes for categorical variables and explore another types of data transfromations such as normalization or other scalers.
Summarize the overall results of the deep learning model. Include a recommendation for how a different model could solve this classification problem, and explain your recommendation.
