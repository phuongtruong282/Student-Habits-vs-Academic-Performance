# The relationship between Student Habits and Academic Performance

## 1. Introduction:

## 2. Purposes and Outcomes:

### 2.1 Purposes:

- Analyze the impact of student habits (study hour per day, social media hours, exercise frequency, …) on Academic performance (Exam score).

- Identify the factors that have the greatest impact on the exam score.

- Develop a regression model to predict exam score based on student habits.

### 2.2 Outcomes:

- Understanding the relationships between student habits and exam score.

- Identify the factors that have the greatest impact on the exam score.

- Prediction model for exam score based on student habits.

- Actionable recommendations for improving exam score.

## 3. Technologies and Tools:

- Python: Data processing and analysis.

- Google Colab: Code execution and presentation.

- Power BI: Utilized for data visualization.

-  GitHub: Code versioning and documentation.

-  PowerPoint: Presentation of results and insights.
## 4. Data Sources:
- Source: Public dataset from Kaggle (Student Habits vs Academic Performance): https://www.kaggle.com/datasets/jayaantanaath/student-habits-vs-academic-performance/data.

- Description: This dataset explores the relationship between student habits and exam score. It contains multiple variables related to study habits, health habits, and enviromental factors.
## 5. Data overview:
|    | student_id   |   age | gender   |   study_hours_per_day |   social_media_hours |   netflix_hours | part_time_job   |   attendance_percentage |   sleep_hours | diet_quality   |   exercise_frequency | parental_education_level   | internet_quality   |   mental_health_rating | extracurricular_participation   |   exam_score |
|---:|:-------------|------:|:---------|----------------------:|---------------------:|----------------:|:----------------|------------------------:|--------------:|:---------------|---------------------:|:---------------------------|:-------------------|-----------------------:|:--------------------------------|-------------:|
|  0 | S1000        |    23 | Female   |                   0   |                  1.2 |             1.1 | No              |                    85   |           8   | Fair           |                    6 | Master                     | Average            |                      8 | Yes                             |         56.2 |
|  1 | S1001        |    20 | Female   |                   6.9 |                  2.8 |             2.3 | No              |                    97.3 |           4.6 | Good           |                    6 | High School                | Average            |                      8 | No                              |        100   |
|  2 | S1002        |    21 | Male     |                   1.4 |                  3.1 |             1.3 | No              |                    94.8 |           8   | Poor           |                    1 | High School                | Poor               |                      1 | No                              |         34.3 |
|  3 | S1003        |    23 | Female   |                   1   |                  3.9 |             1   | No              |                    71   |           9.2 | Poor           |                    4 | Master                     | Good               |                      1 | Yes                             |         26.8 |
|  4 | S1004        |    19 | Female   |                   5   |                  4.4 |             0.5 | No              |                    90.9 |           4.9 | Fair           |                    3 | Master                     | Good               |                      1 | No                              |         66.4 |


### Structure:
- exam_score: The final exam score of each student.
- student_id
- age: The age of the student.
- gender: Indicates the student’s gender.
- study_hours_per_day: the number of hours spent studying per day.
- social_media_hours: the number of hours spent on social media daily.
-  netflix_hours: the number of hours spent watching Netflix per day.
- part_time_job: Indicates whether the student has a part-time job.
- attendance_percentage: Percentage of classes attended by the student.
- sleep_hours: the number of hours of sleep per night.
- diet_quality: Quality of the student's diet.
- exercise_frequency: Number of exercise sessions per week.
- parental_education_level: Highest education level attained by parents.
-  internet_quality: Describes the quality of the student's internet connection.
- mental_health_rating: A self-reported mental health score.
- extracurricular_participation: Indicates if the student is involved in extracurricular activities.
### Summary:
![data summary](images/data_summary.png)
## 5. Data cleaning:
### 5.1 Removing duplicates
- Using python code to check duplicates. There are no duplicate rows in this dataset.
### 5.2 Handling missing values
- The number of missing values in all columns:
  
