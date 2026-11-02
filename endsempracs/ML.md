# MACHINE LEARNING AND DATA ANALYTICS – PRACTICAL JOURNAL

## Complete Practical Journal – Python

**Programming Language:** Python 3  
**Execution Environment:** Linux / Ubuntu  
**Total Practicals:** 4

---

# INDEX

| Sr. No. | Practical Name | Algorithm / Technique |
|---|---|---|
| 1 | Uber Fare Prediction | Linear Regression and Random Forest |
| 2 | Spam Email Detection | KNN and SVM Classification |
| 3 | Gradient Descent Algorithm | Optimization |
| 4 | Clustering | K-Means and Hierarchical Clustering |

---

# PRACTICAL 1 – UBER FARE PREDICTION

## 1. Problem Statement

Predict the price of the Uber ride from a given pickup point to the agreed drop-off location.

Perform the following tasks:

1. Pre-process the dataset.
2. Identify outliers.
3. Check the correlation.
4. Implement Linear Regression and Random Forest Regression models.
5. Evaluate the models and compare their respective scores like R2, RMSE, etc.

**Dataset:** `uber.csv`

## 2. Theory

Uber Fare Prediction is a Machine Learning regression problem.

The objective is to predict the fare amount based on pickup and drop-off locations and other relevant features.

The practical uses two regression algorithms:

**1. Linear Regression**

Linear Regression is a supervised Machine Learning algorithm used to predict continuous numerical values by establishing a relationship between input features and the target variable.

**2. Random Forest Regression**

Random Forest Regression uses multiple decision trees and combines their predictions to produce a final result.

**Important Concepts:**

- Data Preprocessing
- Outlier Detection
- Feature Engineering
- Haversine Formula
- Correlation Analysis
- Linear Regression
- Random Forest Regression
- Model Evaluation

### Evaluation Metrics

| Metric | Description |
|---|---|
| R2 Score | Measures how well the model explains variations in the target. |
| RMSE | Measures the square root of the average squared prediction error. |
| MAE | Measures the average absolute prediction error. |

## 3. Algorithm

1. Start.
2. Import the required libraries.
3. Load the Uber dataset.
4. Display the initial dataset.
5. Remove missing values.
6. Convert pickup datetime into datetime format.
7. Extract hour, day, month, year and day of the week.
8. Remove unnecessary columns.
9. Identify and remove unrealistic fare and location values.
10. Calculate the distance between pickup and drop-off locations using the Haversine formula.
11. Perform correlation analysis.
12. Separate input features and target variable.
13. Split the dataset into training and testing sets.
14. Train the Linear Regression model.
15. Train the Random Forest Regression model.
16. Evaluate both models using R2, RMSE and MAE.
17. Compare the model performances.
18. Display the comparison graph.
19. Stop.

## 4. Python Code

**Filename:** `practical1.py`

### Block 1 – Importing Libraries

```python
# Import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import r2_score, mean_squared_error, mean_absolute_error
from math import radians, cos, sin, asin, sqrt
```

### Block 2 – Data Loading and Preprocessing

```python
# Load the dataset
df = pd.read_csv("uber.csv")   # change to your local path if needed
print("Initial Data Shape:", df.shape)
print(df.head())

# Drop rows with missing values
df.dropna(inplace=True)
print("After dropping missing values:", df.shape)

# Rename columns for easier reference
df.rename(columns={'pickup_datetime': 'pickup_datetime'}, inplace=True)

# Convert pickup_datetime to datetime object
df['pickup_datetime'] = pd.to_datetime(df['pickup_datetime'], errors='coerce')

# Extract useful datetime features
df['hour'] = df['pickup_datetime'].dt.hour
df['day'] = df['pickup_datetime'].dt.day
df['month'] = df['pickup_datetime'].dt.month
df['year'] = df['pickup_datetime'].dt.year
df['day_of_week'] = df['pickup_datetime'].dt.dayofweek

# Drop datetime column (not needed as a direct feature)
df.drop(['pickup_datetime', 'key'], axis=1, inplace=True, errors='ignore')

print("\nColumns after feature extraction:\n", df.columns)
```

### Block 3 – Outlier Detection and Removal

```python
# Remove entries with unrealistic fares
df = df[(df['fare_amount'] > 0) & (df['fare_amount'] < 100)]

# Remove unrealistic latitude and longitude values
df = df[(df['pickup_latitude'] <= 90) & (df['pickup_latitude'] >= -90)]
df = df[(df['dropoff_latitude'] <= 90) & (df['dropoff_latitude'] >= -90)]
df = df[(df['pickup_longitude'] <= 180) & (df['pickup_longitude'] >= -180)]
df = df[(df['dropoff_longitude'] <= 180) & (df['dropoff_longitude'] >= -180)]

print("Data shape after removing outliers:", df.shape)
```

### Block 4 – Feature Engineering and Distance Calculation

```python
# Define Haversine function to calculate distance between pickup and drop-off
def haversine(lat1, lon1, lat2, lon2):
    # convert decimal degrees to radians
    lat1, lon1, lat2, lon2 = map(radians, [lat1, lon1, lat2, lon2])
    # haversine formula
    dlon = lon2 - lon1
    dlat = lat2 - lat1
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * asin(sqrt(a))
    km = 6371 * c
    return km

# Apply the Haversine formula
df['distance_km'] = df.apply(lambda x: haversine(x['pickup_latitude'], x['pickup_longitude'],
                                                 x['dropoff_latitude'], x['dropoff_longitude']), axis=1)

# Remove zero-distance trips
df = df[df['distance_km'] > 0]
```

