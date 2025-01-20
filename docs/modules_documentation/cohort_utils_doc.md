# `cohorts_utils` Documentation 

## Overview

This module defines the `Cohort` class, which centralizes the process of homogenizing data from different cohorts into a common format, determined by the configuration data. Additionally, it is responsible for distributing the selected variables into different panels and formatting them appropriately (as specified in `panel_data`) to allow for export in various formats.


## General Sturcture

The module consists of the following key components:

- **Functions responsible for the transformation and homogenization** of the cohort databases (in `pd.DataFrame` format).
- **`Cohort` class**: This class centralizes cohort data (both the raw database, configuration data, etc.) as well as the process of data homogenization and, finally, the distribution of variables into each panel. It is important to note that the arguments required to initialize `Cohort` are the result of preprocessing carried out by `DataPreprocessor`.
- **Auxiliary functions** used by `Cohort` to initialize attributes.

## Functions and Classes

In this sections are described the classes and functions defined in the module: 

### Functions

- **`manage_string_type(df: pd.DataFrame, var: str) -> None`**
    
    Converts the specified column `var` in the DataFrame `df` to string type.

    - **Arguments**:
        - `df` (pd.DataFrame): The DataFrame containing the data.
        - `var` (str): The name of the column to convert to string.

    - **Returns**:
        - `None`: This function does not return any value. It modifies the DataFrame in place.
  
    - **Logs**
        - If a **ValueError** occurs during conversion (e.g., invalid data), an error is logged.
        - If a **KeyError** occurs (i.e., the column does not exist in the DataFrame), an error is logged.
<br><br>

-  **`manage_category_type(...) -> None`**
    
    Converts the specified column `var` in the DataFrame `df` to the category data type, using a mapping provided in `level_data_json`.

      - **Arguments**:
          - `var` (str): The name of the column to convert to category.
          - `df` (pd.DataFrame): The DataFrame containing the data.
          - `level_data_json` (dict): A dictionary that contains the mapping (`map`) for the conversion of the column to category.

      - **Returns**:    
          - `None`: This function does not return any value. It modifies the DataFrame in place.
  
      - **Logs**
           - If a **ValueError** occurs during conversion (e.g., invalid data), an error is logged.
           - If a **KeyError** occurs (e.g., the column does not exist in the DataFrame), an error is logged.
           - If any other unexpected **Exception** occurs, an error is logged.
<br><br>

- **`manage_numerical_type(var: str,`
  `data_type: Literal['int64', 'float64'],`
  `df: pd.DataFrame,`
  `var_data_json: dict) -> None`**

    Converts the specified column `var` in the DataFrame `df` to a numerical data type (int or float), and applies a conversion factor if available.

    - **Arguments**:
        - `var` (str): The name of the column to convert.
        - `data_type` (Literal['int64', 'float64']): The target data type to convert the column to (`int64` or `float64`).
        - `df` (pd.DataFrame): The DataFrame containing the data.
        - `var_data_json` (dict): A dictionary containing additional information for the conversion, specifically the conversion factor.

    - **Returns**:
        - `None`: This function does not return any value. It modifies the DataFrame in place.

    - **Logs**:
        - If a **ValueError** occurs during conversion (e.g., invalid data), an error is logged.
        - If a **KeyError** occurs (e.g., the column does not exist in the DataFrame), an error is logged.
        - If any other unexpected **Exception** occurs, an error is logged.
<br><br>

- **`manage_datetime_type(var: str, data_type: str, df: pd.DataFrame) -> None`**
    
    Converts the specified column `var` in the DataFrame `df` to a datetime format, normalizing the date (removing time components).

    - **Arguments**:
        - `var` (str): The name of the column to convert to datetime.
        - `data_type` (str): The target data type (used in logging) to convert the column to.
        - `df` (pd.DataFrame): The DataFrame containing the data.

    - **Returns**:
        - `None`: This function does not return any value. It modifies the DataFrame in place.

    - **Logs**:
        - If a **ValueError** occurs during conversion (e.g., invalid datetime format), an error is logged.
        - If a **KeyError** occurs (e.g., the column does not exist in the DataFrame), an error is logged.
        - If any other unexpected **Exception** occurs, an error is logged.

    - **Notes**
        - The function uses `pd.to_datetime` to convert the column to datetime format and normalizes the date, setting the time to midnight (00:00:00).
        - In case of errors, detailed log messages are recorded with information about the variable and the type of error that occurred.
