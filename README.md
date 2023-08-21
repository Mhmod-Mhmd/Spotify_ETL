# Data Engineering Project|Building Spotify ETL using Python and Airflow

Create an Extract Transform Load pipeline using python and automate with airflow.

In this repo, will explain how to create a simple ETL(Extract, Transform, Load) pipeline using Python and automate the process through Apache airflow.

# Problem Statement:

 use Spotify’s API to read the data and perform some basic transformations and Data Quality checks finally will load the retrieved data to PostgreSQL DB and then automate the entire process through airflow.  **Est.Time:**[4–7 Hours]

# Tech Stack / Skill used:

1.  Python
2.  API’s
3.  Docker
4.  Airflow
5.  PostgreSQL

# Learning Outcomes:

1.  Understand how to interact with API to retrieve data
2.  Handling Dataframe in pandas
3.  Setting up Airflow and PostgreSQL through Docker-Compose.
4.  Learning to Create DAGs in Airflow

# Introduction:

This is a project to get started with building a simple pipeline and automating through airflow. First, i focused on entirely building the pipeline and then extend the project by combining it with Airflow.

# Building ETL Pipeline:

**Dataset:** In this project, we are using Spotify’s API. 

## Extract.py

We are using this token to Extract the Data from Spotify. We are Creating a function return_dataframe(). The Below python code explains how we extract API data and convert it to a Dataframe.

## Transform.py

Here we are exporting the Extract file to get the data.

**def Data_Quality(load_df):** Used to check for the empty data frame, enforce unique constraints, checking for null values. Since these data might ruin our database it's important we enforce these Data Quality checks.

**def Transform_df(load_df):** Now we are writing some logic according to our requirement here we wanted to know our favorite artist so we are grouping the songs listened to by the artist.

## Load.py

In the load step, we are using sqlalchemy and SQLite to load our data into a database and save the file in our project directory.

Finally, we have completed our ETL pipeline successfully.


After running the  **Load.py**  you could see a .sqlite file will be saved to the project folder, to check the data inside the file head  [here](https://inloop.github.io/sqlite-viewer/)  and drop your file.

![]()


Now we will automate this process using Airflow.

# Automating through Airflow

create the required Dag for our project.

![]()



So inside our dag, we need to create tasks to get our job done. To keep it simple I will use two tasks i.e. one to create Postgres Table and another to load the Data to the Table.

![]()

## spotify_etl.py

In this Python File will write a logic to extract data from API → Do Quality Checks →Transform Data.

1.  **yesterday = today — datetime.timedelta(days=1)**  → Defines the number of days you want data for, change as you wish since our job is the daily load I have set it to 1.
2.  **def spotify_etl()**  → Core function which returns the Data Frame to the DAG python file.
3.  This file needs to be placed inside the dags folder

## spotify_final_dag.py


1.  **from airflow.operators.python_operator import PythonOperator**  → we are using the python operator to perform python functions such as inserting DataFrame to the table.
2.  **from airflow.providers.postgres.operators.postgres import PostgresOperator**  → we are using the Postgres operator to create tables in our Postgres database.
3.  **from airflow. hooks.base_hook import BaseHook**  → A hook is an abstraction of a specific API that allows Airflow to interact with an external system. Hooks are built into many operators, but they can also be used directly in DAG code. We are using a hook here to connect Postgres Database from our python function
4.  **from spotify_etl import spotify_etl**  → Importing spotify_etl function from spotify_etl.py

## Code Explanation:

Setting up the default arguments and interval time. We can change the interval time start date according to our needs.

![]()

Understanding Postgres connection and task.

1.  **conn = BaseHook.get_connection(‘[Your Connection ID]’)**  → Connects to your Postgres DB.
2.  **df.to_sql(‘[Your Table Name]’, engine, if_exists=’replace’)**  → Loads the DF to the table
3.  **create_table >> run_etl**  → Defining the flow of the task

![]()
