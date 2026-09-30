# Week 3 – Python & Data Wrangling

## Data Analyst Internship

This repository contains the completed Week 3 Python & Data Wrangling assignment.

## Assignment Overview

The Week 3 assignment covers Python basics, Pandas data manipulation, CSV reading, filtering, merging, and introductory Matplotlib and Seaborn visualization. The assignment specifically requires cleaning a messy dataset in Pandas, handling missing values, filtering rows, and creating new columns.

## Dataset

**Source file:** `data.csv`

### Original dataset

- Rows: 32
- Columns: 5
- Fields: `Duration`, `Date`, `Pulse`, `Maxpulse`, `Calories`

The dataset contains workout/activity measurements including duration, date, pulse, maximum pulse, and calories.

## Data Cleaning Performed

### 1. Initial inspection

The dataset was loaded and inspected using Pandas:

```python
df = pd.read_csv("data.csv")
df.head()
df.shape
df.info()
df.describe()
```

### 2. Date standardization

The `Date` column contained mixed date representations. It was converted to Pandas datetime format:

```python
df["Date"] = pd.to_datetime(
    df["Date"].astype(str).str.strip("'"),
    format="mixed",
    errors="coerce"
)
```

One missing date was restored as `2020-12-22`, based on the sequential date pattern in the supplied dataset.

### 3. Missing values

Original missing values were:

| Column | Missing values |
|---|---:|
| Duration | 0 |
| Date | 1 |
| Pulse | 0 |
| Maxpulse | 0 |
| Calories | 2 |

The two missing `Calories` values were replaced using the median of the `Calories` column:

```python
df["Calories"] = df["Calories"].fillna(df["Calories"].median())
```

### 4. Duplicate records

The dataset contained 1 duplicate record(s). Duplicate rows were removed using:

```python
df = df.drop_duplicates()
```

### 5. New analytical columns

#### Calories per minute

```python
df["Calories_per_Minute"] = df["Calories"] / df["Duration"]
```

This provides a normalized measure of calorie expenditure relative to workout duration.

#### Pulse category

```python
df["Pulse_Category"] = pd.cut(
    df["Pulse"],
    bins=[0, 99, 109, float("inf")],
    labels=["Low", "Moderate", "High"]
)
```

This groups pulse measurements into simple analytical categories.

## Filtering Analysis

### High-pulse records

```python
high_pulse = df[df["Pulse"] > 110]
```

### High-calorie records

```python
high_calories = df[df["Calories"] > 300]
```

### Long workouts

```python
long_workouts = df[df["Duration"] >= 60]
```

### Long workouts with high calorie expenditure

```python
filtered_data = df[
    (df["Duration"] >= 60) &
    (df["Calories"] > 300)
]
```

These filters demonstrate how Pandas can isolate records satisfying one or multiple conditions.

## Sorting and Summary Analysis

The cleaned dataset was sorted to identify the highest calorie-burning observations:

```python
df.sort_values("Calories", ascending=False).head(10)
```

Summary statistics were generated with:

```python
df[["Duration", "Pulse", "Maxpulse", "Calories"]].mean()
df[["Duration", "Pulse", "Maxpulse", "Calories"]].min()
df[["Duration", "Pulse", "Maxpulse", "Calories"]].max()
```

## Visualization

Matplotlib and Seaborn were used for exploratory analysis.

### Calories vs Duration

A scatter plot was created to examine the relationship between workout duration and calories burned:

```python
plt.scatter(df["Duration"], df["Calories"])
plt.xlabel("Duration (minutes)")
plt.ylabel("Calories")
plt.title("Calories Burned vs Workout Duration")
plt.show()
```

### Calories distribution

A Seaborn histogram was used to visualize the distribution of calories:

```python
sns.histplot(df["Calories"], bins=10, kde=True)
```

### Pulse distribution

A Seaborn boxplot was used to inspect pulse values and visually identify unusual observations:

```python
sns.boxplot(y=df["Pulse"])
```

## Outlier Observation

The supplied dataset contains a notably high `Duration` value of 450 minutes compared with most other duration values.

This value was not automatically deleted. It was retained in the cleaned dataset and treated as a potential outlier requiring contextual validation rather than assuming that every large value is an error.

## Final Cleaned Dataset

After cleaning:

- Final rows: 31
- Final columns: 7
- Missing values remaining: 0
- Duplicate rows remaining: 0

Final fields:

1. `Duration`
2. `Date`
3. `Pulse`
4. `Maxpulse`
5. `Calories`
6. `Calories_per_Minute`
7. `Pulse_Category`

## Tools Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Google Colab

## Recommended Repository Structure

```text
Week 3 Python Data Wrangling/
│
├── 01 Dataset/
│   └── data.csv
│
├── 02 Notebook/
│   └── Week 3 Python Data Wrangling.ipynb
│
├── 03 Cleaned_Data/
│   └── Week 3 Cleaned Data.csv
│
├── 04 Visualizations/
│   ├── Calories vs Duration.png
│   ├── Calories Distribution.png
│   └── Pulse Distribution.png
│
├── 05 Documentation/
│   └── README.md
│
└── 06 Submission/
    └── Week 3 Python Data Wrangling.pdf
```

## Final Conclusion

The Week 3 dataset was processed using Python and Pandas. The workflow included loading and inspecting the CSV, identifying and handling missing values, standardizing date values, removing duplicate records, filtering records based on analytical conditions, and creating new calculated and categorical columns.

Matplotlib and Seaborn were used to visualize the cleaned data and support exploratory analysis. The resulting cleaned dataset is ready for further analysis, while the notebook documents the Python-based data-wrangling process.

## Assignment Reference

This work is based on the supplied **Data Analyst Course – Week 3: Python & Data Wrangling** assignment brief.


## Done by Kirti Sambyal (Github: kirti220)