<br><br>

- **`set_liveraim_format(df: pd.DataFrame, var_data_json: dict, level_data_json: dict) -> None`**
    
    Converts the columns (variables) in the DataFrame `df` to the proper format for liveraim. The format (data types, categories codification, etc.) is configured in `var_data_json` and `level_data_json` dictionaries. Uses the functions defined above to transform each column according to its data type defined in `var_data_json`. 

    - **Arguments**:
        - `df` (pd.DataFrame): The DataFrame containing the data to be transformed.
        - `var_data_json` (dict): Contains relevant information about the variables, used to transform the data into a common format.
        - `level_data_json` (dict): Contains information about categorical variables, their levels, and the description codification of each level.

    - **Returns**:
        - `None`: This function does not return any value. It modifies the DataFrame in place.

    - **Logs**
        - Logs an info message when the data formatting process begins.
        - Logs a warning if the variables in the DataFrame are not a subset of the variables described in `var_data_json` or `level_data_json`.
<br><br>

- **`type_converter(value, data_type: str, dictionary: dict) -> any`**
    
    Converts a value to a specified data type using a provided converter map.

    - **Arguments**:
        - `value` (any): The value to convert.
        - `data_type` (str): The target data type (e.g., 'int', 'float', 'str').
        - `dictionary` (dict): A dictionary mapping data type strings to conversion functions.

    - **Returns**:
        - `any`: The converted value.

    - **Logs**
        - If an **Exception** occurs during the conversion process, an error is logged with details about the value, target data type, and the error itself. The program will then exit.
<br><br>

- **`levels_data_to_json(...) -> dict`**
    
    Converts a DataFrame containing configuration data about categorical variable levels into a dictionary and saves it as a JSON file if a path is provided. This dictionary will be used as a lighter object to format the variables.

    - **Arguments**:
        - `var_column` (str): Column name for the variable name.
        - `original_var_column` (str): Column name for the original variable name.
        - `original_code_column` (str): Column name for the original code value.
        - `common_code_column` (str): Column name for the common code value.
        - `description_column` (str): Column name for the description.
        - `original_data_type_column` (str): Column name for the original data type.
        - `xlsx_file` (str, optional): Path to the Excel file to read the DataFrame from. Default is `None`.
        - `df` (pd.DataFrame, optional): DataFrame containing the data. If `None`, the `xlsx_file` is used. Default is `None`.
        - `final_path` (str, optional): Path to save the JSON file. Default is `None`.

    - **Returns**:
        - `dict`: Dictionary containing the converted data.

    - **Logs**
        - Logs an error if the Excel file is not found (`FileNotFoundError`), if there's an issue with the format (`ValueError`), or if an unexpected exception occurs while reading the Excel file.
        - During the dictionary creation process, no specific logs are recorded, but the function handles type conversion using the `type_converter` function and logs errors accordingly.
        - If a JSON file is saved, a confirmation message is printed after saving the file.
<br><br>

- **`var_data_to_json(var_data: pd.DataFrame) -> dict`**
    
    Converts a DataFrame containing configuration data about core variables into a dictionary. This dictionary will be used as a lighter object to convert cohort databases into a homogeneous format.

    - **Arguments**:
        - `var_data` (pd.DataFrame): DataFrame containing the metadata about the core variables needed to set a common format between cohorts.

    - **Returns**:
        - `dict`: A dictionary containing the variables metadata stored in the DataFrame `var_data`.
<br><br>

- **`panel_data_to_json(panel_data: pd.DataFrame, var_data_json: dict) -> dict`**
    
    Converts a DataFrame containing information about the distribution of variables in panels into a JSON dictionary. It adds information from `var_data_json` to allow the building of the panels.

    - **Arguments**:
        - `panel_data` (pd.DataFrame): DataFrame containing the distribution of the variables in the chosen panels.
        - `var_data_json` (dict): Dictionary containing relevant information about the variables needed to build the panels, such as bounds for numerical variables and units.

    - **Returns**:
        - `dict`: A dictionary containing the information from `panel_data` with additional data needed to build the panels, such as bounds and units for numerical variables.

    - **Logs**
        - Logs an error if `panel_data` is empty and will not proceed with the conversion.
<br><br>

