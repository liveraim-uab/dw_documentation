# `config` Package Documentation

The `config` module contains the parameters and variables required for the proper functioning of the code. These can be modified to adapt the execution to different environments, offer flexibility, and add future functionalities. This allows you to change the behavior of the program without needing to access and modify the source code. The module contains the following files:

* [`main_config.py`](#main_config-module): Main variables and parameters
* [`cohort_config.py`](#cohort_config-module): Cohort Class related parameters. 
* [`connection_config.py`](#connection_config-module): connection parameters.
* [`new_var_config.py`](#new_var_config-module): Definition of new or derived varibles.
* [`new_levels_config.py`](#new_levels_config-module): Definition of the levels of the new/derived categorical variables. 
* [`reference_names.py`](#reference_names-module): Names of the columns of the innitial data files (`var_data`, `level_data` and `panel_metadata`)
* [`biomarkers_config.py`](#biomarkers_config-module): Parameters needed for the proper reading, formatting and computation of biomarkers

The following sections describe the variables contained in each of these files, their purpose, and possible modifications.

## `main_config` module

#### General paremeters
* `EXECUTION_DATETIME (str)`: DateTime of the execution. Used for debugging, logging and exportation purposes. 
* `NUMERIC_TYPES (list)`: List of numeric python types compatibles with the databse. 
* `CONFIG_OBJ_NAMES (list)`: List with the names of the configuration data objects in the code (such as `var_data`, `level_data`, etc.)
* `BUILD_DUMMIES` `(bool)`: Indicates wheter a dummy dataset of the execution must be build and exported to `.feather` files. 

#### Quality control paramenters
* `ALPHA (str)`: Error threshold used during the numeric QC checks.
* `EXPORT_QC_RECORD (bool)`: With a value of `True`, it exports the data generated during the quality control process. For more details, see the section [Quality Control Utils](qc_checks_utils_doc.md). With a value of `False`, quality control is still performed, but the generated data is not saved after execution (except for what is printed in the log).
* `NAN_RECORD_EXPORT_FOLDER`: ...
* `QC_REPORT_FOLDER (str)`: name of the folder taht centralizes the QC: it will contain the QC output and the code for processing this data and the rendered report.
* `QC_DATA_FOLDER (str)`: name of the folder where all the data generated during the quality control checks will be dumped (if `EXPORT_QC_RECORD` is set to true). This folder is itended to be a subdirectory fo the directory above `QC_REPORT_FOLDER`

#### Variables used to read data files (databases, var_data, level_data, panel_metadata)

The following variables are mainly used in the module [`file_reading_utils`](../modules_documentation/file_reading_utils_doc.md). 

* `PANEL_DATA_FILE_NAME (str)`: Name of the file containing the panel data (should be an .xlsx file).
* `CONFIG_DATA_METADATA_FILE_PATH (str)`: Path of the file containing the metadata of the config data tables (i.e. data types of each column) that will be exported to the database. ****
* `LIVERSCREEN_VAR_FILE_NAME (str)`: Name of the file containing the `var_data` for the *Liverscreen* cohort (should be an .xlsx file).
* `GLUCOFIB_VAR_FILE_NAME (str)`: Name of the file containing the `var_data` for the *Glucofib* cohort (should be an .xlsx file).
* `ALCOFIB_VAR_FILE_NAME (str)`: Name of the file containing the `var_data` for the *Alcofib* cohort (should be an .xlsx file).
* `DECIDE_VAR_FILE_NAME (str)`: Name of the file containing the `var_data` for the *Decide* cohort (should be an .xlsx file).
* `MARINA_VAR_FILE_NAME (str)`: Name of the file containing the `var_data` for the *Marina* cohort (should be an .xlsx file).
* `GALAALD_VAR_FILE_NAME (str)`: Name of the file containing the `var_data` for the *Galaald* cohort (should be an .xlsx file).

* `VAR_FILE_NAMES (dict)`: Dictionary containing the cohort names (as defined in the `cohort_config` module) as keys, and the corresponding `var_data` file names as values (i.e., the names defined above). This dictionary **should not be modified** unless a new cohort is added: the dictionary is automatically generated using the other variables, ensuring it stays up to date.

* `LIVERSCREEN_LEVEL_FILE_NAME (str)`: Name of the file containing the `level_data` for the *Liverscreen* cohort (should be an .xlsx file).
* `GLUCOFIB_LEVEL_FILE_NAME (str)`: Name of the file containing the `level_data` for the *Glucofib* cohort (should be an .xlsx file).
* `ALCOFIB_LEVEL_FILE_NAME (str)`: Name of the file containing the `level_data` for the *Alcofib* cohort (should be an .xlsx file).
* `DECIDE_LEVEL_FILE_NAME (str)`: Name of the file containing the `level_data` for the *Decide* cohort (should be an .xlsx file).
* `MARINA_LEVEL_FILE_NAME (str)`: Name of the file containing the `level_data` for the *Marina* cohort (should be an .xlsx file).
* `GALAALD_LEVEL_FILE_NAME (str)`: Name of the file containing the `level_data` for the *Galaald* cohort (should be an .xlsx file).

* `LEVEL_FILE_NAMES (dict)`: Dictionary containing the cohort names (as defined in the `cohort_config` module) as keys, and the corresponding `level_data` file names as values (i.e., the names defined above). This dictionary **should not be modified** unless a new cohort is added: the dictionary is automatically generated using the other variables, ensuring it stays up to date.

- `LIVERSCREEN_COMB_VARS_FILE_NAME (str)`: Name of the file containing the `var_comb_data` for the *Liverscreen* cohort (should be an .json file).
- `GLUCOFIB_COMB_VARS_FILE_NAME (str)`: Name of the file containing the `var_comb_data` for the *Glucofib* cohort (should be an .json file).
- `ALCOFIB_COMB_VARS_FILE_NAME (str)`: Name of the file containing the `var_comb_data` for the *Alcofib* cohort (should be an .json file).
- `MARINA1_COMB_VARS_FILE_NAME (str)`: Name of the file containing the `var_comb_data` for the *Marina* cohort (should be an .json file).
- `DECIDE_COMB_VARS_FILE_NAME (str)`: Name of the file containing the `var_comb_data` for the *Decide* cohort (should be an .json file).
- `GALAALD_COMB_VARS_FILE_NAME (str)`: Name of the file containing the `var_comb_data` for the *Galaald* cohort (should be an .json file).

- `COMB_VARS_FILE_NAMES (dict)`: Dictionary containing the cohort names (as defined in the `cohort_config` module) as keys, and the corresponding `comb_var_data` file names as values (i.e., the names defined above). This dictionary **should not be modified** unless a new cohort is added: the dictionary is automatically generated using the other variables, ensuring it stays up to date.

If you wish to use other `var_data`, `level_data`, or `panel_metadata` files (for testing, verification, etc.), you can modify the value of these variables to read those files.

* `COMMON_DATA_FOLDER (str)`: Name of the folder containing all the data. This folder is not tracked by git and must follow the structure described in [Structure of data/cohort_name/ Directory](../quick_start_guide.md#structure-of-datacohort_namedatabases-directory).

* `DATABASES_FOLDER (str)`: Name of the folder, inside each cohort specific directory, that will contain the raw data recieved from the partners.

* `READING_METHODS_DICT (dict)`: Dictionary containing strings as keys that indicate different types of files and their assiciated reading method as the value. 

* `DATABASES_FORMAT (dict)`: Dictionary containig the name of each cohort as key and the type of file of their raw databases file. It is used with the `READING_METHODS_DICT` to read the cohorts databases properly. 

#### Variables used to export data

* `EXPORT_FILES (bool)`: With a value of `True`, it exports the final data (the data warehouse, with the panel structure described in [???]()) to .csv and .feather files. With a value of `False`, these files are not generated. For more details, see the section [File Exporting Utils Module](../modules_documentation/file_exporting_utils_doc.md).
* `CREATE_SQL_DB (bool)`: Con un valor de `True` se exporta los datos a una base de datos MySQL (i.e. se crea el datawarehouse). Con un valor de `False` no se exportan los datos a MySQL.
* `DATAWAREHOUSE_PATH (str)`: Name of the folder where the data warehouse in .csv and .feather will be exported.

- **`ANALYSIS_REPORT_SCRIPT_PATH (str)`**: absolute path to the `.Rmd` script used to generate the initial descriptive analysis of the data, in case the results are exported to files.

- **`COLS_IN_PANELS (dict)`**: Dictionary mapping each panel name to the list of column names that should be exported for that panel in .feather/.csv files. Used to customize the structure of the data warehouse.


- `COLS_IN_SQL_PANELS (dict)`: Dictionary mapping each panel name to the list of column names that should be exported for that panel in the SQL DB. Used to customize the structure of the data warehouse.

> **Note**: This module is usually imported using the abreviation `config`:
> 
    from config import main_config as config


## `cohort_config` module

This module contians variables related to the configuration and instantiation of each Cohort. 

* `ALCOFIB_NAME (str)`: Name to be used to refer to the *Alcofib* cohort during the execution of the main code. 
* `GLUCOFIB_NAME (str)`: Name to be used to refer to the *Glucofib* cohort during the execution of the main code.
* `LIVERSCREEN_NAME (str)`: Name to be used to refer to the *Liverscreen* cohort during the execution of the main code.
* `DECIDE_NAME (str)`: Name to be used to refer to the *Decide* cohort during the execution of the main code.
* `MARINA1_NAME (str)`: Name to be used to refer to the *Marina* cohort during the execution of the main code.
* `LIVERAIM_NAME (str)`: Name to be used to refer to the *Liveraim* merged database during the execution of the main code.
* `GALAALD_NAME (str)`: Name to be used to refer to the *Galaald* merged database during the execution of the main code.

* `ALL_COHORTS (list)`: List of all the cohort names. This variable is created using the variables defined above and should be only modified to add new elements (i.e. new cohorts). 
* `WP1_COHORT_NAME_LIST (list)`: List of cohort names that will be used for the processing of WP1. This variable is created using the variables defined above.
* `WP2_COHORT_NAME_LIST (list)`: List of cohort names that will be used for the processing of WP2. This variable is created using the variables defined above.

For each cohort, the following variables are also defined (for brevity, the term `<COHORT_NAME>` will be used to refer to the name of the specific cohort):

* `COHOR<T_NAME>_INCL_DATE_VAR (str)`: Original name of the 'inclusion date' variable.
* `<COHORT_NAME>_AGE_VAR (str)`: Original name of the 'age' variable.
* `<COHORT_NAME>_ID_VAR (str)`: Original name of the 'identifier' variable.
* `<COHORT_NAME>_COHORT_STATUS (str)`: Value of the categorical `status` variable. For more details about this variable, refer to the section on the [`new_levels_config module`](#new_levels_config-module).
 
* `COHORTS_DATA (dict)`: A dictionary that contains the previously defined variables for each cohort. This dictionary is imported (instead of importing each variable individually) into the code, making it easier to access these variables. The dictionary keys are the names of each cohort, and the values are the corresponding variables for that cohort as described above.

- `PERCENT_VARS (list)`: List of variable names (using their common mapped name) that should be represented with percentage (%) units. This list is used during formatting and processing to handle inconsistencies in how these variables are expressed.


> **Note**: This module is usually imported using the abreviation `cc`:
> 
    from config import cohort_config as cc


## `connection_config` module

This module defines:

+ Database [connection parameters](#connection-parameters) for MySQL.
+ [Dictionaries for mapping variable types](#mapping-types-dictionaries) across platforms/languages.

#### Connection Parameters

It's important to understand how the connection parameters for the MySQL database are managed. To connect to the database (as explained in the [MySQL connection configuration](../quick_start_guide.md#mysql-connection-configuration) section), five parameters are required:

+ **USER**: The username used to connect to the database (must have appropriate permissions).
+ **PASSWORD**: The user's password to access the database.
+ **DATABASE_NAME**: The name of the database to be accessed (the user must have permissions to access the database).
+ **PORT**: The connection port.
+ **HOST**: The hostname. If accessing a local database, this is usually *localhost*, while for remote access, it is typically the server name or IP address.

Each of these parameters ***must be created as environment variables***. These can be virtual or global environment variables, but they must be accessible through Python's `os` package. The names of these environment variables (which correspond to each of the connection parameters mentioned above) can be chosen at will.

Once these variables are created, you need to modify the `connection_config.py` file, where the variables `USER (str)`, `PASSWORD (str)`, `DATABASE_NAME (str)`, `PORT (str)`, `HOST (str)` are listed. These variables should be assigned the *name of the environment variable you want to use for that parameter*. For example, suppose you want to connect to a database named `example_db`. First, you would create the following environment variables (ensuring that their values are correct to establish the connection properly):

    USER=me_myself
    DB_PASSWORD=super_secret_password
    DATABASE_NAME=example_db
    PORT=22
    HOST=localhost

Next, access the `connection_config.py` file in the `config` module and assign the environment variable names to the file variables:

    USER="USER"
    PASSWORD="DB_PASSWORD"
    DATABASE_NAME="DATABASE_NAME"
    PORT="PORT"
    HOST="HOST"

If the values of the environment variables are correct, the connection should work. See the section [Testing the connection](../modules_documentation/sql_exporting_utils_doc.md#testing-the-connection) for more details to check your connection.

Although this configuration may seem unnecessarily complicated, it has been implemented this way for two reasons:

1. **Isolated sensitive data**: This configuration allows you to isolate any private or sensitive data, such as usernames or passwords. This way, this information is not recorded or written anywhere in the code, but is accessed through independently configured environment variables.

2. It allows **multiple environment variables** to be configured simultaneously. For example, if we want to connect to a remote database, we can simply add new environment variables (without modifying the previous ones) that reflect this second connection:

        REMOTE_USER=me_myself
        REMOTE_DB_PASSWORD=an_other_super_secret_password
        REMOTE_DATABASE_NAME=example_db
        REMOTE_PORT=8085
        REMOTE_HOST=remote_host

    Then, we only need to modify the `connection_config.py` file to update the connection parameters as follows:

        USER="REMOTE_USER"
        PASSWORD="REMOTE_DB_PASSWORD"
        DATABASE_NAME="REMOTE_DATABASE_NAME"
        PORT="REMOTE_PORT"
        HOST="REMOTE_HOST"

    This way, we reference other environment variables (isolated from the code) that allow the connection to another database.


#### Mapping types dictionaries

Two main dictionaries are declared in this module:

+ `sql_type_mapping (dict)`: Used to map from python datatypes to SLQ datatyes. This dictionary is used while creating the tables for the SQL database, and indicate what SQL datatype corresponds with each python datatype. If new datatypes are added to the database, make sure to update the new information here. Also, the values (SQL datatypes) can be modified to fit better the data that is going to be stored (e.g. the datatype 'category' is currently mapped to 'String(255)', but it can be changed to `Strings(31)` instead, to reduce memory consumption).
+ `py_type_mapping (dict)`: Used to map from datatpyes as string to the dataype class itself (e.g. from `'int64'` to `int`)

> **Note**: This module is usually imported using the abbreviation `con_config`
> 
    from config import connection_config as con_config

## `new_var_config` module

During data preprocessing (see the [`data_processing_utils` module](../modules_documentation/data_processing_utils_doc.md) section), some variables are added that do not appear in the initial `row_data` of the cohorts. These can be calculated or secondary variables, cohort indicators, etc. This module stores the data for these variables necessary to include them in the `var_data` object (refer to the [`cohort_utils` module](../modules_documentation/cohort_utils_doc.md) for more information), which is essential for the creation of the database.

For each new variable, the following parameters corresponding to the columns of the `var_data` object need to be added. The column names (defined in the [`reference_names` module](#reference_names-module)) are used for this purpose. This module is usually imported in the code with the alias `rf`. The parameters to be added are as follows:

+ `rf.VAR_ORIGINAL_NAME_COLUMN`: The name of the variable in the original cohort (presumably the same as the final name of the variable since it is a new variable added 'after the fact').
+ `rf.VAR_LIVERAIM_NAME_COLUMN`: The final name of the variable in the DB (presumably the same as the original name of the variable since it is a new variable added 'after the fact').
+ `rf.VAR_PANEL_COLUMN`: The panel where the variable will appear <span style="color:red">(obsolete feature, this column should be removed from `var_data` and the code)</span>
+ `rf.VAR_DATA_TYPE_COLUMN`: The data type of the added variable. Ensure that it is a data type compatible with the program.
+ `rf.ORIGINAL_UNITS_COLUMN`: The units of the variable in the original cohort (presumably the same as the final units since it is a new variable added 'after the fact').
+ `rf.FINAL_UNITS_COLUMN`: The units of the variable in the final database (presumably the same as the original units since it is a new variable added 'after the fact').
+ `rf.VAR_DESCRIPTION_COLUMN`: A brief description of the new variable.
+ `rf.VAR_CONVERSION_FACTOR_COLUMN`: Conversion factor to change from the original units to the final units. Since it is a 'new' variable, the conversion factor should be 1.

> **Note 1**: Some of these parameters may not be necessary (or simply do not apply to the variable being added). In that case, you can omit the declaration of that parameter/column, and it will appear in the `var_data` object as `NaN`.

To facilitate use and simplify the code, these new variable parameters are not listed directly in the `new_var_config.py` file. Instead, they should be added as a dictionary following the format exemplified below:

    VAR_NAME_DATA_ROW = {
        rf.VAR_ORIGINAL_NAME_COLUMN: ['var_name'],
        rf.VAR_LIVERAIM_NAME_COLUMN: ['var_name'],
        rf.VAR_PANEL_COLUMN: ['blood_test'],
        rf.VAR_DATA_TYPE_COLUMN: ['double64[ns]'],
        rf.ORIGINAL_UNITS_COLUMN: ['mg/dl'],
        rf.FINAL_UNITS_COLUMN:  ['mg/dl'],
        rf.VAR_DESCRIPTION_COLUMN: ["A brief description of the new variable"],
        rf.VAR_CONVERSION_FACTOR_COLUMN: [1]
    }

> **Note 2**: Note that the values used to initialize the dictionary are declared as lists (i.e., enclosed in square brackets `[]`). It is important to maintain this format for the code to function correctly (this is due to how `pandas` internally handles the transformation of dictionaries to DataFrames).

> **Note 3**: This module is usually imported using the abbreviation `new_vars`:
>
    from config import new_var_config as new_vars


## `new_levels_config` module

This module has a structure and function similar to the `new_var_config` module. In this case, only new categorical variables are included, and the file contains the data needed to update the `level_data` object.

> **Note**: To illustrate the addition of a categorical variable, we will use the `status` variable (already included in the module). This variable is of type string and has three levels: finished (coded as 0), ongoing (coded as 1), and withdrawn (coded as 2).

The following describes what elements should be added to the `new_levels_config.py` file to include a new categorical variable:

#### Level Codification

Since the different levels of a variable are coded as integers (generally starting from 0), each level should be listed as a variable. These variables should have their corresponding code as the value. The naming convention followed is `variable-name_level-name = code`. For example, if we are adding the `status` variable (which has three levels), we should include:

    STATUS_FINISHED = '0'
    STATUS_ONGOING = '1'
    STATUS_WITHDRAWN = '2'

#### Level Mapping

Next, a dictionary must be created to map the levels of this variable, following the naming convention `VARIABLE-NAME_LEVELS`. In this case:

    STATUS_LEVELS = {STATUS_FINISHED: 'finished',
                    STATUS_ONGOING: 'ongoing',
                    STATUS_WITHDRAWN: 'withdrawn'}

#### Creation of New `level_data` Rows

Finally, a list of dictionaries is created (each dictionary corresponds to a row in `level_data`, where the key is the column name and the value is the value of that row for that column). This list will be imported wherever these metadata need to be included in the `level_data` object. (For more information on the structure of this object, you can refer to the [Initial data and configuration data](../liveraim_data_warehouse_specifications.md#initial-data-and-configuration-data) section). In summary, for each level, a row must be included in the `level_data` object. Each row should contain the corresponding information for each of the columns in that object (to do this, as in the `var_data` module, the `reference_names` module is used to reference the column names), specifically:

+ `rf.LEVEL_ORIGINAL_NAME_COLUMN`: Column with the original cohort name of the variable to which the level belongs.
+ `rf.LEVEL_LIVERAIM_NAME_COLUMN`: Column with the final name of the variable to which the level belongs.
+ `rf.LEVEL_ORIGINAL_VALUE_COLUMN`: Value of the level in the original cohort (presumably the same as the final value since it is a variable added 'after the fact').
+ `rf.LEVEL_LIVERAIM_VALUE_COLUMN`: Value of the level in the final database (presumably the same as the original value since it is a variable added 'after the fact').
+ `rf.LEVEL_DESCRIPTION_COLUMN`: Description of the meaning of that level.
+ `rf.LEVEL_ORIGINAL_DATA_TYPE_COLUMN`: Original data type of the variable.

To create the list of dictionaries, the variable `VARIABLE-NAME_LEVEL_DATA_ROW` is created, which is initialized using list comprehension. Continuing with the example of the `status` variable and maintaining the naming convention, we would need to add these lines of code:

    STATUS_LEVEL_DATA_ROW = [{
        rf.LEVEL_ORIGINAL_NAME_COLUMN: 'status',
        rf.LEVEL_LIVERAIM_NAME_COLUMN: 'status',
        rf.LEVEL_ORIGINAL_VALUE_COLUMN: key,
        rf.LEVEL_LIVERAIM_VALUE_COLUMN: key,
        rf.LEVEL_DESCRIPTION_COLUMN: value,
        rf.LEVEL_ORIGINAL_DATA_TYPE_COLUMN: 'str'
    } for key, value in STATUS_LEVELS.items()]

> **Important Note**: This code can be used as a template to add more levels/categorical variables. It is important to note that this list comprehension relies on the parameters previously defined in the `STATUS_LEVELS` dictionary, so if the steps described are followed, only the values of the keys `original name`, `liveraim name`, `description`, and `data type` need to be modified.

> **Note**: This module is usually imported using the abbreviation `new_levels`:
>
    from config import new_levels_config as new_levels


## `reference_names` module

> **Note**: It is recommended to read the section [Initial data and configuration](../liveraim_data_warehouse_specifications.md#initial-data-and-configuration-data) before continuing with this section.

This module lists the variables that reference the column names of the three initial metadata files, namely:

+ `level_data`
+ `var_data`
+ `panel_metadata`

These names are widely used throughout the code. The variables follow a specific naming convention: the first part of the variable name refers to the file/object it belongs to, and the last part is `COLUMN`, indicating that the variable contains the name of a column from a `.xlsx` file (or a DataFrame). This helps differentiate them from similar variables.

For instance, the variable that stores the name of the column in the `var_data` file containing the description of each variable is named `VAR_DESCRIPTION_COLUMN`.

At the risk of being redundant, the variables defined in this module are listed below:

#### `var_data` column variables

Variables that reference the columns of the `var_data` file/object (note that each row corresponds to a variable):


+ `VAR_ORIGINAL_NAME_COLUMN`: Name of the column where the variable's original name in the cohort is defined.
+ `VAR_LIVERAIM_NAME_COLUMN`: Name of the column where the final name (in the common format) of the variable is defined.
+ `VAR_PANEL_COLUMN`: Name of the column where the panel to which the variable belongs is defined.
+ `VAR_DATA_TYPE_COLUMN`: Name of the column where the data type of the variable is defined.
+ `VAR_ORIGINAL_UNITS_COLUMN`: Name of the column where the original units of the variable in the cohort are defined.
+ `VAR_DESCRIPTION_COLUMN`: Name of the column where the variable is described.
+ `VAR_FINAL_UNITS_COLUMN`: Name of the column where the final units of the variable in the final database are defined.
+ `VAR_LOWER_BOUND_COLUMN`: Name of the column where the acceptable lower bound for the variable's values is defined.
+ `VAR_UPPER_BOUND_COLUMN`: Name of the column where the acceptable upper bound for the variable's values is defined.
+ `VAR_CONVERSION_FACTOR_COLUMN`: Name of the column where the conversion factor used to transform values from the original cohort to the final units is defined.

#### `level_data` column variables

Variables that reference the columns of the `level_data` file/object:

+ `LEVEL_ORIGINAL_NAME_COLUMN`: Name of the column where the original name of the variable in the cohort is defined.
+ `LEVEL_LIVERAIM_NAME_COLUMN`: Name of the column where the final name (in the common format) of the variable is defined.
+ `LEVEL_ORIGINAL_VALUE_COLUMN`: Name of the column where the level value in the original cohort is defined.
+ `LEVEL_LIVERAIM_VALUE_COLUMN`: Name of the column where the level value in the final database is defined.
+ `LEVEL_DESCRIPTION_COLUMN`: Name of the column where the variable is described.
+ `LEVEL_ORIGINAL_DATA_TYPE_COLUMN`: Name of the column where the data type of the variable in the original cohort is defined.

#### `panel_metadata` column variables

Variables that reference the columns of the `panel_metadata` file/object. This file is composed of different tabs, each describing a panel. Each row in each tab corresponds to a variable that should appear in the panel (whether in long or wide format):

+ `PANEL_VAR_NAME_COLUMN`: Name of the column containing the variable names (in the common format) that will appear in the panel.
+ `PANEL_MELT_COLUMN`: Name of the column that indicates whether the variable should appear in long or wide format.

#### `final_data` column variables

The following variables specify the names of some of the columns in the tables with the final data (the final DB). In particular, they describe the names of the columns in the 'long' format tables that will contain the variable name and its corresponding value.

+ `FINAL_DATA_VARIABLE_NAME`: Name of the column in the final long-format tables where the variable names will appear.
+ `FINAL_DATA_VALUE_NAME`: Name of the column in the final long-format tables where the variable values will appear.

This module should only be changed if the column names in `level_data`, `var_data` and `panel_metadata` are changed.

## `ids_to_drop.txt` File

Inside the configuration module directory, a file named `ids_to_drop.txt` can be placed (this file is not tracked by Git). It should contain a raw list of patient identifiers, one per line. Each patient listed in this file will be excluded from the data warehouse during data processing.

This provides a simple way to remove specific patients from the data warehouse—e.g., due to incorrect identification, misspelled IDs, duplicates, or ID collisions—by adding them to this list.

## `biomarkers_config` module

In this module contains the following configuration classes, that contain all the specifications for the proper reading and formatting of biomarker data received from the providers:

- **[`BMKConfig`](#BMKConfig-class)**: Works as parent class for the rest of the provierd-specific configuration classes. In consequence, it contains all the common attributes that this classes share.
- **[`HClinicBMKConfig`](#HClinicBMKConfig-class)**: Contains all the specific configuration parameters for the data received from the Hospital Clínic
- **[`NordicBMKConfig`](#NordicBMKConfig-class)**: Contains all the specific configuration parameters for the data received from the Nordic
- **[`RocheBMKConfig`](#RocheBMKConfig-class)**: Contains all the specific configuration parameters for the data received from the Roche

#### `BMKConfig` class

##### **Attributes**:

- `provider`: Name of the provider (hclinic, nordic or roche)
- `provider_data_dir`: Directory where the biomarkers raw data for this provider is stored.
- `BMK_DATA_DIR_PATH`: Common directory containing all biomarkers data (in particular, it contains the subpath `provider_data_dir`)
- `FINAL_ID_COLUN`: Name of the ID column in the final formatted DataFrame.
- `FINAL_VARIABLE_COLUMN`: Name of the variable column (which contains the blinded name of the biomarker) in the final formatted DataFrame.
- `FINAL_VALUE_COLUMN`: Name of the ID column in the final formatted DataFrame (in the current version this column is not exported to the final data and is replaced by the FINAL_NUMERIC_VALUE_COLUMN).
- `FINAL_NORM_VALUE_COLUMN`: Name of the normalized value column in the final formatted DataFrame.
- `FINAL_REQUEST_COLUMN`: Name of the request ID column in the final formatted DataFrame (in the current version this column is not exported to the final data).
- `FINAL_REGISTER_DATE_COLUMN`: Name of the register date column in the final formatted DataFrame (in the current version this column is not exported to the final data).
- `FINAL_VALIDATION_DATE_COLUMN`: Name of the validation date column in the final formatted DataFrame.
- `FINAL_NUMERIC_VALUE_COLUMN`: Name of the final numeric value (which only contains numeric values) column in the final formatted DataFrame.
- `FINAL_LIVERAIM_ID_COLUMN`: Name of the common liveraim ID column in the final formatted DataFrame.
- `FINAL_LIMIT_DETECT_COLUMN`: Name of the limit detection column in the final formatted DataFrame.
- `FINAL_COMMENTS_COLUMN`: Name of the comments column in the final formatted DataFrame.
- `FINAL_Z_SCORE_COLUMN`: Name of the z_score column in the final formatted DataFrame.
- `FINAL_QUINTILES_COLUMN`: Name of the quintiles column in the final formatted DataFrame.

- `BMK_FINAL_COLUMNS`: List of the column names selected for the final biomarker panel.
- `python_column_types`: Dictionary with the python datatypes of each of the final columns.
- `mysql_column_types`: Dictionary with the SQL datatypes of each of the final columns.
- `BLINDED_BIOMARKER_MAP`: Dictionary to map the biomarkers names to blinded names (currently uppercase letters).
- `BMKS_CUT_OFFS`: Dictionary with the cutoff values of the biomarkers (if provided).


#### `HClinicBMKConfig` class

Inherits from `BMKConfig`.

##### **New attributes**:

- `provider`: name of the provider (nordic)
- `ID_COLUMN`: name of the original/raw column of the IDs.
- `VARIABLE_COLUMN`: name of the original/raw column of the biomarker name (variable column)
- `VALUE_COLUMN`: name of the original/raw column of the biomarker measurement value.
- `REQUEST_COLUMN`: name of the original/raw request code column.
- `REGISTER_DATE_COLUMN`: name of the original/raw register date colu,m.
- `VALIDATION_DATE_COLUMN`: name of the original/raw validation_date column.
- `NUMERIC_VALUE_COLUMN`: name of the original/raw numeric value column  (this column is not present in the raw data). In this case, it is set to the same as VALUE_COLUMN.
- `LIVERAIM_ID_COLUMN`: name of the liveraim ID column (this column is not present in the raw data). Its value should be the same as FINAL_LIVERAIM_ID_COLUMN.
- `LIMIT_DETECT_COLUMN`: name of limit detection column (this column is not present in the raw data). Its value should be the same as FINAL_LIMIT_DETECT_COLUMN.
- `ORIGINAL_COLUMNS`:  list with all the column names in the original raw data.
- `biomarker_columns_map`: dictionary mapping the original names to the final names of the columns.
-  `FILE_TYPE`: type of the raw data files received from the provider.
- `READING_METHOD`: method that will be used to read the raw data.
- `COMMENTS_DICT`: Dictionary used to map and harmonize the comments from strings to a numeric code.
- `MISSING_SYMBOLS`: List of symbols that, in VALUE_COLUMN, are equivalent to a missing (i.e. "no calculable", "ND", ...).

#### `NordicBMKConfig` class
Inherits from `BMKConfig`.


##### **New attributes**:

- `provider`: name of the provider (nordic)
- `ID_COLUMN`: name of the original/raw column of the IDs.
- `VARIABLE_COLUMN`: name of the original/raw column of the biomarker name (variable column)
- `VALUE_COLUMN`: name of the original/raw column of the biomarker measurement value.
- `REGISTER_DATE_COLUMN`: name of the original/raw register date colu,m.
- `VALIDATION_DATE_COLUMN`: name of the original/raw validation_date column.
- `NUMERIC_VALUE_COLUMN`: name of the original/raw numeric value column  (this column is not present in the raw data). In this case, it is set to the same as VALUE_COLUMN.
- `LIVERAIM_ID_COLUMN`: name of the liveraim ID column (this column is not present in the raw data). Its value should be the same as FINAL_LIVERAIM_ID_COLUMN.
- `LIMIT_DETECT_COLUMN`: name of limit detection column (this column is not present in the raw data). Its value should be the same as FINAL_LIMIT_DETECT_COLUMN.
- `COMMENTS_COLUMN`: name of the original/raw comments column.
- `STUDY_ID_COLUMN`: name of the original/raw study column.
- `UNITS_COLUMN`: name of the original/raw units column.
- `METHOD_COLUMN`: name of the original/raw method column.

- `IDS_REASSIGNMENT_PATH`: path to the .json dictionary that contains the mapping of the unmatching IDs from Odense patients.

- `ORIGINAL_COLUMNS`: list with all the column names in the original raw data.
- `biomarker_columns_map`: dictionary mapping the original names to the final names of the columns.
- `DATA_DIR`: Name of the directory inside `BMK_DATA_DIR_PATH` where the provider's data is stored.
- `FILE_TYPE`: type of the raw data files received from the provider.
- `READING_METHOD`: method that will be used to read the raw data.
- `COMMENTS_DICT`: Dictionary used to map and harmonize the comments from strings to a numeric code.
- `MISSING_SYMBOLS`: List of symbols that, in VALUE_COLUMN, are equivalent to a missing (i.e. "no calculable", "ND", ...).
- `limit_detect_bounds`: Dictionary with the detection limit bounds (upper and lower bounds for each biomarker) if provided by the provider.
- `l_bound_comment`: Comment that indicates that the the value in the register us below the limit of quantification.
- `u_bound_comment`: Comment that indicates that the the value in the register us above the limit of quantification.

#### `RocheBMKConfig` class
Inherits from `BMKConfig`.

- `provider`: name of the provider (nordic)
- `ID_COLUMN`: name of the original/raw column of the IDs.
- `VARIABLE_COLUMN`: name of the original/raw column of the biomarker name (variable column)
- `VALUE_COLUMN`: name of the original/raw column of the biomarker measurement value.
- `REGISTER_DATE_COLUMN`: name of the original/raw register date colu,m.
- `VALIDATION_DATE_COLUMN`: name of the original/raw validation_date column.
- `NUMERIC_VALUE_COLUMN`: name of the original/raw numeric value column  (this column is not present in the raw data). In this case, it is set to the same as VALUE_COLUMN.
- `LIVERAIM_ID_COLUMN`: name of the liveraim ID column (this column is not present in the raw data). Its value should be the same as FINAL_LIVERAIM_ID_COLUMN.
- `LIMIT_DETECT_COLUMN`: name of limit detection column (this column is not present in the raw data). Its value should be the same as FINAL_LIMIT_DETECT_COLUMN.
- `COMMENTS_COLUMN`: name of the original/raw comments column.
- `STUDY_ID_COLUMN`: name of the original/raw study column.
- `UNITS_COLUMN`: name of the original/raw units column.
- `METHOD_COLUMN`: name of the original/raw method column.

- `IDS_REASSIGNMENT_PATH`: path to the .json dictionary that contains the mapping of the unmatching IDs from Odense patients.

- `ORIGINAL_COLUMNS`: list with all the column names in the original raw data.
- `biomarker_columns_map`: dictionary mapping the original names to the final names of the columns.
- `DATA_DIR`: Name of the directory inside `BMK_DATA_DIR_PATH` where the provider's data is stored.
- `FILE_TYPE`: type of the raw data files received from the provider.
- `READING_METHOD`: method that will be used to read the raw data.
- `COMMENTS_DICT`: Dictionary used to map and harmonize the comments from strings to a numeric code.
- `MISSING_SYMBOLS`: List of symbols that, in VALUE_COLUMN, are equivalent to a missing (i.e. "no calculable", "ND", ...).
- `limit_detect_bounds`: Dictionary with the detection limit bounds (upper and lower bounds for each biomarker) if provided by the provider.
- `l_bound_comment`: Comment that indicates that the the value in the register us below the limit of quantification.
- `u_bound_comment`: Comment that indicates that the the value in the register us above the limit of quantification.