### Block 5 – Correlation Analysis

```python
plt.figure(figsize=(10, 6))
sns.heatmap(df.corr(), annot=True, cmap='coolwarm')
plt.title("Feature Correlation Heatmap")
plt.show()
```

### Block 6 – Model Training

```python
# Define features and target
X = df[['distance_km', 'hour', 'day', 'month', 'year', 'day_of_week']]
y = df['fare_amount']

# Split data into train and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# -------------------- Linear Regression --------------------
lr_model = LinearRegression()
lr_model.fit(X_train, y_train)
y_pred_lr = lr_model.predict(X_test)

# -------------------- Random Forest Regression --------------------
rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)
y_pred_rf = rf_model.predict(X_test)
```

### Block 7 – Model Evaluation

```python
def evaluate_model(y_true, y_pred, model_name):
    r2 = r2_score(y_true, y_pred)
    rmse = np.sqrt(mean_squared_error(y_true, y_pred))
    mae = mean_absolute_error(y_true, y_pred)
    print(f"\nModel: {model_name}")
    print(f"R² Score: {r2:.4f}")
    print(f"RMSE: {rmse:.4f}")
    print(f"MAE: {mae:.4f}")
    return r2, rmse, mae

# Evaluate both models
lr_scores = evaluate_model(y_test, y_pred_lr, "Linear Regression")
rf_scores = evaluate_model(y_test, y_pred_rf, "Random Forest Regressor")
```

### Block 8 – Model Comparison

```python
results = pd.DataFrame({
    'Model': ['Linear Regression', 'Random Forest Regressor'],
    'R2': [lr_scores[0], rf_scores[0]],
    'RMSE': [lr_scores[1], rf_scores[1]],
    'MAE': [lr_scores[2], rf_scores[2]]
})

print("\nModel Comparison:")
print(results)
```

### Block 9 – Plotting Comparison

```python
# Plot comparison
plt.figure(figsize=(8,5))
sns.barplot(x='Model', y='R2', data=results)
plt.title("R² Score Comparison between Models")
plt.show()
```

## 5. Sample Output

The original practical file does not contain a fixed sample output.

The program displays:

- Initial dataset shape.
- First five dataset records.
- Dataset shape after removing missing values.
- Dataset shape after outlier removal.
- Feature correlation heatmap.
- R2 Score, RMSE and MAE for both models.
- Model comparison table.
- R2 comparison graph.

**Note:** Numerical results depend on the dataset and execution environment.

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Install the required libraries.

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

Step 3: Create the Python file.

```bash
nano practical1.py
```

Step 4: Paste the code and save the file.

Step 5: Execute the program.

```bash
python3 practical1.py
```

Make sure `uber.csv` is present in the same directory or provide its correct path.

## 7. Explanation of the Code

### Part A – Importing Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

- `pandas`: Used for data manipulation and analysis.
- `numpy`: Used for numerical calculations.
- `matplotlib`: Used for plotting graphs.
- `seaborn`: Used for statistical visualizations.

```python
from sklearn.model_selection import train_test_split
```

Splits the dataset into training and testing sets.

```python
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import RandomForestRegressor
```

Imports the two regression algorithms.

```python
from sklearn.metrics import r2_score, mean_squared_error, mean_absolute_error
```

Imports evaluation metrics.

```python
from math import radians, cos, sin, asin, sqrt
```

Imports mathematical functions required for calculating geographical distance.

### Part B – Loading Dataset

```python
df = pd.read_csv("uber.csv")
```

Loads the Uber dataset into a Pandas DataFrame.

```python
df.dropna(inplace=True)
```

Removes rows containing missing values.

```python
print(df.head())
```

Displays the first five records.

### Part C – Date and Time Feature Extraction

```python
df['pickup_datetime'] = pd.to_datetime(df['pickup_datetime'], errors='coerce')
```

Converts the pickup datetime column into datetime format.

```python
df['hour'] = df['pickup_datetime'].dt.hour
df['day'] = df['pickup_datetime'].dt.day
df['month'] = df['pickup_datetime'].dt.month
df['year'] = df['pickup_datetime'].dt.year
df['day_of_week'] = df['pickup_datetime'].dt.dayofweek
```

Extracts useful information from the pickup date and time.

For example:
- Hour represents the pickup hour.
- Day represents the day of the month.
- Month represents the month.
- Year represents the year.
- Day of week represents the weekday.

### Part D – Outlier Removal

```python
df = df[(df['fare_amount'] > 0) & (df['fare_amount'] < 100)]
```

Removes unrealistic fare values.

```python
df = df[(df['pickup_latitude'] <= 90) & (df['pickup_latitude'] >= -90)]
```

Checks whether pickup latitude is within the valid geographical range.

Similar conditions are applied to the other latitude and longitude columns.

### Part E – Haversine Function

```python
def haversine(lat1, lon1, lat2, lon2):
```

Defines a function to calculate the geographical distance between two points.

```python
lat1, lon1, lat2, lon2 = map(radians, [lat1, lon1, lat2, lon2])
```

Converts latitude and longitude values from degrees into radians.

```python
dlon = lon2 - lon1
dlat = lat2 - lat1
```

Calculates the difference between the coordinates.

```python
a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
c = 2 * asin(sqrt(a))
```

Applies the Haversine formula to calculate the angular distance.

```python
km = 6371 * c
return km
```

Multiplies the angular distance by Earth's approximate radius of 6371 kilometres.

### Part F – Correlation Analysis

```python
sns.heatmap(df.corr(), annot=True, cmap='coolwarm')
```