![checking missing values](images/checking_missing_values.png)
- Handling missing values in the parental_education_level cloumns by replace it with mode value.
```python
# Filling missing values in the 'parental_education_level' column with the most frequent value (mode)
df['parental_education_level'] = df['parental_education_level'].fillna(df['parental_education_level'].mode()[0])
```
### 5.3 Ensuring consistency
- Check unique values in categorical columns to ensure consistency.
### 5.4 Fixing incorrect data types
- Converting all categorical columns in the Data Frame to the 'category' data type
### 5.5 Identifying and handling outliers
- Outlier Detection & Removal using IQR:
```python
# Outlier Detection & Removal using IQR
def  remove_outliers_iqr(data, column):
Q1 = data[column].quantile(0.25)
Q3 = data[column].quantile(0.75)
IQR = Q3 - Q1
lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR
return data[(data[column] >= lower) & (data[column] <= upper)]
# Apply to selected numerical columns
numeric_cols = df.select_dtypes(include=['number']).columns.tolist()
for col in numeric_cols:
df = remove_outliers_iqr(df, col)
```
## 6. Exploratory data analysis
### 6.1  Summary Statistics
- Now, let's take a look at some descriptive information of this dataset after cleaning:
 ![data cleaned describe](images/Data_cleaned_describe.png)
 **Observations**
-   Average exam score: 69.5 (range: 26.2 → 100.0)
-   Average study time: 3.53 hours/day, with some students studying 0 hours and others up to 7.3 hours
-   Average social media use: 2.5 hours/day (up to 5.6 hours)
-   Average netflix use: 1.8 hours/day (up to 4.6 hours)
-   Average attendance: 84.1% (up to 100%)
-   Average sleep: 6.5 hours (minimum 3.2h, maximum 9.8h)
-   Average exercise: 3 (range: 0 -> 6)
-   Average mental health rating: 5.4 (range: 1 -> 10)
### 6.2 Exploring the distribution of objective variable
- Using histogram to visualize the distribution of exam_scores (objective variable):
![](images/Histogram_exam_score.png)

**Observation**
- The distribution is fairly normal, with a slight left skew (a slight skew toward lower scores).
- Many students have exam scores ranging from 60 to 80.
- Some students scored very low (20-30), while others achieved a perfect score of 100.
### 6.3 Correlation Analysis
- Creating a heatmap to visualize correlations:
![](images/Heatmap.png)

**Key Correlations:**
-  exam_Score and study_hours_per_day: +0.82 Very strong => Studying more is closely associated with higher exam scores.
-  exam_score and mental_health_rating: +0.32 Moderate => Better mental health tends to support more effective learning.
-  exam_score and exercise_frequency: +0.15 Weak => Regular exercise has a slight positive effect.
-  exam_score and sleep_hours: +0.13 Weak => Sleep enough has a slight positive effect.
-  exam_score and social_media_hours/netflix_hours: -0.17 Weak – There is a slight negative correlation, suggesting that spending more time for social media or watching more Netflix tends to be associated with lower exam scores.
  
**Conclusion**
-  Study hours have the strongest impact on exam performance
-  Mental health is the only health-related factor with a noticeable correlation to academic success.
- Sleep and exercise play a role but are not strongly influential – further analysis is needed, especially by comparing high vs. low performers.
-  There is a weak negative correlation between entertainment time (Netflix, social media) and exam scores. This suggests that spending too much time on leisure may slightly reduce academic performance. While not a direct cause, it highlights the importance of balancing study and entertainment for better results.
### 6.4 Exploring the relationship between exam score and study habits
In the scope of this project and dataset, the factors considered as study habits include:

- study_hours_per_day
- social_media_hours
- netflix_hours
- part_time_job
- attendance_percentage
- extracurricular_participation.

**Using Power BI to visualize the relationship between exam _score and study habits:**
![](images/Corr_Studyhours_score.png)
  

### 6.5 Exploring the relationship between exam score and health habits
### 6.6 Exploring the relationship between exam score and related environmental factors
#### 6.6.1 Exam_score and parental_education_level

![](images/Relationship_parental_education_score.png)
**Observations**

-   There are three groups: Master, High School, and Bachelor.
-   The Bachelor group has the highest median score (72), followed by High School, and lastly Master (66).
-   The score distribution of the Master group is skewed toward lower scores.
-   The Bachelor group tends to have higher scores, with more stable distribution.

**Conclusion:**

-   The group with parents holding a Bachelor's or High School education has a slightly higher median score compared to the Master's group, which is contrary to common expectations.This could be due to other factors such as expectations or pressure.

=> Parental education level does not have a linear relationship with exam results. It may have an indirect influence, but it is not a determining factor.
Observations

All three groups (Poor, Average, Good) have similar median scores (around 70).

The Average group has the highest median, followed by Poor, and lastly Good (which is surprising, as expectations usually suggest the opposite).
## 7. Data modeling
## 8. Outcomes:
