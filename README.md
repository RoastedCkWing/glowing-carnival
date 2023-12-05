# 5310 Fall 2023 Final Project
Airline satisfaction tables design and ETL Procedures
Reading and Initial Processing of Data

The script begins by importing necessary libraries: pandas for data manipulation, sqlalchemy for database interaction, and psycopg2, a PostgreSQL database adapter for Python. The code reads a CSV file named 'Airline Passenger Satisfaction.csv' into a DataFrame, df. Initial data exploration is conducted using df.head and df.info(), which provide an overview of the data's structure, such as column names and data types. The script identifies and removes duplicate IDs, ensuring data uniqueness. An unnamed column, likely an artifact of the data export process, is dropped for data cleanliness.

Database Connection and Table Creation

Next, the script establishes a connection to a PostgreSQL database using a connection URL and the create_engine method from sqlalchemy. This connection allows for direct SQL operations on the database. Several CREATE TABLE SQL commands are defined, each creating a specific table in the database, such as customer_type, travel_type, class, eservices_rating, and more. These tables are designed to organize different aspects of the survey data, such as customer demographics, travel information, and various service ratings. Each table is structured with appropriate data types and constraints (like primary keys and foreign keys) to maintain data integrity and relational connections between tables.

Data Transformation and Insertion into Tables

Renaming Columns for Consistency

The script begins by renaming columns in the main DataFrame (df) to align with the naming conventions of the database tables. This step is crucial for maintaining consistency and ensuring that the data seamlessly fits into the structure of the database. For example, column names like 'Gender', 'Customer Type', 'Type of Travel', etc., are renamed to lowercase and underscore-separated names like 'gender', 'customer_type', 'type_of_travel', and so on.

Segmenting Data into Specific DataFrames

The data in df is then segmented into various smaller DataFrames, each representing a distinct aspect of the data and correlating to a specific table in the database. For instance, df_customertype might be created to hold unique customer types, df_traveltype for different types of travel, and df_class for class types. This segmentation is typically done by selecting relevant columns from df and removing duplicates, so each DataFrame contains unique records.

Generating Unique Identifiers

For each of these segmented DataFrames, unique identifiers (IDs) are generated. These IDs are crucial for establishing relationships between tables in the database. For example, in the df_customertype DataFrame, a unique ID is assigned to each customer type. Similarly, unique IDs are generated for travel types, class types, and other segmented DataFrames.

Inserting Data into Database Tables

Once the segmented DataFrames are prepared, they are inserted into their corresponding tables in the database using the .to_sql method. This method takes the DataFrame, the name of the target table, and the database connection as arguments and inserts the DataFrame into the table. For example, df_customertype.to_sql('customer_type', con=engine, if_exists='append', index=False) inserts the customer type data into the customer_type table in the database.

Creating Foreign Key Relationships

After the segmented DataFrames are inserted into the database, the script updates the main DataFrame (df) with the foreign key relationships. For instance, it replaces the 'customer_type' column in df with the 'customer_type_id' column from df_customertype. This step links the main survey data with the newly created unique identifiers in the database, establishing a relational structure. The process is repeated for other aspects like travel type, class type, etc.

