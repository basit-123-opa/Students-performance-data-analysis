# Students-performance-data-analysis
Exploratory data analysis of student academic performance using Python,Panda,NumPy, and Matplotlib
1. Project Overview
2. Import Libraries
3. Load Dataset
4. Section A — Basic Exploration
5. Section B — Filtering & Conditional Selection
6. Section C — GroupBy & Aggregation
7. Section D — Data Visualization
8. Section E — Interpretation
9. Conclusion
# Pandas Data Analysis Assessment

## Objective

This assessment explores students' academic performance using **Pandas, NumPy, and Matplotlib**. The analysis covers data exploration, filtering, conditional selection, aggregation, and visualization.

---

# Section A — Basic Exploration

## 1. Import the necessary libraries and read the dataset

```python
# Import the required libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Read the StudentsPerformance.csv dataset
df = pd.read_csv("StudentsPerformance.csv")

# Display the first few rows to confirm that the dataset loaded correctly
df.head()
```

---

## 2. Display the first 10 rows and last 10 rows

```python
# Display the first 10 rows
print("First 10 rows:")
display(df.head(10))

# Display the last 10 rows
print("Last 10 rows:")
display(df.tail(10))
```

**Answer:**
The first 10 rows show the records at the beginning of the dataset, while the last 10 rows show the records at the end.

---

## 3. Check the dataset overview using `.info()`

```python
# Display information about the dataset
df.info()
```

**Expected overview:**

* Number of entries: **1000**
* Number of columns: **8**
* Numerical columns: **3**
* Categorical columns: **5**
* There are no missing values in the dataset.

---

## 4. Check the shape of the dataset

```python
# Display the number of rows and columns
print("Dataset shape:", df.shape)
```

**Answer:**

```text
(1000, 8)
```

This means the dataset contains **1,000 rows and 8 columns**.

---

## 5. List all column names

```python
# Display all column names
print(df.columns.tolist())
```

**Answer:**

```text
[
    'gender',
    'race/ethnicity',
    'parental level of education',
    'lunch',
    'test preparation course',
    'math score',
    'reading score',
    'writing score'
]
```

---

## 6. Identify numerical and categorical columns

```python
# Display the data types of all columns
print(df.dtypes)
```

### Numerical columns

```text
math score
reading score
writing score
```

### Categorical columns

```text
gender
race/ethnicity
parental level of education
lunch
test preparation course
```

We can also identify them programmatically:

```python
# Get numerical columns
numerical_columns = df.select_dtypes(include=np.number).columns.tolist()

# Get categorical columns
categorical_columns = df.select_dtypes(exclude=np.number).columns.tolist()

print("Numerical columns:")
print(numerical_columns)

print("\nCategorical columns:")
print(categorical_columns)
```

---

## 7. Display descriptive statistics for numerical columns

```python
# Generate descriptive statistics for numerical columns
df.describe()
```

The `.describe()` function provides statistics such as:

* Count
* Mean
* Standard deviation
* Minimum
* 25th percentile
* Median
* 75th percentile
* Maximum

For example, the approximate mean scores are:

| Subject |  Mean |
| ------- | ----: |
| Math    | 66.09 |
| Reading | 69.17 |
| Writing | 68.05 |

---

## 8. Check for missing values

```python
# Check the number of missing values in each column
print(df.isnull().sum())
```

To check whether there are missing values anywhere in the dataset:

```python
# Check whether the dataset contains any missing values
print("Total missing values:", df.isnull().sum().sum())
```

**Answer:**

```text
Total missing values: 0
```

Therefore, the dataset contains **no missing values**.

---

# Section B — Filtering & Conditional Selection

## 1. Select all students who completed the test preparation course

```python
# Select students who completed the test preparation course
completed_preparation = df[
    df["test preparation course"] == "completed"
]

# Display the selected students
display(completed_preparation)
```

To find the number of students:

```python
print("Number of students who completed the course:",
      len(completed_preparation))
```

**Answer:**
**358 students** completed the test preparation course.

---

## 2. Find all students who scored above 70 in mathematics

```python
# Select students who scored above 70 in mathematics
math_above_70 = df[df["math score"] > 70]

# Display the results
display(math_above_70)
```

To count them:

```python
print("Number of students scoring above 70 in math:",
      len(math_above_70))
```

---

## 3. Retrieve female students who scored at least 65 in all three subjects

We use the `&` operator to combine multiple conditions.

