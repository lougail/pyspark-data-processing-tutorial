# PySpark Data Processing Tutorial

Interactive Jupyter notebook tutorial for learning Apache Spark with Python. This hands-on guide covers fundamental to intermediate PySpark concepts with real-world data processing examples.

## 📋 Overview

Comprehensive tutorial demonstrating distributed data processing using PySpark, including DataFrames, SQL queries, RDDs, and various output formats. Perfect for beginners and intermediate learners.

## 🎯 What You'll Learn

### Core Concepts
- **Apache Spark Architecture** - Understanding Driver, Executors, and Cluster Manager
- **PySpark Basics** - Python interface to Spark (Py4J bridge)
- **Lazy Evaluation** - How Spark optimizes query execution
- **Spark Web UI** - Monitoring jobs at `http://localhost:4040`

### Practical Skills
1. **Setting Up Spark**
   - Creating SparkSession
   - Local execution with `master("local[*]")`
   - Configuration options

2. **Loading Data**
   - Reading CSV files with schema inference
   - Inspecting DataFrames (`.printSchema()`, `.show()`)
   - Understanding DataFrame structure

3. **Data Manipulation**
   - Selecting columns (`.select()`)
   - Filtering data (`.filter()`, `.where()`)
   - Sorting (`.orderBy()`)
   - Creating computed columns (`.withColumn()`)

4. **Data Cleaning**
   - Normalizing text (`.upper()`, `.trim()`)
   - Handling NULL values (`.na.fill()`, `.when().otherwise()`)
   - Fixing inconsistent data (department names, missing salaries)
   - Creating categorical columns (salary bands)

5. **Aggregations**
   - Grouping data (`.groupBy()`)
   - Aggregate functions (`count`, `avg`, `sum`, `min`, `max`)
   - Multiple aggregations in single query

6. **Spark SQL**
   - Creating temporary views (`.createOrReplaceTempView()`)
   - Writing SQL queries on DataFrames
   - `GROUP BY` and aggregations in SQL

7. **Output Formats**
   - Writing CSV files (`.write.csv()`)
   - Writing Parquet files (columnar format)
   - Overwrite vs append modes

8. **RDD Operations**
   - Low-level Resilient Distributed Datasets
   - Transformations (`.map()`, `.filter()`)
   - Actions (`.collect()`, `.count()`)
   - When to use RDDs vs DataFrames

## 📊 Dataset

**File:** `data/employees.csv`

**Structure:**
```csv
employee_id,name,age,department,salary,experience_years
1,John Doe,30,IT,65000,5
2,Jane Smith,28,HR,55000,3
...
```

**Characteristics:**
- 10,000+ employee records
- Intentional data quality issues for cleaning practice:
  - Missing salary values (NULL)
  - Inconsistent department capitalization
  - Extra whitespace in fields
  - Mixed case formatting

**Departments:** IT, HR, Sales, Marketing, Finance, R&D

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Java 8 or 11 (required by Spark)
- 4GB+ RAM recommended

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/lougail/pyspark-data-processing-tutorial.git
cd pyspark-data-processing-tutorial
```

2. **Create virtual environment**
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

### Running the Tutorial

1. **Start Jupyter Notebook**
```bash
jupyter notebook intro_pyspark.ipynb
```

2. **Run cells sequentially** - Execute each cell in order to follow the tutorial progression

3. **Access Spark UI** - While Spark is running, visit `http://localhost:4040` to monitor jobs

## 📚 Tutorial Structure

The notebook is organized into 8 main sections with 69 interactive cells:

### Section 1: Introduction
- Spark vs Hadoop comparison
- Architecture overview
- PySpark setup

### Section 2: Starting Spark
- SparkSession creation
- Local mode configuration
- Web UI introduction

### Section 3: Loading Data
- Reading CSV with options
- Schema inspection
- Understanding lazy evaluation

### Section 4: Basic Operations
- Selecting specific columns
- Filtering rows with conditions
- Sorting results

### Section 5: Spark SQL
- Creating temporary views
- SQL queries on DataFrames
- Aggregations with SQL

### Section 6: Data Cleaning
- Text normalization
- NULL handling strategies
- Creating derived columns
- Salary band categorization

### Section 7: Aggregations
- GroupBy operations
- Multiple aggregate functions
- Ordering aggregated results

### Section 8: Output & RDDs
- Writing CSV and Parquet files
- Reading Parquet files
- RDD basics and operations

## 🛠️ Technologies

- **Apache Spark 3.5.3** - Distributed computing framework
- **PySpark** - Python API for Spark
- **Jupyter Notebook** - Interactive development environment
- **NumPy 1.26.4** - Numerical computing library
- **Py4J** - Python-Java bridge

## 💡 Key Examples

### Example 1: Average Salary by Department
```python
# DataFrame API
df_clean.groupBy("department") \
    .agg(F.avg("salary").alias("avg_salary")) \
    .orderBy(F.desc("avg_salary")) \
    .show()

# SQL
spark.sql("""
    SELECT department, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department
    ORDER BY avg_salary DESC
""").show()
```

### Example 2: Data Cleaning Pipeline
```python
df_clean = df \
    .withColumn("department", F.upper(F.trim(F.col("department")))) \
    .withColumn("salary",
                F.when(F.col("salary").isNull(), 50000)
                 .otherwise(F.col("salary"))) \
    .na.fill({"experience_years": 0})
```

### Example 3: Creating Salary Bands
```python
df_with_bands = df_clean.withColumn(
    "salary_band",
    F.when(F.col("salary") < 40000, "Junior")
     .when(F.col("salary") < 70000, "Mid-Level")
     .otherwise("Senior")
)
```

## 📈 Learning Path

1. **Beginners:** Start from Section 1, follow sequentially
2. **Intermediate:** Jump to Sections 5-6 for SQL and data cleaning
3. **Advanced:** Focus on Section 8 for RDD operations and optimizations

## 🎓 Best Practices Demonstrated

- **Lazy Evaluation:** Understanding transformations vs actions
- **Schema Inference:** Automatic type detection from CSV
- **Data Cleaning:** Real-world data quality issues
- **Performance:** Using Parquet for efficient storage
- **SQL Integration:** Seamless SQL queries on DataFrames
- **Monitoring:** Using Spark UI for job inspection

## 🐛 Troubleshooting

### Java Not Found
```bash
# Install Java 8 or 11
# Set JAVA_HOME environment variable
export JAVA_HOME=/path/to/java
```

### Py4J Errors
- Ensure PySpark version matches Spark version
- Check Python environment variables
- Restart Jupyter kernel

### Memory Issues
```python
# Increase driver memory
spark = SparkSession.builder \
    .config("spark.driver.memory", "4g") \
    .getOrCreate()
```

## 📚 Additional Resources

Included in notebook:
- Official Apache Spark documentation links
- PySpark API reference
- Spark SQL guide
- RDD programming guide

## 🤝 Contributing

This is an educational project. Suggestions for improvements:
- Additional exercises
- More complex transformations
- Real-world use cases
- Performance optimization examples

Feel free to fork and create pull requests!

## 📝 License

MIT License - Free for educational and commercial use

## 🎯 Next Steps

After completing this tutorial:
1. Explore Spark MLlib for machine learning
2. Learn about Spark Streaming for real-time processing
3. Study performance optimization and partitioning
4. Deploy Spark on cloud platforms (Azure, AWS, GCP)

## 📧 Contact

Questions or feedback? Open an issue on GitHub!

---

**Happy Learning with PySpark!** 🚀