- **`var_data_merge(cohort_list: list[Cohort]) -> tuple[pd.DataFrame, dict, bool]`**

    Merges the `var_data` DataFrames of each cohort and creates a new common `var_data` DataFrame to be used in the instantiation of the merged cohort (Liveraim). It selects a subset of columns in the `var_data` DataFrames from the different cohorts, using the `var_data` from the first cohort as a reference to check for discrepancies. Only the selected columns will be taken into account for merging. It also checks for any unmatched variables across cohorts and reports discrepancies.

    - **Arguments**:
        - `cohort_list` (list[Cohort]): A list of cohorts whose `var_data` DataFrames will be merged.

    - **Returns**:
        - `pd.DataFrame`: The merged `var_data` DataFrame containing the selected columns from all cohorts.
        - `dict[str, tuples]`: A dictionary (`merge_meta`) containing rows (in tuple format) from each cohort’s `var_data` that have at least one discrepant value in a selected column (i.e., this row does not exist in the reference `var_data` DataFrame).
        - `bool`: `True` if the merging process was successful without discrepancies. `False` if there were discrepancies or if the input format was incorrect.

    - **Logs**
        - Logs an error if there is an issue with the `cohort_list`, such as being empty or not containing a list of valid `Cohort` objects.
        - No specific logs are recorded during the comparison of `var_data` rows, but discrepancies are recorded in `merge_meta`.

    - **Additional Information**:
        - The function defines an internal `compare_tuples` function to handle tuple comparison, especially in cases where NaN values are present.
        - If any cohort has rows in `var_data` that do not match the reference cohort, `correct_merge` is set to `False`.
<br><br>

- **`merge_cohorts(cohorts_list: list[Cohort], cohort_name: str) -> Cohort`**
    
    Merges the cohorts in `cohorts_list`. First, it merges the `raw_data` of each cohort, the `var_data` DataFrame (using the `var_data_merge` method), and the `level_data` DataFrame. It then processes the merged data to add the appropriate columns, drop duplicates, and finally creates a new instance of a `Cohort` using the merged data.

    - **Arguments**:
        - `cohorts_list` (list[Cohort]): A list of cohorts to be merged into a new cohort.
        - `cohort_name` (str): The name of the new merged cohort.

    - **Returns**:
        - `Cohort`: The new `merged_cohort` initialized with the merged data from the cohorts in `cohorts_list`.

    - **Logs**
        - Logs an info message indicating the start of the cohort merging process.
        - Logs an error if there is a problem during the merging of `var_data`, including information about discrepancies recorded in `merge_meta`.
    
    - **Additional Information**:
        - The function concatenates the `raw_data`, `var_data`, and `level_data` from each cohort.
        - After merging, it removes duplicates in the merged `var_data` and `level_data`.
        - The `level_data` is processed to ensure the correct mapping between original and final values, and the description column is dropped.
        - The resulting merged cohort is initialized with the merged data, using `'id'` as the ID variable and `'inclusion_date'` as the date variable.

### Classes

### Class `Cohort`

#### Description
This class is the main object for data processing and export in the LiverAim project. It takes `raw_data` (presumably preprocessed) as input, along with configuration data about the selected/core variables (`var_data`), configuration data about the levels of the selected/core categorical variables (`level_data`), and other relevant parameters that define each cohort. It then centralizes the process of homogenization and formatting of the cohort, creating the attribute `homogenized_data`, which has a common format across cohorts.