Creates a correlation heatmap.

Correlation measures the relationship between numerical features.

- Positive correlation: Both variables tend to increase together.
- Negative correlation: One variable tends to decrease when the other increases.
- Zero correlation: No linear relationship.

### Part G – Model Training

```python
X = df[['distance_km', 'hour', 'day', 'month', 'year', 'day_of_week']]
y = df['fare_amount']
```

- `X` contains input features.
- `y` contains the target variable.

```python
train_test_split(X, y, test_size=0.2, random_state=42)
```

Splits the dataset into 80% training data and 20% testing data.

```python
lr_model = LinearRegression()
lr_model.fit(X_train, y_train)
```

Creates and trains the Linear Regression model.

```python
rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)
```

Creates and trains a Random Forest model using 100 decision trees.

### Part H – Model Evaluation

```python
r2 = r2_score(y_true, y_pred)
```

Calculates the R2 score.

```python
rmse = np.sqrt(mean_squared_error(y_true, y_pred))
```

Calculates Root Mean Squared Error.

```python
mae = mean_absolute_error(y_true, y_pred)
```

Calculates Mean Absolute Error.

The function evaluates both models and returns their scores.

### Part I – Model Comparison

```python
results = pd.DataFrame({
    'Model': ['Linear Regression', 'Random Forest Regressor'],
    'R2': [lr_scores[0], rf_scores[0]],
    'RMSE': [lr_scores[1], rf_scores[1]],
    'MAE': [lr_scores[2], rf_scores[2]]
})
```

Creates a DataFrame containing the performance metrics of both models.

```python
sns.barplot(x='Model', y='R2', data=results)
```

Displays a bar graph comparing the R2 scores.

## 8. Time and Space Complexity

The complexity depends on the number of dataset records and features.

| Operation | Complexity |
|---|---|
| Data Preprocessing | O(n) approximately |
| Haversine Calculation | O(n) |
| Correlation Analysis | O(n × m²) |
| Linear Regression Training | Depends on n and m |
| Random Forest Training | Depends on trees, n and m |
| Model Prediction | Depends on the trained model |

Where:
- n = Number of dataset records.
- m = Number of input features.

## 9. Viva Questions and Answers

**Q1. What is Regression?**

Regression is a supervised Machine Learning technique used to predict continuous numerical values.

**Q2. What is Linear Regression?**

It is an algorithm that establishes a linear relationship between input features and a target variable.

**Q3. What is Random Forest?**

Random Forest is an ensemble learning algorithm that combines multiple decision trees.

**Q4. What are outliers?**

Outliers are data points that differ significantly from other observations.

**Q5. What is the Haversine formula?**

It calculates the great-circle distance between two geographical coordinates.

**Q6. What is R2 Score?**

It measures how well a regression model explains the variation in the target variable.

**Q7. What is RMSE?**

Root Mean Squared Error measures the square root of the average squared prediction errors.

**Q8. What is MAE?**

Mean Absolute Error measures the average absolute difference between actual and predicted values.

**Q9. Why do we split the dataset?**

To train the model on one portion and evaluate its performance on unseen data.

**Q10. What is feature engineering?**

It is the process of creating or transforming features to improve Machine Learning performance.

---

# PRACTICAL 2 – SPAM EMAIL DETECTION

## 1. Problem Statement

Classify the email using the binary classification method.

Email Spam detection has two states:

1. Normal State – Not Spam.
2. Abnormal State – Spam.

Use K-Nearest Neighbors and Support Vector Machine for classification. Analyze their performance.

**Dataset:** `emails.csv`

## 2. Theory

Spam Email Detection is a supervised Machine Learning classification problem.

The objective is to classify emails into two categories:

- 0 – Not Spam.
- 1 – Spam.

This practical uses two classification algorithms.

**1. K-Nearest Neighbors (KNN)**

KNN classifies a data point based on the majority class among its nearest neighbouring data points.

**2. Support Vector Machine (SVM)**

SVM is a supervised learning algorithm that finds an optimal decision boundary to separate different classes.

**Important Concepts:**

- Binary Classification
- Training and Testing
- K-Nearest Neighbors
- Support Vector Machine
- Accuracy
- Classification Report
- Confusion Matrix

### Evaluation Metrics

| Metric | Description |
|---|---|
| Accuracy | Percentage of correctly classified emails. |
| Precision | Percentage of predicted positive cases that are actually positive. |
| Recall | Percentage of actual positive cases correctly identified. |
| F1 Score | Harmonic mean of precision and recall. |

## 3. Algorithm

1. Start.
2. Import the required libraries.
3. Load the emails dataset.
4. Remove unnecessary columns.
5. Convert the Prediction column into integer format.
6. Separate input features and target variable.
7. Split the dataset into training and testing sets.
8. Create the KNN classifier.
9. Train the KNN model.
10. Predict the test results.
11. Calculate KNN accuracy and classification report.
12. Create the SVM classifier.
13. Train the SVM model.
14. Predict the test results.
15. Calculate SVM accuracy and classification report.
16. Generate confusion matrices.
17. Display confusion matrix heatmaps.
18. Compare both classification algorithms.
19. Stop.

## 4. Python Code

**Filename:** `practical2.py`

### Block 1 – Importing Libraries

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
import matplotlib.pyplot as plt
import seaborn as sns
```

### Block 2 – Loading Dataset

```python
df = pd.read_csv("emails.csv", encoding="ISO-8859-1")  # Adjust path if needed

# Drop unnecessary columns if present
if "Email No." in df.columns:
    df = df.drop(columns=["Email No."])

