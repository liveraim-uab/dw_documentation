# `file_exporting_utils` Documentation

## Overview

The `file_exporting_utils` module is responsible for exporting data produced during the execution of the main program into various file formats, such as `.csv` and `.feather`. It ensures that the exported files are saved within a structured directory system, which includes a timestamp-based execution folder and separate subfolders for each file format.

This module facilitates the following functionalities:
+ **Export to CSV**: Exports pandas DataFrames into `.csv` files.
+ **Export to Feather**: Exports pandas DataFrames into `.feather` files.
+ **Dynamic directory creation**: Creates structured directories based on the execution time to organize the exported files.

The `FileExporter` class is used to centralize all export operations, handling the creation of necessary subdirectories, file formatting, and logging potential issues during the export process. This class is initialized with a common root folder and dynamically creates subdirectories for each export type.

## General Structure

The module includes the definition of the main class resposible of managing the exportation:

+ `FileExporter`: This is the main class of the module and is responsible for handling data exports to both CSV and Feather formats. It also manages the creation of subdirectories based on the current execution time and logs any errors that occur during the export process.

When the class is instantiated, it autoatically creates the directories needed for the exportations.

## Class Description

### **`FileExporter`**

#### Description:
The `FileExporter` class is designed to export pandas DataFrames to `.csv` and `.feather` formats, ensuring the exported files are organized into separate directories for each execution. It dynamically creates directories for each type of export (CSV and Feather) based on the execution time and logs the export process.

#### Attributes:
- **`common_export_folder` (str)**: The root directory where all exported files will be saved.
- **`execution_time` (str)**: A timestamp indicating when the export was initiated, based on the current execution time.
- **`execution_export_folder` (str)**: A folder specific to the current execution, where all export files will be stored.
- **`feather_folder` (str)**: Subfolder where `.feather` files will be saved.
- **`csv_folder` (str)**: Subfolder where `.csv` files will be saved.

#### Methods:
- **`__init__(self, common_export_folder: str) -> None`**: 
    Initializes the `FileExporter` object and creates the necessary export folders.

    - **Arguments**:
        - `common_export_folder` (str): The root directory where export files will be stored.


- **`_create_folder(self, folder: str) -> None`**: 
    Creates a folder in the file system. Logs an error if the folder could not be created.
    - **Arguments**:
        - `folder` (str): The path of the folder to create.


- **`_create_csv_exports_folder(self, parent_folder: str) -> str`**: 
    Creates the subfolder for storing CSV files within the specified parent folder.

    - **Arguments**:
        - `parent_folder` (str): The directory where the CSV subfolder will be created.
    - **Returns**:
        - `str`: The path to the created CSV folder.


- **`_create_feather_exports_folder(self, parent_folder: str) -> str`**: 
    Creates the subfolder for storing Feather files within the specified parent folder.

    - **Arguments**:
        - `parent_folder` (str): The directory where the Feather subfolder will be created.
    - **Returns**:
        - `str`: The path to the created Feather folder.


- **`export_to_csv(self, df: pd.DataFrame, df_name: str = "Unspecified file") -> None`**: 
    Exports the given pandas DataFrame to a CSV file in the CSV export subfolder.

    - **Arguments**:
        - `df` (pandas.DataFrame): The DataFrame to export.
        - `df_name` (str): The name of the file (without extension). Defaults to "Unspecified file".
    - **Returns**:
        - `None`


- **`export_to_feather(self, df: pd.DataFrame, df_name: str = "Unspecified file") -> None`**: 
    Exports the given pandas DataFrame to a Feather file in the Feather export subfolder.

    - **Arguments**:
        - `df` (pandas.DataFrame): The DataFrame to export.
        - `df_name` (str): The name of the file (without extension). Defaults to "Unspecified file".
    - **Returns**:
        - `None`

## Logs

The `FileExporter` class logs various events during the execution of its methods:

- **Folder creation**: Logs whether folders were successfully created or not.
- **CSV and Feather exports**: Logs success messages when DataFrames are exported correctly. Logs error messages if any issues arise during the export process.

## Usage Example

The following is an example of how to use the `FileExporter` class:

```python
from file_exporter import FileExporter
import pandas as pd

# Example DataFrame
df = pd.DataFrame({
    'col1': [1, 2, 3],
    'col2': [4, 5, 6]
})

# Create a FileExporter object
exporter = FileExporter(common_export_folder="/path/to/export/folder")

# Export the DataFrame to CSV
exporter.export_to_csv(df, df_name="example_data")

# Export the DataFrame to Feather
exporter.export_to_feather(df, df_name="example_data")
```

In this example, the `FileExporter` is instantiated with a root export folder. When the class is isntantiated, the directories are automatically created. The DataFrame is then exported to both CSV and Feather formats.

## Output Description

The FileExporter class creates the following directory structure for each execution in the main directory:

```php
<common_export_folder>/
    execution_<timestamp>/
        csv_files/
            LIVERAIM_DATA_<file_name>.csv
        feather_files/
            LIVERAIM_DATA_<file_name>.feather
```

Each execution will create a new folder using the timestamp to store the exported files. The subfolders csv_files and feather_files contain the respective exported data formats.

    LIVERAIM_DATA_example_data.csv
    LIVERAIM_DATA_example_data.feather

These files will be named based on the prefix LIVERAIM_DATA followed by the user-provided file name. In particular, for the `main.py` execution, the file names would be the panel names. File names would be, for example: 

    LIVERAIM_DATA_population.csv
    LIVERAIM_DATA_population.feather

During the main execution, the name `common_export_folder` is imported from the variable `DATAWAREHOUSE_PATH`, declared in the [`main_config` module](configuration_module.md#main_config-module).

## Error Handling

The FileExporter handles common export errors such as:

- `FileNotFoundError`: If the directory does not exist or cannot be accessed.
- `PermissionError`: If there are insufficient permissions to write to the directory.
- `OSError`: General errors related to file system operations.

These errors are logged, and the process continues without crashing the program.