It also includes a method to distribute the variables in the final panels according to the configuration data in `panel_metadata` (see [initial data and configuration data](../liveraim_data_warehouse_specifications.md#initial-data-and-configuration-data) section for more specifications about this object).

#### Attributes

- **`cohort_name` (str):**  
  The name of the cohort being processed.

- **`status` (str):**  
  The status of the cohort. Set to `"ongoing"` by default.

- **`raw_data` (pd.DataFrame):**  
  Preprocessed data extracted from the ``DataPreprocessor`` class. This contains all the variables in the cohort in their original format, along with additional variables added in the preprocessor class.

- **`var_data` (pd.DataFrame):**  
  Preprocessed variable configuration data extracted from the `DataPreprocessor` class. This DataFrame contains the original variable information along with additional metadata added during preprocessing.

- **`var_data_json` (dict):**  
  A dictionary containing the same information as `var_data`, in a JSON-like format.

- **`level_data` (pd.DataFrame):**  
  Preprocessed level configuration data extracted from the `DataPreprocessor` class. This DataFrame contains original categorical variable information along with additional configuration data.

- **`level_data_json` (dict):**  
  A dictionary containing the same information as `level_data`, in a JSON-like format.

- **`id_variable` (str):**  
  The name of the variable used for patient IDs.

- **`date_variable` (str):**  
  The name of the variable representing the inclusion date for patients.

- **`selected_variables` (list):**  
  A list of the selected/core variables of the cohort. These are the original variable names from the cohort data.

- **`panel_data` (pd.DataFrame, optional):**  
  Configuration data about panels and the distribution of variables in each panel.

- **`panel_data_json` (dict, optional):**  
  A JSON-like dictionary containing the same information as `panel_data`.

- **`var_translate_dict` (dict):**  
  A dictionary used to translate specific cohort variable names to the common format names.

- **`homogeneous_data` (pd.DataFrame, optional):**  
  A DataFrame containing the selected variables, processed to fit the common LiverAim format.

- **`final_data` (dict[str, pd.DataFrame], optional):**  
  A dictionary where each key is a panel name, and the corresponding value is a DataFrame with the panel's data.

#### Methods

- **`__init__(self, cohort_name: str,`
  `raw_data: pd.DataFrame,`
  `var_data: pd.DataFrame,`
  `level_data: pd.DataFrame,`
  `id_variable: str,` 
  `date_variable: str,`
  `status: str = "ongoing")`:**

    Initializes the `Cohort` class with cohort data, variable and level configuration data, and relevant parameters such as ID and inclusion date variables. In particular it calls the `setup` method, responsible of creating the main attributes. Then, it initializes the attributes `_homogeneous_data` and `fina_data` as `None`, so the class can check if they have been instantiatied or not. 

    - **Arguments**:
        - `cohort_name` (str): The name of the cohort.
        - `raw_data` (pd.DataFrame): The preprocessed cohort data.
        - `var_data` (pd.DataFrame): The preprocessed variable metadata.
        - `level_data` (pd.DataFrame): The preprocessed level metadata.
        - `id_variable` (str): The patient ID variable.
        - `date_variable` (str): The inclusion date variable.
        - `status` (str, optional): The status of the cohort, defaults to `"ongoing"`.
<br><br>

- **`setup(self, cohort_name: str,`
  `raw_data: pd.DataFrame,`
  `var_data: pd.DataFrame,`
  `level_data: pd.DataFrame,`
  `id_variable: str,`
  `date_variable: str,`
  `status: str = "ongoing")`:**

    Sets up the main attributes of the cohort. This method is called within `__init__` but can also be used for updating data and configuration data.

    - **Arguments**:
        - Same as `__init__`.

    - **Returns**:
        - `None`.
  <br><br>

- **`homogenize_data(self) -> Cohort`:**
    
    Processes the raw data, selecting core variables, translating them to the final common name (using the `var_translate_dict` attribute) and formatting them according to the common LiverAim structure. It calls the function `set_liveraim_format` described in the section [Functions](#functions). Sets the `homogeneous_data` attribute.

    - **Returns**:
        - `Cohort`: Returns the `Cohort` instance with the `homogeneous_data` attribute set.
<br><br>

- **`set_final_data(self, panel_data: dict[str, pd.DataFrame]) -> Cohort`:**
    
    Creates and sets the final dataset structure based on panel data. The `panel_data` provides information on which variables belong to each panel and which should be "melted". 

    - **Arguments**:
        - `panel_data` (dict[str, pd.DataFrame]): A dictionary with panel names as keys and DataFrames describing the panel structure as values.

    - **Returns**:
        - `Cohort`: Returns the updated `Cohort` instance.
<br><br>

- **`set_var_translate_dict(self) -> None`:**
    
    Sets the `var_translate_dict` attribute using the data in `var_data` to translate variable names to the common LiverAim format.

    - **Returns**:
        - `None`.
<br><br>

- **`set_selected_variables(self) -> None`:**
    
    Sets the `selected_variables` attribute, which is a list of core variables based on the configuration data in `var_data`.

    - **Returns**:
        - `None`.
<br><br>

- **`set_panel_data(self, panel_data: pd.DataFrame) -> None`:**
    
    Sets the `panel_data` and `panel_data_json` attributes using the provided panel data.

    - **Returns**:
        - `None`.
<br><br>

- **`set_id(self, id_name: str = "liveraim_id") -> Cohort`:**
    
    Sets a new `liveraim_id` variable, a commmon-format identifier for each patient, and updates the cohort data with the new variable.

    - **Arguments**:
        - `id_name` (str, optional): The name of the new ID variable, defaults to `"liveraim_id"`.

    - **Returns**:
        - `Cohort`: Returns the updated `Cohort` instance.
  
#### Logs

- In `__init__`:  
    - Logs the initialization process and any errors related to setting up the cohort attributes.

- In `homogenize_data`:  
    - Logs the start and completion of the data homogenization process.

- In `set_final_data`:  
    - Logs the start and completion of the final data structuring process.

- In `set_id`:  
    - Logs the start and completion of the process to set the common ID (`liveraim_id`).

## General Operation

The instantiation of a `Cohort` requires three fundamental elements (in addition to other less critical parameters) that come from preprocessing by the `DataPreprocessor` class (see [Data Processing utils](data_processing_utils_doc.md)):

- **`raw_data`**: The cohort database, which is the result of merging different versions of the database for the same cohort.
- **`var_data`**: A DataFrame containing configuration data for the selected variables, along with those added during preprocessing.
- **`level_data`**: A DataFrame containing configuration data for the levels of selected categorical variables, as well as those added during preprocessing.

During instantiation, the attributes described in the class are created. Specifically, for the `var_data` and `level_data` DataFrames, an attribute with the same data is created but in dictionary format. This facilitates access to the information and subtly improves performance. Variables used as the **ID** and **inclusion date** are also defined, as they are particularly important. Additionally, a list of selected variables for the final dataset and a dictionary for translating their respective names into the common format are created.

Once the class is instantiated, the `homogenize_data` method can be called. This method uses `raw_data` to select, translate, and transform variables, creating a new DataFrame. This new DataFrame is stored in the `homogenized_data` attribute. These transformations (e.g., data type assignment, unit conversion, and level mapping for categorical variables) are centralized in the `set_liveraim_format` function.

After the data for all cohorts is homogenized, the `merge_cohorts` function takes a list of cohorts and merges them, returning a `Cohort` object in the common format. It does so by concatenating the `homogenized_data` DataFrames from each cohort and using them as the `raw_data` parameter to instantiate a new cohort. Similarly, the `level_data` and `var_data` files from each cohort are correctly "merged" for use as parameters in this new cohort instantiation. As a result, a new `Cohort` object is created, containing as raw_data the homogenized data from all the cohorts. 

The `set_id` method allows the creation of a new identifier variable in the `raw_data` attribute while updating the configuration data. This is useful for creating a common identifier across all cohorts once they have been merged (after homogenization).

Although the `raw_data` produced by `merge_cohorts` has the correct format, merging DataFrames can sometimes alter some of the data types. To prevent this, it is recommended to call the `homogenize_data` method again, which ensures that the data types are correct.

If you wish to create the final tables or panels (presumably after homogenizing, merging cohorts, and creating a new common ID variable), the `set_final_data` method generates a dictionary containing the different DataFrames, each representing a panel. It requires the `panel_data` dictionary as a parameter, which contains the configuration data for the panels. This method selects the variables that should appear in each panel and transforms the DataFrames to long format if necessary. The dictionary with the final DataFrames is stored in the `final_data` attribute.


## Usage in main execution

```python
...

# Create a list with the names of the cohorts to be used. 
cohort_name_list: list[str] = [cc.LIVERSCREEN_NAME, cc.GLUCOFIB_NAME, cc.ALCOFIB_NAME]

# Create an empty list to store all the cohort objects. 
cohort_list = []

# Iterates throgh all the cohort names to creat a cohort object for each cohort. 
for cohort_name in cohort_name_list:
    
    ...

    # Preprocess the configuration data and the databases with an instance of DataPreprocessor,
    #in this case cohort_preprocessor
    data, var_data, level_data = cohort_preprocessor.get_data()

    # Intantiate a Cohort object using the config data and call the homogenize_data method. 
    cohort = Cohort (cohort_name=cohort_name, 
                    raw_data=data,
                    var_data=var_data, 
                    level_data=level_data,
                    id_variable ='id',
                    date_variable='date_0').homogenize_data()

    cohort_list.append(cohort)

# Creates a cohort by merging all cohorts in cohort list
liverscreen_cohort = merge_cohorts(cohort_list).set_id().homogenize_data()

# Sets the attribute final_data, with all the final panels. 
liverscreen_cohort.set_final_data(panel_data)
```