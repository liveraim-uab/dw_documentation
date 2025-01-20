# `file_reading_utils` Documentation

## Overview

The `file_reading_utils` module is responsible for reading various types of data files (databases and metadata) required for building the data warehouse. The primary class in this module is `DataReader`, which centralizes the reading process for all relevant files, including raw cohort data, variable metadata (`var_data`), level metadata (`level_data`), panel metadata (`panel_data`) and metadata to combine variables (`comb_var_data`). 

This module is sensitive to the structure of the directory where the data is stored, and it also includes the function `hash_file`, which calculates the hash of a file. This can be useful for logging purposes, as it helps trace the files used during data reading.

## General Structure

The module contains the following key components:

- **`DataReader`**: The main class used for reading raw cohort data and metadata, managing logging, and ensuring the directory structure is correct.
- **`hash_file`**: A utility function for generating the hash of a file.
- **`stringfy`**: A helper function to convert a list of dictionaries into a formatted string for logging purposes.

## Class Description

### Class **`DataReader`**

#### Description:
The `DataReader` class centralizes the process of reading various data files required for creating a data warehouse. This includes reading raw cohort data, variable and level metadata, panel data and combination variable data. It manages logging, error handling, and ensures proper file access and reading.

#### Attributes:
- **`reading_methods_dict` (dict)**: Contains the methods for reading different types of data (e.g., `.csv`, `.dta`, `.xlsx`).
- **`databases_format_dict` (dict)**: Specifies the format of the raw databases for each cohort, used in conjunction with `reading_methods_dict` to read cohort data.
- **`common_data_folder` (str)**: The directory where all the data files to be read are stored.
- **`databases_folder` (str)**: The subfolder where the raw data for each cohort is stored.
- **`level_file_names` (dict)**: A dictionary containing the filenames of the level data for each cohort.
- **`var_file_names` (dict)**: A dictionary containing the filenames of the variable data for each cohort.
- **`comb_vars_file_names` (dict)**: A dictionary containing the filenames of the combined variables data for each cohort.
- **`panel_file_name` (str)**: The filename for the panel data.
- **`cohorts` (list[str])**: A list containing the names of the cohorts to be read.
- **`reading_exceptions` (tuple)**: A tuple of common exceptions that may arise while reading files.
- **`all_data` (dict)**: A dictionary containing all the data required to create the data warehouse.
- **`read_files_log` (dict)**: A dictionary containing the filenames and their hashes for logging purposes.

#### Methods:
- **`__init__(self, cohort_names_list: list) -> None`**: 
    Initializes the `DataReader` with a list of cohort names and sets up the necessary attributes and configurations.
    - **Arguments**:
        - `cohort_names_list` (list): List of cohort names that will be read.

- **`check_cohort_names(self, cohort_names_list: list) -> list`**: 
    Checks if the provided cohort names are valid based on the available configuration. Logs an error if any name is incorrect.
    - **Arguments**:
        - `cohort_names_list` (list): List of cohort names to validate.
    - **Returns**:
        - `list`: The validated list of cohort names.

- **`read_cohort_databases(self, cohort: str, engine='csv') -> dict[str, pd.DataFrame]`**: 
    Reads all versions of the raw data for a specified cohort. The files must be stored in the same directory and have the same format.
    - **Arguments**:
        - `cohort` (str): Name of the cohort whose data will be read.
        - `engine` (str): The engine used to read the files (default is 'csv').
    - **Returns**:
        - `dict[str, pd.DataFrame]`: A dictionary with version names as keys and DataFrames as values.

- **`read_meta_data(self, cohort_name: str, meta: Literal['var', 'level']) -> pd.DataFrame`**: 
    Reads the specified metadata (either variable or level data) for a cohort and returns it as a DataFrame.
    - **Arguments**:
        - `cohort_name` (str): The name of the cohort whose metadata will be read.
        - `meta` (Literal): Specifies whether to read variable (`var`) or level (`level`) metadata.
    - **Returns**:
        - `pd.DataFrame`: The metadata DataFrame.

- **`read_panel_data(self) -> dict[str, pd.DataFrame]`**: 
    Reads the panel data, which contains additional metadata for the panels.
    - **Returns**:
        - `dict[str, pd.DataFrame]`: A dictionary with the panel data.

- **`read_combined_vars_data(self, cohort_name: str) -> dict`**: 
    Reads the combined variables data for a specified cohort and returns it as a dictionary.
    - **Arguments**:
        - `cohort_name` (str): The name of the cohort whose combined variables data will be read.
    - **Returns**:
        - `dict`: The combined variables data as a dictionary.
:
- **`read_all_cohort_data(self, cohort_name: str) -> dict`** 
    Reads all data related to a specific cohort, including metadata and raw data.
    - **Arguments**:
        - `cohort_name` (str): The name of the cohort whose data will be read.
    - **Returns**:
        - `dict`: A dictionary containing the raw data, variable data, and level data for the cohort.

- **`read_all_data(self) -> dict`**: 
    Reads all data for all cohorts, including metadata and panel data.
    - **Returns**:
        - `dict`: A dictionary containing the data for all cohorts and panel data.

- **`set_all_data(self) -> None`**: 
    Sets the `all_data` attribute by calling the `read_all_data` method.

## Utility Functions

- **`hash_file(file_name: str, algorithm: str="sha256") -> str`**: 
    Generates the hash of a specified file using the provided hashing algorithm (default is SHA-256).
    - **Arguments**:
        - `file_name` (str): The path of the file to hash.
        - `algorithm` (str): The hashing algorithm to use (default is "sha256").
    - **Returns**:
        - `str`: The hash of the file in hexadecimal format.

- **`stringfy(files: list[dict]) -> str`**: 
    Converts a list of dictionaries into a string format for logging purposes.
    - **Arguments**:
        - `files` (list[dict]): A list of simple dictionaries.
    - **Returns**:
        - `str`: The formatted string representation of the input.

- **`test_datareader() -> None`**:
    Tests the instantiation of the DataReader class and prints the relative path and names of the files that have been read. For testing purpose. If this module (`file_reading_utils`) is executed directly as the main program, this function will be executed. 

## General Operation 

Cuando se instancía la clase `DataReader` se desencadenan, secuencialmente, las siguientes acciones: 

- Instanciación de los atributos básicos importados desde los modulos de configuración (`config`) como `reading_methods_dict`, `common_data_folder`, etc. 
- Chequeo de los nombres de las cohortes para las cuales se va a leer los datos. Esto se hace llamando al método `check_cohort_names`.
- Lectura de todos los datos, que se almacenan en el atributo `all_data`. Esto se hace llamando al método `read_all_data`. Consulta el apartado [`output description`](#output-description) para más detalles. Este método llama internamente, y para cada cohorte, al método `read_cohort_data`, encargado de leer todos los datos necesarios de una cohorte. Además, lee el archivo `panel_data`. 

## Logs

The `DataReader` class logs various events during the reading process:
- **Cohort Name Validation**: Logs an error if any invalid cohort names are provided.
- **File Reading**: Logs success or failure messages for each file read (raw data, metadata, panel data). For the database files, it also logs the name of each version and its hash. 
- **Hash Calculation**: Logs errors if any issues occur while calculating file hashes.

## Usage Example

The following is an example of how to use the `DataReader` class to read cohort data and metadata:

```python
from data_reader import DataReader
import config.cohort_config as cc

# List of cohorts to read
cohorts_list = cc.COHORT_NAME_LIST

# Instantiate the DataReader class
data_reader = DataReader(cohorts_list)

# Access the all_data attribute, which contains all cohort data
all_data = data_reader.all_data

# Access data for a specific cohort
liverscreen_data = all_data['Liverscreen']['data']
var_data = all_data['Liverscreen']['var_data']
level_data = all_data['Liverscreen']['level_data']
```

In this example, the `DataReader` is instantiated with a list of cohort names, and the data for each cohort is read into the `all_data` attribute. The raw data, variable metadata, and level metadata for each cohort can be accessed from this attribute.

## Output Description

The DataReader class reads and stores the following data for each cohort:

- **Raw Data**: The raw data (multiple versions if available) for each cohort. 
- **Variable Metadata** (var_data): Metadata describing the variables in the cohort.
- **Level Metadata** (level_data): Metadata describing the levels for categorical variables.
- **Combined Variables Data**: Information required to combine specific variables.

Additionally, the **panel data** is read and stored as a dictionary. 

The structure of `all_data` is:

    all_data = {
        'Liverscreen': {
            'data': {version_name: DataFrame, ...},
            'var_data': DataFrame,
            'level_data': DataFrame,
            'comb_var_data': dict,
        },
        'Glucofib': {...
        },
        ...
        'panel_data': {
            'panel_name': DataFrame, ...
        }
    }

## Error Handling

The DataReader handles several common errors that may arise during the file reading process, including:

- `FileNotFoundError`: Raised if a specified file or directory does not exist.
- `pd.errors.EmptyDataError`: Raised if a file is empty.
- `pd.errors.ParserError`: Raised if there is a parsing error while reading a file.
- `json.JSONDecodeError`: Raised if there is an error decoding JSON files.
- `UnicodeDecodeError`: Raised if there is an encoding issue while reading a file.

These errors are logged, and empty DataFrames or dictionaries are returned in case of failure.