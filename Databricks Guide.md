# Advanced Databricks Deep Dive Guide (Free Edition)

## 1. Introduction to Databricks

Databricks is a cloud-based platform for big data processing and machine learning. It integrates with Apache Spark and provides an interactive workspace for collaboration. This guide covers an in-depth exploration of Databricks Free Edition.

## 2. Setting Up Databricks Community Edition

### Step 1: Sign Up for Databricks Free Edition

1. Go to Databricks Community Edition.
2. Click on "Sign Up".
3. Register with your email and confirm the verification link.
4. Log in to your Databricks workspace.

### Step 2: Understanding the UI

- **Workspace**: Organizes notebooks, libraries, and repos.
- **Clusters**: Virtual machines running Apache Spark.
- **Jobs**: Schedules and automates workloads.
- **Repos**: Version control via Git integration.
- **Data**: Stores and manages tables (Delta Lake support limited in Free Edition).

## 3. Creating and Configuring Clusters

### Step 1: Create a Cluster

1. Go to "Clusters" → "Create Cluster".
2. Provide a name (e.g., `AdvancedDBCluster`).
3. Select Single Node mode (Free Edition limitation).
4. Choose Databricks Runtime 12.2 LTS (or latest available).
5. Click "Create Cluster".

### Step 2: Understanding Cluster Configuration

- **Spark UI & Metrics**: Explore execution plans.
- **Logs & Driver Logs**: Debug issues.
- **Libraries**: Install MLflow, PySpark, Pandas, etc.
- **Cluster Policies**: Not available in Free Edition.

## 4. Working with Notebooks

### Step 1: Create a Notebook

1. Go to "Workspace" → "Create" → "Notebook".
2. Name it `AdvancedDatabricks`.
3. Choose Python as the default language.
4. Attach to your cluster.

### Step 2: Execute Basic Spark Commands

```python
# Check Spark session
print(spark)

# Load sample data
df = spark.read.csv('/databricks-datasets/learning-spark-v2/flights/departuredelays.csv', header=True, inferSchema=True)
df.show(5)
```

### Step 3: SQL & Delta Tables

```sql
CREATE OR REPLACE TEMP VIEW flights_view AS
SELECT origin, destination, delay FROM flights WHERE delay > 100;

SELECT * FROM flights_view;
```

## 5. Data Engineering in Databricks

### Step 1: Optimizing Spark Performance

#### Cache Data:

```python
df.cache()
```

#### Optimize Join Strategies:

```python
from pyspark.sql.functions import broadcast
result = df1.join(broadcast(df2), 'common_column')
```

### Step 2: Working with Delta Lake (Limited in Free Edition)

```python
df.write.mode("overwrite").parquet("/mnt/data/flights.parquet")
df_parquet = spark.read.parquet("/mnt/data/flights.parquet")
df_parquet.show()
```

### Step 3: Automating Jobs

1. Go to "Jobs" → "Create Job".
2. Add a task → Select a notebook.
3. Configure schedule and triggers.
4. Click "Run Now".

## 6. Advanced Integrations

### Step 1: Connect Databricks with GitHub

1. Go to "Repos" → "Add Repo".
2. Connect GitHub with OAuth.
3. Clone your repository and manage version control.

### Step 2: REST API for Databricks

```python
import requests
headers = {"Authorization": "Bearer YOUR_TOKEN"}
response = requests.get("https://community.cloud.databricks.com/api/2.0/clusters/list", headers=headers)
print(response.json())
```

### Step 3: Stream Processing with Structured Streaming

```python
from pyspark.sql.functions import expr
input_stream = (spark
    .readStream
    .format("csv")
    .option("header", "true")
    .schema(df.schema)
    .load("/mnt/streaming-data"))

query = (input_stream
    .writeStream
    .format("console")
    .outputMode("append")
    .start())
```

## 7. Machine Learning & MLflow

### Step 1: Training a ML Model in Databricks

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
import pandas as pd

data = df.toPandas()
X = data[['delay', 'distance']]
y = data['destination']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

model = RandomForestClassifier()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))
```

### Step 2: Tracking Experiments with MLflow

```python
import mlflow
mlflow.start_run()
mlflow.log_param("n_estimators", 100)
mlflow.log_metric("accuracy", accuracy_score(y_test, y_pred))
mlflow.sklearn.log_model(model, "rf_model")
mlflow.end_run()
```

## 8. Best Practices & Limitations of Free Edition

### Limitations:

- Single-node clusters only.
- Limited storage (No DBFS mounts for external storage like AWS S3).
- No production jobs (no jobs API access).
- No access to Unity Catalog.

### Best Practices:

- Use Parquet for efficient storage.
- Optimize Joins with Broadcast Variables.
- Cache data intelligently to improve performance.
- Utilize Databricks Repos for version control.