# Ensure label is integer
df["Prediction"] = df["Prediction"].astype(int)

# Features & target
X = df.drop(columns=["Prediction"])
y = df["Prediction"]

# Print basic info
print(df.columns)
print(df.head(5))
```

### Block 3 – Data Splitting

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
```

### Block 4 – K-Nearest Neighbors (KNN)

```python
knn = KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train, y_train)
y_pred_knn = knn.predict(X_test)

print("\n--- KNN Performance ---")
print("Accuracy:", accuracy_score(y_test, y_pred_knn))
print("Classification Report:\n", classification_report(y_test, y_pred_knn))
print("Confusion Matrix:\n", confusion_matrix(y_test, y_pred_knn))
```

### Block 5 – Support Vector Machine (SVM)

```python
svm = SVC(kernel='linear', random_state=42)  # Linear kernel for binary classification
svm.fit(X_train, y_train)
y_pred_svm = svm.predict(X_test)

print("\n--- SVM Performance ---")
print("Accuracy:", accuracy_score(y_test, y_pred_svm))
print("Classification Report:\n", classification_report(y_test, y_pred_svm))
print("Confusion Matrix:\n", confusion_matrix(y_test, y_pred_svm))
```

### Block 6 – Plotting Confusion Matrices

```python
fig, ax = plt.subplots(1, 2, figsize=(12, 5))

sns.heatmap(confusion_matrix(y_test, y_pred_knn), annot=True, fmt="d", cmap="Blues", ax=ax[0])
ax[0].set_title("KNN Confusion Matrix")
ax[0].set_xlabel("Predicted")
ax[0].set_ylabel("Actual")

sns.heatmap(confusion_matrix(y_test, y_pred_svm), annot=True, fmt="d", cmap="Greens", ax=ax[1])
ax[1].set_title("SVM Confusion Matrix")
ax[1].set_xlabel("Predicted")
ax[1].set_ylabel("Actual")

plt.show()
```

## 5. Sample Output

The original practical file does not provide fixed numerical sample output.

The program displays:

- Dataset column names.
- First five dataset records.
- KNN accuracy.
- KNN classification report.
- KNN confusion matrix.
- SVM accuracy.
- SVM classification report.
- SVM confusion matrix.
- Graphical comparison of both confusion matrices.

**Note:** Actual performance values depend on the dataset.

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Install the required libraries.

```bash
pip install pandas scikit-learn matplotlib seaborn
```

Step 3: Create the Python file.

```bash
nano practical2.py
```

Step 4: Paste the code and save it.

Step 5: Execute the program.

```bash
python3 practical2.py
```

Make sure `emails.csv` is present in the same directory.

## 7. Explanation of the Code

### Part A – Importing Libraries

```python
import pandas as pd
```

Imports Pandas for handling the dataset.

```python
from sklearn.model_selection import train_test_split
```

Imports the function used to divide the dataset into training and testing sets.

```python
from sklearn.neighbors import KNeighborsClassifier
```

Imports the KNN classification algorithm.

```python
from sklearn.svm import SVC
```

Imports the Support Vector Machine classifier.

```python
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
```

Imports the evaluation metrics.

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

Imports libraries for visualizing confusion matrices.

### Part B – Loading Dataset

```python
df = pd.read_csv("emails.csv", encoding="ISO-8859-1")
```

Loads the email dataset.

```python
if "Email No." in df.columns:
    df = df.drop(columns=["Email No."])
```

Checks whether the Email No. column exists and removes it if present.

```python
df["Prediction"] = df["Prediction"].astype(int)
```

Converts the Prediction column into integer format.

### Part C – Separating Features and Target

```python
X = df.drop(columns=["Prediction"])
y = df["Prediction"]
```

- `X` stores the input features.
- `y` stores the target labels.

The target variable represents whether an email is spam or not spam.

