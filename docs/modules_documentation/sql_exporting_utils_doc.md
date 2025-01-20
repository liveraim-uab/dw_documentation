# `sql_exporting_utils` Documentation

## Overview

The `SQLExporter` module is responsible for creating the structure of a MySQL database, handling connections, and exporting processed data into the database. It relies on SQLAlchemy ORM for creating tables, establishing relationships, and managing connections to the database. The primary functionality of this module is centralized in the `SQLExporter` class.

## General Structure

The module defines the following key components:

+ **`Base`**: This is the base class used by SQLAlchemy for defining metadata and creating the ORM mappings. All table classes in this module inherit from this base class. For further information, visit the [SQLAlchemy documentation](https://docs.sqlalchemy.org/en/20/). 
+ **`SQLExporter`**: This is the main class that manages the creation of tables, connection to the database, and the export of cohort data to the MySQL database. It uses SQLAlchemy's ORM to handle the database schema and pandas to export dataframes.

## Class Description

### **`SQLExporter`**

#### Description:
The `SQLExporter` class handles the creation of MySQL tables based on the cohort data, establishes a connection to the MySQL database, and exports processed cohort data to the database. It uses the SQLAlchemy ORM to define the table schema and manage database interactions.

#### Attributes:
- **`panel_data_json` (dict)**: A dictionary containing metadata about the cohort panels (tables) and their columns.
- **`engine` (Engine)**: The SQLAlchemy engine object that manages the connection to the MySQL database.
- **`base` (type)**: The SQLAlchemy `Base` class used to define ORM mappings.
- **`metadata` (MetaData)**: SQLAlchemy's metadata object, which contains information about all the tables defined in the schema.
- **`type_mapping` (dict)**: A dictionary that maps Python data types to SQLAlchemy data types.

#### Methods:

- **`set_engine() -> Engine`**:  
    This method initializes and returns the SQLAlchemy engine for connecting to the MySQL database. The connection parameters are retrieved from environment variables using the `connection_config` module.
    - **Return**: 
        - `Engine`: The SQLAlchemy engine for the MySQL connection.

- **`create_table_class(name: str, table_json: dict, type_mapping: dict) -> type`**:  
    Dynamically creates a table class based on the provided metadata (`table_json`). The method defines columns, data types, and sets primary/foreign keys based on whether the table is in wide or long format.
    - **Arguments**:
        - `name` (str): The name of the table.
        - `table_json` (dict): Metadata about the table, including columns and data types.
        - `type_mapping` (dict): A mapping of Python data types to SQLAlchemy data types.
    - **Return**:
        - `cls`: The dynamically created class representing the table.

- **`is_long_format(table_json: dict) -> bool`**:  
    Determines if a table is in long format (melted) based on the configuration data.
    - **Arguments**:
        - `table_json` (dict): configuration data for the table.
    - **Return**:
        - `bool`: Returns `True` if the table is in long format, `False` otherwise.

- **`get_value_datatype(table_json: dict) -> str`**:  
    Retrieves the data type for the 'value' column in long-format tables by checking if there are any variables to be melted in the table_json configuration dictionary.
    - **Arguments**:
        - `table_json` (dict): Metadata for the table.
    - **Return**:
        - `str`: The data type of the 'value' column.

- **`define_final_columns(table_json: dict, long_format: bool) -> dict`**:  
    Defines the final columns and their data types for a given table based on the metadata. If the table is in long format, additional columns like 'variable' and 'value' are added.
    - **Arguments**:
        - `table_json` (dict): configuration data for the table.
        - `long_format` (bool): Whether the table is in long or wide format.
    - **Return**:
        - `dict`: A dictionary mapping column names to their data types.

- **`create_tables() -> None`**:  
    For each table specified in `panel_data_json` (except the population panel), creates its corresponding ORM class by calling `create_table_class`.

- **`create_all() -> None`**:  
    Creates all the ORM table classes specified in `panel_data_json` and calls the SQLAlchemy method `metadata.create_all` to create the tables in the MySQL database.

- **`export_data_to_sql(final_data: dict[str, pd.DataFrame], if_exists: Literal['fail', 'replace', 'append'] = 'append') -> None`**:  
    Exports the final cohort data to the MySQL database. The data is exported using pandas' `to_sql` method, with options to handle existing data (fail, replace, append).
    - **Arguments**:
        - `final_data` (dict[str, pd.DataFrame]): A dictionary where keys are panel names and values are pandas DataFrames representing the panel data.
        - `if_exists` (Literal['fail', 'replace', 'append']): Determines what happens if the table already exists in the database. Default is 'append'.
  - **`export_df_to_sql(self, df: pd.DataFrame,`
                         `table_name: str,`
                         `if_exists: Literal['fail', 'replace', 'append'] ="append") -> None:`**
    A simple method to dump a single dataframe into the database. Uses the pandas method pd.to_sql and the engine created in the class.

    - **Arguments**:  
            - `df` (pd.DataFrame): dataframe to be dumped in the MySQL database
            - `table_name` (str): name of the table where the dataframe should be dumped. 
            - `if_exists` (Literal['fail', 'replace', 'append'], optional): 
                Specifies the behavior if the table already exists in the database:
                    - '*fail*': If the table exists, an error is raised.
                    - '*replace*': The existing table is dropped, and a new one is created.
                    - '*append*': The new data is appended to the existing table.
                Default is 'append'.

- **`drop_all() -> None`**:  
    Drops all the tables from the MySQL database.

- **`verify_tables() -> None`**:  
    Verifies the existence of tables in the database and logs their names if present.

- **`test_connection() -> bool`**:  
    Tests the connection to the MySQL database by executing a simple query. Returns `True` if the connection is successful, `False` otherwise.

## Logs

- In method `set_engine`:
    - Logs an error if any enviroment variable needed for the connection couldn't be called. 
    - Logs if the engine was successfully created or there was an error while creating it. 
- In `create_table_class`:
    - Logs an error If any data type in the configuration data (`final_columns` parameter) is not compatible (i.e. is not in the type_mapping attribute).
    - Logs if the table ORM class has been succesfully created or if there has been an error while creating it (i.e. while calling metaclass `type`)
- In `get_value_datatype`: 
    - Logs an error if there are two or more different datatypes in the variables that have to be melted (transformed into long format) in a specific panel.
    - Logs an error if there are no variables to be melted. 
- In `export_data_to_sql`:
    - Logs if everte panel has been exported to MySQL properly or if, on the contrary, an error ocurred while exporting the data. 
- In `export_df_to_sql`:
    - Logs if the dataframe has been exported to MySQL properly or if, on the contrary, an error ocurred while exporting the data. 
- In `drop_all`:
    - If no tables are found in the database, it logs an info message. 
    - For every dropped table, it logs an info message. 
    - If a specific table to drop is not found in metadata, logs a message indicating that it skips this table. 
    - If an error ocurrs while dropping a table, it is logged. 
- In `verify_tables`:
    - If an error ocurrs while inspecting the database and getting the table names, it is logged. 
    - It logs the list of tables in the database (if any).
    - If no tables are found, an info message is logged indiciating so.
- In `test_connection`: 
    - If the connection test was successful, logs a message indicating. Otherwise logs an error message. 

## General operation

It is important to mention that `SQLExporter` utilizes the ORM (Object-Relational Mapping) framework provided by SQLAlchemy. This means that future SQL database tables must be treated as Python classes in the code. For example, if you wish to create a table called `population`, you must first define a Python class (which, for technical reasons, should inherit from `Base`), let's call it `Population`. This class will define the metadata for the table through its attributes.

When an instance of the `SQLExporter` class is created (which requires an object of the `panel_data` type), an `Engine` object is automatically created. The `Engine` is responsible for managing database connections as they are needed.

> **Note**: The `Engine` object follows a "lazy" connection paradigm. This means that it does not establish a connection to the database until it is actually required. Therefore, when the `Engine` is created, no immediate connection is made. One consequence of this behavior is that, although the `Engine` can be created without errors, the actual database connection may still fail. To verify the connection, it is recommended to run the `test_connection` method or execute the module as the main program.

Additionally, the `SQLExporter` class creates the attributes `base` (which uses the previously defined `Base` class) and `metadata` (an attribute of the `Base` class). For more detailed information on the internal workings of `Base` and `metadata`, refer to the [SQLAlchemy documentation](https://docs.sqlalchemy.org/en/20/). In general terms, the `Base` class acts as a "repository" where all tables and relationships (defined as classes) are stored within the `SQLExporter`. To create a table in the database, it must first be declared as a class in Python. For the class to be registered in `Base`, it must inherit from `Base`. This allows `Base` to record all tables/classes and their configurations, so that they can later be created in the SQL database when the connection is established.

When the `SQLExporter` class is instantiated, the `Population` table/class is stored within `Base`.

After instantiating the class, the `create_all` method is called. This method dynamically creates the table/class for each panel in the `panel_data_json`. It does so using the methods previously defined. For each panel defined in the `panel_data_json` dictionary:

1. It checks if the panel follows a long or wide format. If the panel is in long format, it extracts the data type of the future `values` column.
2. It runs the `define_final_columns` method, which generates a dictionary containing the metadata for each final column (taking into account the final table structure), as well as relationships, primary keys, and foreign keys.
3. It executes the `create_table_class` method, which creates a table/class using the parameters obtained from `define_final_columns`.

Once the tables are created and stored in `Base`, the `create_all` method is called. At this point, the first explicit connection to the database is made: the tables stored in `Base` (along with their respective configurations) are now created in the actual MySQL database.

Finally, the `export_data_to_sql` method (called from `main`) is executed, taking as a parameter the final data that needs to be exported to SQL. This method reconnects to the database and loads the data into the corresponding tables.


## Example Usage

### Creating and Exporting Data

```python
from sql_exporter import SQLExporter
import pandas as pd

# Example metadata
panel_data_json = {
    'population': {...},  # Population panel metadata
    'panel_1': {...},     # Panel 1 metadata
    # Other panels...
}

# Example data
final_data = {
    'population': pd.DataFrame(...),
    'panel_1': pd.DataFrame(...),
    # Other panels...
}

# Instantiate SQLExporter
sql_exporter = SQLExporter(panel_data_json)

# Create the tables in MySQL
sql_exporter.create_all()

# Export the data to MySQL
sql_exporter.export_data_to_sql(final_data)
```

### Testing the connection: 

You can test if the connection cnfiguration is correct by calling the method `test_connection` or by executiong the module as the main program. To do so, just execute the following comand in the terminal (make sure you are in the proper directory)ç

```bash
python sql_exporting_utils.py
```

## Output Description

Consulta la sección 

## Usage in the main execution

In the context of database creation, an instance of the `SQLExporter` class is created using `liveraim.panel_data_json` as a parameter. Next, the database tables are created, and finally, the final data (previously created and stored in the `Liveraim.final_data` attribute) is exported.

```python
if config.CREATE_SQL_DB:
        sql_exporter = SQLExporter(liveraim.panel_data_json)
        sql_exporter.create_all()
        sql_exporter.export_data_to_sql(liveraim.final_data)
```