```python
# Select female students who scored at least 65
# in mathematics, reading, and writing
female_high_scores = df[
    (df["gender"] == "female") &
    (df["math score"] >= 65) &
    (df["reading score"] >= 65) &
    (df["writing score"] >= 65)
]

# Display the selected students
display(female_high_scores)
```

To count them:

```python
print("Number of female students meeting the requirement:",
      len(female_high_scores))
```

---

## 4. Count how many students have free/reduced lunch

```python
# Count students with free/reduced lunch
free_reduced_lunch = (df["lunch"] == "free/reduced").sum()

print("Number of students with free/reduced lunch:",
      free_reduced_lunch)
```

**Answer:**

```text
355 students
```

---

# Section C — GroupBy & Aggregation

## 1. Compute average math score by gender

```python
# Calculate the average mathematics score for each gender
average_math_by_gender = df.groupby("gender")["math score"].mean()

print(average_math_by_gender)
```

**Expected result:**

```text
gender
female    63.633205
male      68.728216
```

Therefore, male students have a higher average mathematics score in this dataset.

---

## 2. Compute average math, reading, and writing scores by parental level of education

```python
# Calculate average scores grouped by parental education level
average_scores_education = df.groupby(
    "parental level of education"
)[
    ["math score", "reading score", "writing score"]
].mean()

# Display the results
display(average_scores_education)
```

This allows us to compare students' average performance across different parental education levels.

---

## 3. Compare average scores of students with and without test preparation

```python
# Calculate average scores based on test preparation status
# Calculate average scores based on test preparation status
    "test preparation course"
)[
    ["math score", "reading score", "writing score"]
].mean()

# Display the results
display(test_prep_comparison)
```

The expected pattern is:

| Test preparation |   Math | Reading | Writing |
| ---------------- | -----: | ------: | ------: |
| None             | ~64.08 |  ~66.53 |  ~64.50 |
| Completed        | ~69.70 |  ~73.89 |  ~74.42 |

**Observation:**
Students who completed the test preparation course have higher average scores in mathematics, reading, and writing than students who did not complete the course.

---

# Section D — Visualization

## 1. Histogram of Mathematics Scores

```python
# Create a histogram for mathematics scores
plt.figure(figsize=(8, 5))

plt.hist(df["math score"], bins=10)

plt.title("Distribution of Mathematics Scores")
plt.xlabel("Math Score")
plt.ylabel("Number of Students")

plt.show()
```

---

## Histogram of Reading Scores

```python
# Create a histogram for reading scores
plt.figure(figsize=(8, 5))

plt.hist(df["reading score"], bins=10)

plt.title("Distribution of Reading Scores")
plt.xlabel("Reading Score")
plt.ylabel("Number of Students")

plt.show()
```

---

## Histogram of Writing Scores

```python
# Create a histogram for writing scores
plt.figure(figsize=(8, 5))

plt.hist(df["writing score"], bins=10)

plt.title("Distribution of Writing Scores")
plt.xlabel("Writing Score")
plt.ylabel("Number of Students")

plt.show()
```

---

## 2. Bar plot showing average score by gender

For this plot, we can calculate the average score across all three subjects for each gender.

```python
# Calculate the average of the three subject scores for each student
df["average score"] = df[
    ["math score", "reading score", "writing score"]
].mean(axis=1)

# Calculate the overall average score by gender
average_score_gender = df.groupby("gender")["average score"].mean()

# Create the bar plot
plt.figure(figsize=(8, 5))

plt.bar(
    average_score_gender.index,
    average_score_gender.values
)

plt.title("Average Student Score by Gender")
plt.xlabel("Gender")
plt.ylabel("Average Score")

plt.show()
```

---

# Section E — Interpretation

## Does test preparation appear to improve performance?

The analysis shows that students who completed the test preparation course achieved higher average scores in mathematics, reading, and writing than students who did not complete the course. The difference is particularly noticeable in reading and writing scores. Therefore, test preparation appears to be associated with improved academic performance in this dataset. However, this analysis shows an association and does not by itself prove that the test preparation course directly caused the improvement.

---

# Conclusion

The analysis demonstrated how Pandas can be used to explore, filter, and aggregate a dataset. The StudentsPerformance dataset contains 1,000 students and has no missing values. GroupBy analysis showed differences in academic performance based on gender, parental education, and test preparation. Matplotlib was also used to visualize the distributions of students' scores and compare average performance by gender.