### Part D – Data Splitting

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
```

Splits the dataset into training and testing portions.

- 80% data is used for training.
- 20% data is used for testing.
- `random_state=42` makes the split reproducible.
- `stratify=y` maintains the class distribution in both sets.

### Part E – KNN Classification

```python
knn = KNeighborsClassifier(n_neighbors=5)
```

Creates a KNN classifier with five neighbours.

```python
knn.fit(X_train, y_train)
```

Trains the classifier using training data.

```python
y_pred_knn = knn.predict(X_test)
```

Predicts the classes of the testing data.

```python
accuracy_score(y_test, y_pred_knn)
```

Calculates the accuracy of the KNN model.

```python
classification_report(y_test, y_pred_knn)
```

Displays precision, recall, F1 score and support.

```python
confusion_matrix(y_test, y_pred_knn)
```

Displays the classification results in a confusion matrix.

### Part F – SVM Classification

```python
svm = SVC(kernel='linear', random_state=42)
```

Creates an SVM classifier using a linear kernel.

```python
svm.fit(X_train, y_train)
```

Trains the SVM model.

```python
y_pred_svm = svm.predict(X_test)
```

Predicts the classes of the testing emails.

The accuracy, classification report and confusion matrix are then calculated.

### Part G – Confusion Matrix Visualization

```python
fig, ax = plt.subplots(1, 2, figsize=(12, 5))
```

Creates two side-by-side plotting areas.

```python
sns.heatmap(confusion_matrix(y_test, y_pred_knn), annot=True, fmt="d", cmap="Blues", ax=ax[0])
```

Displays the KNN confusion matrix as a heatmap.

```python
sns.heatmap(confusion_matrix(y_test, y_pred_svm), annot=True, fmt="d", cmap="Greens", ax=ax[1])
```

Displays the SVM confusion matrix.

```python
plt.show()
```

Displays both graphs.

## 8. Time and Space Complexity

Let:

- n = Number of training samples.
- m = Number of features.
- k = Number of neighbours.

| Algorithm | Time Complexity |
|---|---|
| KNN Training | Approximately O(n × m) |
| KNN Prediction | O(n × m) per query |
| Linear Kernel SVM Training | Depends on solver and dataset |
| Confusion Matrix | O(n) |

**Space Complexity:** Depends on the number of training samples, features and model representation.

## 9. Viva Questions and Answers

**Q1. What is Spam Email Detection?**

It is a classification problem that identifies whether an email is spam or not spam.

**Q2. What is Binary Classification?**

Classification in which data is divided into two classes.

**Q3. What is KNN?**

K-Nearest Neighbors is an algorithm that classifies data based on the majority class of its nearest neighbours.

**Q4. What is the value of k in this practical?**

The value of k is 5.

**Q5. What is SVM?**

Support Vector Machine is an algorithm that finds an optimal decision boundary between different classes.

**Q6. What is a kernel in SVM?**

A kernel is a function used to calculate relationships between data points and help create decision boundaries.

**Q7. What is a confusion matrix?**

It is a table that compares actual and predicted classification results.

**Q8. What is Precision?**

Precision measures how many predicted positive cases are actually positive.

**Q9. What is Recall?**

Recall measures how many actual positive cases are correctly identified.

**Q10. What is F1 Score?**

It is the harmonic mean of precision and recall.

**Q11. What is the difference between KNN and SVM?**

KNN classifies data using neighbouring points, whereas SVM classifies data using an optimal decision boundary.

---

# PRACTICAL 3 – GRADIENT DESCENT ALGORITHM

## 1. Problem Statement

Implement Gradient Descent Algorithm to find the local minima of a function.

For example, find the local minima of the function:

\[
y=(x+3)^2
\]

Starting from the point:

\[
x=2
\]

## 2. Theory

Gradient Descent is an optimization algorithm used to find the minimum value of a function.

It works by repeatedly updating the current value in the direction opposite to the gradient.

The objective is to reach the minimum value of the function.

### Gradient Descent Formula

\[
x_{new}=x_{old}-\alpha f'(x)
\]

Where:

- \(x_{new}\) = Updated value.
- \(x_{old}\) = Current value.
- \(\alpha\) = Learning rate.
- \(f'(x)\) = Derivative of the function.

### Given Function

\[
f(x)=(x+3)^2
\]

Its derivative is:

\[
f'(x)=2(x+3)
\]

The starting point is:

\[
x=2
\]

The algorithm repeatedly updates x until it approaches the minimum.

**Important Concepts:**

- Optimization
- Gradient
- Derivative
- Learning Rate
- Convergence
- Local Minimum

## 3. Algorithm

1. Start.
2. Define the function \(f(x)=(x+3)^2\).
3. Define its derivative \(f'(x)=2(x+3)\).
4. Initialize the starting point x = 2.
5. Set the learning rate to 0.1.
6. Set the convergence tolerance.
7. Set the maximum number of iterations.
8. Calculate the gradient.
9. Update x using the Gradient Descent formula.
10. Check whether the difference between consecutive values is smaller than the tolerance.
11. If the condition is satisfied, stop the iteration.
12. Otherwise, continue the process.
13. Display the local minimum and function value.
14. Plot the convergence graph.
15. Stop.

## 4. Python Code

**Filename:** `practical3.py`

### Block 1 – Importing Libraries

```python
import numpy as np
import matplotlib.pyplot as plt
```

### Block 2 – Defining the Function and Derivative

```python
def f(x):
    return (x + 3)**2

def grad_f(x):
    return 2 * (x + 3)  # derivative of f(x)
```

### Block 3 – Initializing Parameters

```python
x_current = 2          # starting point
learning_rate = 0.1    # step size
tolerance = 1e-6       # convergence tolerance
max_iterations = 25    # maximum iterations
history = [x_current]  # sotring history
```

### Block 4 – Gradient Descent Loop

```python
for i in range(max_iterations):
    gradient = grad_f(x_current)
    x_next = x_current - learning_rate * gradient  # update step
    
    # Check convergence
    if abs(x_next - x_current) < tolerance:
        print(f"Converged after {i+1} iterations.")
        break
    
    x_current = x_next
    history.append(x_current)
    print(f"Iteration {i+1}: x = {x_current:.4f}, f(x) = {f(x_current):.4f}")
```

### Block 5 – Printing the Result

```python
print("Local minima at x =", x_current)
print("Function value at local minima y =", f(x_current))
```

### Block 6 – Plotting

```python
plt.plot(history, [f(val) for val in history], marker='o')
plt.xlabel("x values")
plt.ylabel("f(x)")
plt.title("Gradient Descent Convergence")
plt.grid()
plt.show()
```

## 5. Sample Output

The original practical file does not include a fixed sample output.

The program displays:

- Iteration number.
- Updated x value.
- Function value at each iteration.
- Convergence message if the tolerance condition is reached.
- Approximate local minimum.
- Function value at the minimum.
- Gradient Descent convergence graph.

For the given function, the theoretical minimum is:

\[
x=-3
\]

\[
y=0
\]

The algorithm approaches this minimum through repeated iterations.

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Install the required libraries.

```bash
pip install numpy matplotlib
```

Step 3: Create the Python file.

```bash
nano practical3.py
```

Step 4: Paste the code and save it.

Step 5: Execute the program.

```bash
python3 practical3.py
```

## 7. Explanation of the Code

### Part A – Importing Libraries

```python
import numpy as np
import matplotlib.pyplot as plt
```

- NumPy is used for numerical operations.
- Matplotlib is used to plot the convergence graph.

### Part B – Defining the Function

```python
def f(x):
    return (x + 3)**2
