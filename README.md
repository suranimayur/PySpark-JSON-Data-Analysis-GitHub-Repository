# PySpark-JSON-Data-Analysis-GitHub-Repository
PySpark JSON Data Analysis GitHub Repository


## Project Summary:
This project aims to demonstrate how to perform multidimensional data analysis using PySpark with JSON data. It covers various aspects such as ingesting JSON data, representing hierarchical data, reducing duplication, and creating/unpacking data from complex data types.

## Project Report:
The project report includes detailed explanations, code snippets, and analyses for performing data analysis on JSON documents using PySpark. It covers the following topics:

## Reading JSON Data: Exploring different approaches to ingest JSON data into PySpark DataFrames.
* **Starting Small:** Understanding JSON data as a limited Python dictionary and basic operations.
* **Nested Structures:** Handling nested structures within JSON data and accessing nested elements.
* **Arrays:** Working with arrays in JSON data, extracting elements, and performing operations.
* **Maps:** Creating and using maps from two arrays to represent key-value pairs.
* **Structs:** Understanding and navigating structs within columns, representing objects in JSON.
* **Building** and Using Schema: Defining and using schemas explicitly for JSON data ingestion.
* **Exploding and Collecting:** Exploding arrays and maps to unnest hierarchical data and collecting results back into an array.
* **Creating Hierarchies:** Utilizing struct as a function to create structured columns.


PySpark Codes Functions Used in Repo:

**1. Reading JSON Data:**
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("JSON_Data_Ingestion").getOrCreate()

#### Read JSON data into DataFrame
shows = spark.read.json("path/to/json/file.json")

**2. Starting Small:**

import json

sample_json = """{
                    "id": 143,
                    "name": "Silicon Valley",
                    "type": "Scripted",
                    "language": "English",
                    "genres": ["Comedy"],
                    "network": {
                    "id": 8,
                    "name": "HBO",
                    "country": {
                    "name": "United States",
                    "code": "US",
                    "timezone": "America/New_York"
                                }
                                }
}"""

document = json.loads(sample_json)
print(type(document))

**3. Nested Structures:**

shows.printSchema()
print(shows.columns)

**4. Arrays:**

array_subset = shows.select("name","genres")
array_subset.show(5,False)

**5. Maps:**

columns = ["name", "language", "type"]

shows_map = shows.select(
    *[F.lit(column) for column in columns],
    F.array(*columns).alias("values"),
)

shows_map.show()

**6. Structs:**

shows.select("schedule").printSchema()
shows.select(F.col("_embedded")).printSchema()

**7. Building and Using Schema:**

episode_schema = T.StructType([...])  # Define schema for episodes
embedded_schema = T.StructType([...])  # Define schema for embedded data

shows_with_schema = spark.read.json("path/to/json/file.json", schema=embedded_schema, mode="FAILFAST")

**8. Exploding and Collecting:**

episodes = shows.select("id", F.explode("_embedded.episodes").alias("episodes"))
episodes.show(5, truncate=70)

**9. Creating Hierarchies:**

struct_ex = shows.select(
    F.struct(
        F.col("status"),F.col("weight"),F.lit(True).alias("has_watched")
    ).alias("info")
)
struct_ex.show(1, False)


## Final Summary Conclusion:

PySpark provides powerful tools for analyzing JSON data, allowing users to efficiently handle hierarchical and nested structures. By leveraging PySpark's functionalities, data analysts and engineers can perform multidimensional data analysis, reduce data duplication, and build sophisticated data pipelines for processing JSON documents effectively. Understanding how to work with complex data types in PySpark opens up a wide range of possibilities for analyzing diverse datasets, making it a valuable skill for any data professional.