```

Defines the mathematical function.

\[
f(x)=(x+3)^2
\]

This function has its minimum at x = -3.

### Part C – Defining the Derivative

```python
def grad_f(x):
    return 2 * (x + 3)
```

Defines the derivative of the function.

The derivative is:

\[
f'(x)=2(x+3)
\]

The gradient indicates the direction and rate of change of the function.

### Part D – Initializing Parameters

```python
x_current = 2
```

Sets the starting point to 2.

```python
learning_rate = 0.1
```

Sets the learning rate to 0.1.

The learning rate determines the size of each update.

```python
tolerance = 1e-6
```

Defines the convergence tolerance.

It determines when the algorithm should stop updating.

```python
max_iterations = 25
```

Sets the maximum number of iterations to 25.

```python
history = [x_current]
```

Stores the values of x during the optimization process.

### Part E – Gradient Descent Loop

```python
for i in range(max_iterations):
```

Executes the Gradient Descent process for a maximum of 25 iterations.

```python
gradient = grad_f(x_current)
```

Calculates the derivative at the current value of x.

```python
x_next = x_current - learning_rate * gradient
```

Updates x using the Gradient Descent formula.

\[
x_{new}=x_{old}-\alpha f'(x)
\]

### Part F – Checking Convergence

```python
if abs(x_next - x_current) < tolerance:
```

Checks whether the difference between the new and old values is smaller than the tolerance.

```python
print(f"Converged after {i+1} iterations.")
break
```

Displays the convergence message and stops the loop when the condition is satisfied.

```python
x_current = x_next
```

Updates the current value.

```python
history.append(x_current)
```

Stores the updated value in the history list.

```python
print(f"Iteration {i+1}: x = {x_current:.4f}, f(x) = {f(x_current):.4f}")
```

Displays the current iteration, x value and corresponding function value.

### Part G – Printing the Result

```python
print("Local minima at x =", x_current)
```

Displays the approximate minimum location.

```python
print("Function value at local minima y =", f(x_current))
```

Displays the function value at the minimum.

### Part H – Plotting

```python
plt.plot(history, [f(val) for val in history], marker='o')
```

Plots the function values corresponding to the stored x values.

```python
plt.xlabel("x values")
plt.ylabel("f(x)")
```

Sets the x-axis and y-axis labels.

```python
plt.title("Gradient Descent Convergence")
```

Sets the graph title.

```python
plt.grid()
plt.show()
```

Displays the grid and final graph.

## 8. Time and Space Complexity

Let n be the number of iterations.

| Operation | Complexity |
|---|---|
| Function Evaluation | O(1) |
| Gradient Calculation | O(1) |
| One Gradient Update | O(1) |
| Total Time Complexity | O(n) |
| Space Complexity | O(n) |

The history list stores the values generated during each iteration, requiring O(n) space.

## 9. Viva Questions and Answers

**Q1. What is Gradient Descent?**

Gradient Descent is an optimization algorithm used to find the minimum of a function.

**Q2. What is a gradient?**

A gradient represents the direction and rate of the greatest increase of a function.

**Q3. What is a learning rate?**

It determines the size of each update during optimization.

**Q4. What happens if the learning rate is too high?**

The algorithm may overshoot the minimum or fail to converge.

**Q5. What happens if the learning rate is too low?**

The algorithm takes more iterations to reach the minimum.

**Q6. What is convergence?**

Convergence occurs when the algorithm approaches a stable solution.

**Q7. What is a local minimum?**

A point where the function has a smaller value than nearby points.

**Q8. What is the derivative of the given function?**

For \(f(x)=(x+3)^2\), the derivative is \(f'(x)=2(x+3)\).

**Q9. What is the minimum point of the given function?**

The minimum point is x = -3 and the minimum function value is 0.

**Q10. Why do we use tolerance?**

Tolerance determines when the algorithm should stop because the updates have become sufficiently small.

---

# PRACTICAL 4 – CLUSTERING

## 1. Problem Statement

Implement K-Means clustering / Hierarchical clustering on `sales_data_sample.csv` dataset.

Determine the number of clusters using the Elbow Method.

**Dataset:** `sales_data_sample.csv`

## 2. Theory

Clustering is an unsupervised Machine Learning technique used to group similar data points into clusters.

Unlike supervised learning, clustering does not require predefined target labels.

This practical uses two clustering algorithms.

**1. K-Means Clustering**

K-Means divides the dataset into a specified number of clusters.

It assigns data points to the nearest cluster centre and repeatedly updates the cluster centres.

**2. Hierarchical Clustering**

Hierarchical Clustering creates a hierarchy of clusters by repeatedly combining similar data points or groups.

It is commonly represented using a dendrogram.

### Elbow Method

The Elbow Method is used to determine the appropriate number of clusters.

It calculates WCSS (Within-Cluster Sum of Squares) for different values of K.

The graph is plotted between:

- Number of clusters (K).
- Inertia (WCSS).

The point where the reduction in WCSS begins to slow down is known as the elbow point.

**Important Concepts:**

- Unsupervised Learning
- Clustering
- K-Means
- Hierarchical Clustering
- Standardization
- Elbow Method
- Dendrogram
- WCSS

## 3. Algorithm

1. Start.
2. Import the required libraries.
3. Load the sales dataset.
4. Display the dataset shape and first few records.
5. Select numerical features.
6. Standardize the numerical data.
7. Initialize an empty list to store WCSS values.
8. Apply K-Means for different values of K from 1 to 10.
9. Store the inertia value for each cluster count.
10. Plot the Elbow Method graph.
11. Select the number of clusters.
12. Apply K-Means with three clusters.
13. Assign cluster labels to the dataset.
14. Visualize the K-Means clusters.
15. Display cluster centres and cluster counts.
16. Create a linkage matrix for Hierarchical Clustering.
17. Plot the dendrogram.
18. Assign three hierarchical clusters.
19. Display hierarchical cluster counts.
20. Stop.

## 4. Python Code

**Filename:** `practical4.py`

### Block 1 – Importing Libraries

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
from scipy.cluster.hierarchy import linkage, dendrogram, fcluster
import seaborn as sns
```

### Block 2 – Loading Dataset

```python
df = pd.read_csv("sales_data_sample.csv", encoding='latin1', on_bad_lines='skip')
print("Dataset shape:", df.shape)
print(df.head())
```

### Block 3 – Selecting Numerical Features

```python
X = df.select_dtypes(include=['int64', 'float64'])
print("Features used for clustering:\n", X.head())

# Select relevant numeric columns
# X = df[['SALES', 'QUANTITYORDERED', 'PRICEEACH']]

# Handle missing values if any
# X = features.dropna()
```

### Block 4 – Standardizing Data

```python
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

### Block 5 – K-Means Clustering and Elbow Method

```python
# Determine optimal number of clusters using Elbow Method
wcss = []
for k in range(1, 11):
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(X_scaled)
    wcss.append(kmeans.inertia_)

# Plot Elbow Method
plt.figure(figsize=(6,4))
plt.plot(range(1, 11), wcss, marker='o')
plt.title('Elbow Method')
plt.xlabel('Number of clusters (k)')
plt.ylabel('Inertia (WCSS)')
plt.show()

# Fit KMeans with chosen number of clusters (example: 3 clusters)
kmeans = KMeans(n_clusters=3, random_state=42) # Add n_init=10 param in the function to suppress warnings
clusters_kmeans = kmeans.fit_predict(X_scaled)
df['KMeans_Cluster'] = clusters_kmeans

# Visualize clusters
sns.scatterplot(x='SALES', y='PRICEEACH', hue='KMeans_Cluster', data=df, palette='viridis')
plt.title("K-Means Clustering")
plt.show()

print("\nK-Means Cluster Centers:\n", kmeans.cluster_centers_)
print("\nCluster counts:\n", df['KMeans_Cluster'].value_counts())
```

### Block 6 – Hierarchical Clustering

```python
# Create linkage matrix
Z = linkage(X_scaled, method='ward')

# Plot dendrogram
plt.figure(figsize=(10,5))
dendrogram(Z)
plt.title('Hierarchical Clustering Dendrogram')
plt.xlabel('Samples')
plt.ylabel('Distance')
plt.show()

# Assign clusters (example: 3 clusters)
clusters_hier = fcluster(Z, t=3, criterion='maxclust')
df['Hierarchical_Cluster'] = clusters_hier

print("\nHierarchical Cluster counts:\n", pd.Series(clusters_hier).value_counts())
```

## 5. Sample Output

The original practical file does not provide fixed numerical sample output.

The program displays:

- Dataset shape.
- First five dataset records.
- Numerical features selected for clustering.
- Elbow Method graph.
- K-Means cluster visualization.
- K-Means cluster centres.
- K-Means cluster counts.
- Hierarchical Clustering dendrogram.
- Hierarchical cluster counts.

**Note:** The practical uses three clusters as an example. The final number of clusters should be selected by examining the Elbow Method graph.

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Install the required libraries.

```bash
pip install pandas matplotlib scikit-learn scipy seaborn
```

Step 3: Create the Python file.

```bash
nano practical4.py
```

Step 4: Paste the code and save the file.

Step 5: Execute the program.

```bash
python3 practical4.py
```

Make sure `sales_data_sample.csv` is present in the same directory.

## 7. Explanation of the Code

### Part A – Importing Libraries

```python
import pandas as pd
```

Imports Pandas for loading and manipulating the dataset.

```python
import matplotlib.pyplot as plt
```

Imports Matplotlib for plotting graphs.

```python
from sklearn.preprocessing import StandardScaler
```

Imports StandardScaler to standardize numerical features.

```python
from sklearn.cluster import KMeans
```

Imports the K-Means clustering algorithm.

```python
from scipy.cluster.hierarchy import linkage, dendrogram, fcluster
```

Imports the functions required for Hierarchical Clustering.

- `linkage`: Creates the hierarchical clustering structure.
- `dendrogram`: Displays the clustering hierarchy.
- `fcluster`: Assigns cluster labels.

```python
import seaborn as sns
```

Imports Seaborn for cluster visualization.

### Part B – Loading Dataset

```python
df = pd.read_csv("sales_data_sample.csv", encoding='latin1', on_bad_lines='skip')
```

Loads the sales dataset.

- `encoding='latin1'` handles the dataset's character encoding.
- `on_bad_lines='skip'` skips malformed rows.

```python
print("Dataset shape:", df.shape)
```

Displays the number of rows and columns.

```python
print(df.head())
```

Displays the first five records.

### Part C – Selecting Numerical Features

```python
X = df.select_dtypes(include=['int64', 'float64'])
```

Selects only numerical columns from the dataset.

Clustering algorithms require numerical input.

```python
print("Features used for clustering:\n", X.head())
```

Displays the selected numerical features.

### Part D – Standardizing Data

```python
scaler = StandardScaler()
```

Creates a StandardScaler object.

```python
X_scaled = scaler.fit_transform(X)
```

Standardizes the numerical features.

Standardization transforms data so that features have approximately:

- Mean = 0.
- Standard deviation = 1.

It prevents features with larger numerical scales from dominating the distance calculations.

### Part E – Elbow Method

```python
wcss = []
```

Creates an empty list to store WCSS values.

```python
for k in range(1, 11):
```

Runs the clustering algorithm for values of K from 1 to 10.

```python
kmeans = KMeans(n_clusters=k, random_state=42)
```

Creates a K-Means model with k clusters.

```python
kmeans.fit(X_scaled)
```

Trains the K-Means model using standardized data.

```python
wcss.append(kmeans.inertia_)
```

Stores the WCSS value for each cluster count.

```python
plt.plot(range(1, 11), wcss, marker='o')
```

Plots the WCSS values against the number of clusters.

The elbow point helps identify a suitable number of clusters.

### Part F – Applying K-Means

```python
kmeans = KMeans(n_clusters=3, random_state=42)
```

Creates a K-Means model with three clusters.

```python
clusters_kmeans = kmeans.fit_predict(X_scaled)
```

Fits the model and predicts the cluster assigned to each record.

```python
df['KMeans_Cluster'] = clusters_kmeans
```

Stores the cluster labels in the original DataFrame.

### Part G – Visualizing K-Means Clusters

```python
sns.scatterplot(x='SALES', y='PRICEEACH', hue='KMeans_Cluster', data=df, palette='viridis')
```

Creates a scatter plot.

- X-axis represents SALES.
- Y-axis represents PRICEEACH.
- Different colours represent different clusters.

```python
plt.title("K-Means Clustering")
plt.show()
```

Displays the clustering graph.

### Part H – Displaying Cluster Information

```python
print("\nK-Means Cluster Centers:\n", kmeans.cluster_centers_)
```

Displays the centre coordinates of each cluster in the standardized feature space.

```python
print("\nCluster counts:\n", df['KMeans_Cluster'].value_counts())
```

Displays the number of records assigned to each K-Means cluster.

### Part I – Hierarchical Clustering

```python
Z = linkage(X_scaled, method='ward')
```

Creates the linkage matrix using Ward's method.

Ward's method combines clusters while minimizing the increase in within-cluster variance.

```python
dendrogram(Z)
```

Creates the dendrogram.

A dendrogram represents the hierarchical relationships between data points and clusters.

```python
plt.title('Hierarchical Clustering Dendrogram')
plt.xlabel('Samples')
plt.ylabel('Distance')
plt.show()
```

Sets the graph title and axis labels and displays the dendrogram.

### Part J – Assigning Hierarchical Clusters

```python
clusters_hier = fcluster(Z, t=3, criterion='maxclust')
```

Assigns the observations to a maximum of three clusters.

```python
df['Hierarchical_Cluster'] = clusters_hier
```

Stores the hierarchical cluster labels in the dataset.

```python
pd.Series(clusters_hier).value_counts()
```

Counts the number of records in each hierarchical cluster.

## 8. Time and Space Complexity

Let:

- n = Number of data points.
- k = Number of clusters.
- d = Number of numerical features.

### K-Means

| Operation | Complexity |
|---|---|
| One K-Means Iteration | O(n × k × d) |
| K-Means Training | O(n × k × d × i) |
| Elbow Method | O(10 × n × k × d × i) approximately |

Where i represents the number of iterations.

### Hierarchical Clustering

| Operation | Complexity |
|---|---|
| Hierarchical Clustering | O(n² log n) for common implementations |
| Dendrogram Generation | Depends on the number of observations |

**Space Complexity:** Hierarchical clustering can require O(n²) memory for pairwise distances and linkage operations.

## 9. Viva Questions and Answers

**Q1. What is Clustering?**

Clustering is an unsupervised Machine Learning technique used to group similar data points.

**Q2. What is K-Means?**

K-Means is an algorithm that divides data into a specified number of clusters.

**Q3. What is Hierarchical Clustering?**

It is a clustering technique that creates a hierarchy of clusters.

**Q4. What is the Elbow Method?**

It is a technique used to determine a suitable number of clusters by plotting WCSS against the number of clusters.

**Q5. What is WCSS?**

Within-Cluster Sum of Squares measures the sum of squared distances between data points and their cluster centres.

**Q6. What is a centroid?**

A centroid is the centre point of a cluster.

**Q7. Why do we use StandardScaler?**

To bring numerical features to a comparable scale.

**Q8. What is a dendrogram?**

A tree-like diagram that represents the hierarchy of clusters.

**Q9. What is Ward's method?**

A linkage method that merges clusters while minimizing the increase in within-cluster variance.

**Q10. What is the difference between K-Means and Hierarchical Clustering?**

K-Means requires the number of clusters to be specified beforehand, whereas Hierarchical Clustering builds a hierarchy of clusters that can be examined using a dendrogram.

**Q11. What is Unsupervised Learning?**

It is a Machine Learning approach in which the model learns patterns from data without predefined target labels.

**Q12. Can K-Means work with categorical data directly?**

No. Standard K-Means requires numerical features.

---

# END OF PRACTICAL JOURNAL

**Programming Language:** Python 3  
**Subject:** Machine Learning and Data Analytics  
**Environment:** Linux / Ubuntu

