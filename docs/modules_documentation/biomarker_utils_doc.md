# `biomarker_utils` Documentation



## General Sturcture
This module defines the following classes:

- `BMKDataReader`: Responsible for centralizing the process of reading biomarker data.
- `BMKDataProcessor`: Handles the processing, formatting, and validation of biomarker data. 
  The resulting data is ready for export in `.csv` or `feather` format as well as to a 
  **MySQL database**.

## Functions and Classes

In this sections are described the classes and functions defined in the module:

### Functions

  
- **`test_BMKDataReader() -> None`**
    
    If executed, it creates an instance of `BMKDataReader` and reads the biomarker data using the parameters set in `biomarkers_config`. If an error occurs, it prints the error. Otherwise, it prints the names of the files read and their hash.

    - **Logs**
        - If an error occurs during the instantiation of the `BMKDataReader` class.
        - If an error occurs while reading any of the files listed in the `bmk_data_directory` attribute of the `BMKDataReader` instance.
        - If no errors occur, the files read and their hash are printed.

### Classes

### Class `BMKDataReader`

#### Description
Reads all the data related to the biomarkers. It is responsible for reading and and organizing the files in a data dictionary, by version. This class is file-name dependant, so the files in the folder bmk-data_directory must meet the nomenclature (see read_bmk_data).

#### Attributes

- **`bmk_data_directory` (str)**: directory where de bmk raw data is stored. Variable exported from the bmk configuration file.
  
- **`read_files` (str):** list of the files that has been read (the file name and its hash). initialized as None.
  
- **`bmk_data_dict` (str):** dict whit all the read bmk data. Keys are each bmk version (date it was recieved) and values are the data read in DataFrame format. Initialized as None.

#### Methods

- **`read_bmk_data(self) -> 'BMKDataReader':`**

    Reads all files in the folder `self.bmk_data_directory`. The correct operation of this method depends on the proper naming of the files in the folder. BMK data files must follow the naming convention: `bmk_database_<version_date>`, where `<version_date>` is formatted as `yyyymmdd`. An example of a valid filename would be: `bmk_database_20241011`. 
    The method stores the read data in a dictionary, where keys are the different versions and values are DataFrames containing the BMK data.

    - **Returns**:
        - `BMKDataReader`: The updated instance of BMKDataReader with the attribute `bmk_data_dict` filled with the BMK data.

### Class `BMKDataProcessor`

Processes the biomarker data to meet the proper format and merges all the versions (data batches). It also includes a QC checks and manages the exportation of the QC results.

#### Attributes

- **`cohort_ids_dict` (dict)**: dictionary to map the cohort ids to the common liveraim ids. 
- **`cohort_ids` (set)**: set of original ids of all patients in liveraim database. 
- **`bmk_qc_dir` (str)**: directory where qc reports will be dumped. 
- **`raw_bmk_data` (pd.DataFrame)**: 
- **`formatted_bmk_data` (pd.DataFrame)**: df containing all bmk data ready to export.
- **`unmatching_ids` (set)**: ids in bmk data that do not match with any id in liveraim db.
- **`inconsistancies` (pd.Series)**: 
- **`bmk_summary_dict` (dict[str, pd.DataFrame])**: dict of dataframes with the qc reports 
of the bmk data.

#### Methods

- **`check_bmk_structure(self, df: pd.DataFrame) -> None:`**

    Checks if the DataFrame provided as a parameter contains the columns defined in the `biomarkers_config` module. It also verifies if there are any duplicates for the combination of ID and variable (biomarker/Prestacion).

    - **Arguments:**
        - `df` (pd.DataFrame): DataFrame to be checked.
  <br><br>

    - **Logs**:
        - Logs an error if the columns in biomarkers databases do not match the ones described in `biomarkers_config`.
        - Logs an error if finds any duplicates of the pair `ID(Paciente) - Variable(Prestacion)` 
  <br><br>

- **`process(self) -> 'BMKDataProcessor':`**
  
    Centralizes the formatting of `bmk_data`. For each version (data batch), applies the 
    following transformations:

      - `process_symbol`: Extracts symbols for "out-of-range" values and creates two additional
        columns (see `process_symbol` method).
      - `column_typing`: Assigns the appropriate data type to each column.
      - Generates a summary of the processing results for quality control (QC).
      - Renames the columns in biomarkers data. 
      - Add the `liveraim_id` column, the common identifier, to the patients that appear in the biomarker data and in the Liveraim database.
  <br><br>
  
    After processing, the method merges all DataFrames and checks for any inconsistencies 
    in the data. Finally, it assigns the merged DataFrame to the attribute `formatted_bmk_data` and stores the QC data in `bmk_summary_dict`.

      - **Returns**:
          - `BMKDataProcessor`: The updated instance of BMKDataProcessor with `formatted_bmk_data` and `bmk_summary_dict` attributes populated.
   <br><br>

- **`log_bmk_summary(self, df: pd.DataFrame) -> None:`**
  
    Logs a brief summary of the current biomarkers data.

    - **Arguments**:
        - `df` (`pd.DataFrame`): The DataFrame containing biomarker data to be summarized.

    - **Logs**:
        - Logs the count of missing numeric values in the biomarkers data.
        - Logs the count of "off limits" values found in biomarkers data, specifically values marked with symbols `<` or `>`.
        - Logs the total number of unique patients and unique biomarker labels in the data.
    <br><br>

- **`extract_symbol(self, value: str) -> float:`**

    Extracts the symbols '<' or '>' from a string (if present) and returns the numeric portion as a float. If the input contains "No calculable," it returns a NaN value. This method is intended to be used within a DataFrame's `apply` function (see `process_symbol`).

    - **Arguments**:
        - `value` (str): A string containing a numeric value and, in some cases, the symbol '<' or '>'.

    - **Returns**:
        - `float`: The numeric portion of the input value, or NaN if "No calculable" is found.
    <br><br>

- **`process_symbol(self, df: pd.DataFrame, regex: str = r'([<>])') -> None:`**

    Processes the symbols '<' or '>' in a specified column of the DataFrame and extracts the numeric values. This method creates two additional columns: one for storing the detected symbols (if any) and another for the numeric portion of the values. It has the following effects: 

    - Adds a new column to `df` named `bmk_config.LIMIT_DETECT_COLUMN`, containing any 
    extracted '<' or '>' symbols.
    - Adds another column named `bmk_config.NUMERIC_VALUE_COLUMN` with the numeric 
    portion of each value, processed by the `extract_symbol` method to handle 
    strings with symbols.

    - **Arguments**:
        - `df` (pd.DataFrame): The DataFrame containing the data to process. This DataFrame 
            should include the column defined by `bmk_config.VALUE_COLUMN`, which may 
            contain numerical values with '<' or '>' symbols.
        - `regex` (str, optional): The regular expression used to identify and extract symbols 
            from the `VALUE_COLUMN`. Defaults to `r'([<>])'`, which targets the symbols 
            '<' and '>'.
    <br><br>

- **`column_typing(self, df: pd.DataFrame) -> None:`**

    Assigns the appropriate data type to each column in the provided DataFrame based on the types specified in `bmk_config.python_column_types`. If a column's type is not found in this configuration, a warning is logged, and the column remains unmodified. Modifies `df` in place by changing the data types of its columns.
    
    - **Argumentss**:
        `df` (pd.DataFrame): The DataFrame containing the data to be typed. Each column's 
            data type will be set according to `bmk_config.python_column_types`.

    - **Logs**:
        - Logs a warning if a column is not found in `python_column_types`.
        - Logs an error if a column cannot be converted to the specified data type, 
        detailing the column and error message.
    <br><br>

- **`column_typing(self, df: pd.DataFrame) -> None:`**
    Maps and assigns `liveraim_id` values to the DataFrame based on a provided ID mapping 
    dictionary. If no mapping is provided, it defaults to `self.cohort_ids_dict`.

    - **Arguments**:
        - `df` (pd.DataFrame): The DataFrame containing the data to which `liveraim_id` values 
            will be assigned.
        `id_map` (dict, optional): A dictionary mapping original IDs (in `bmk_config.ID_COLUMN`) 
            to `liveraim_id` values. If not provided, `self.cohort_ids_dict` is used.

    - **Returns**:
        `pd.DataFrame`: The updated DataFrame with a new column, `bmk_config.LIVERAIM_ID_COLUMN`, containing the mapped `liveraim_id` values.
    <br><br>

- **` check_ids_coherence(self, df: pd.DataFrame, cohort_ids: set) -> pd.DataFrame:`**
    
    Checks for coherence between IDs in the provided DataFrame and a set of cohort IDs. Identifies and removes any unmatched IDs from the DataFrame to prevent potential errors during exportation to MySQL database. The effects are:

    - Sets `self.unmatching_ids` with the unmatched IDs found.
    - Modifies `df` by removing rows with unmatched IDs.

    - **Arguments**:
        - `df` (pd.DataFrame): The DataFrame containing the data with IDs to be checked.
        - `cohort_ids` (set): A set of valid cohort IDs for comparison against the IDs in the 
          DataFrame column defined by `bmk_config.ID_COLUMN`.
    
    - **Returns**:
            `pd.DataFrame`: The updated DataFrame with unmatched IDs removed.

    - **Logs**:
            - Logs a warning if unmatched IDs are found, detailing the count and indicating 
            that these IDs will be removed.
  <br><br>

- **`check_inconsistancies(self, df: pd.DataFrame) -> pd.DataFrame:`**
    Checks for inconsistencies in the provided DataFrame, including duplicate records and  value mismatches across duplicates, ensuring data compatibility for export to MySQL.

    This method identifies and handles:
    - Duplicate records based on key columns, retaining only the most recent record (based on validation date).
    - Inconsistent values across duplicate entries, logging any discrepancies found.

    - **Arguments**:
        `df` (pd.DataFrame): The DataFrame containing biomarker data to be checked for both 
            duplicate records and value inconsistencies.

    - **Returns**:
        `pd.DataFrame`: The DataFrame with duplicate records removed, where applicable.

    - **Logs**:
        - Logs the number of duplicates and any value inconsistencies found across duplicate
        entries.
  <br><br>

- **`export_qc_report(self) -> None:`**

    Exports the quality control (QC) report to an Excel file if the configuration allows it. The report includes data summaries, such as version summaries and biomarker-specific information, saved in an Excel workbook with separate sheets.

    This method uses the `config.EXPORT_QC_RECORD` setting to determine if the export should 
    proceed. If enabled, the report is saved in a directory defined by  `config.QC_RECORD_EXPORT_FOLDER`, organized by execution time.

    Creates a directory at the specified export path if it does not already exist and exports the QC report to an Excel file at the location `{QC_RECORD_EXPORT_FOLDER}/qc_execution_<execution_time>/bmk_inconsistancies.xlsx`.
  <br><br>

- **`bmk_summary(self, df: pd.DataFrame, cohort_ids: set) -> dict:`**

    Generates a summary of key metrics for the given biomarkers DataFrame, including the      number of unique patients and biomarkers, counts of missing values, duplicate records, 
    unmatched IDs, and values marked as out-of-range.

    - **Arguments**:
        `df` (pd.DataFrame): The DataFrame containing biomarker data to summarize.
        `cohort_ids` (set): A set of cohort IDs to compare against the IDs in the DataFrame, used to count unmatched IDs.

    - **Returns**:
        - `dict`: A dictionary containing the following summary metrics:
            - "number_patients": Count of unique patients in the data.
            - "number_biomarkers": Count of unique biomarkers in the data.
            - "missings": Total count of missing values in `NUMERIC_VALUE_COLUMN`.
            - "duplicates": Number of duplicate entries based on patient and biomarker ID.
            - "unmatching_ids": Count of IDs in `ID_COLUMN` that do not match any cohort IDs.
            - "off_limits": Count of values marked as out-of-range (with '<' or '>').
  <br><br>

- **`data_by_bmk(self, df: pd.DataFrame) -> pd.DataFrame:`**
    Aggregates biomarker data by calculating the count of missing values and the number of unique patients for each biomarker in the provided DataFrame.

    - **Arguments**:
        - `df` (pd.DataFrame): The DataFrame containing biomarker data to be aggregated. It 
            should include columns specified in `bmk_config.NUMERIC_VALUE_COLUMN` and 
            `bmk_config.ID_COLUMN`.

    - **Returns**:
        - `pd.DataFrame`: A DataFrame with one row per biomarker, containing the following columns:
            - `bmk_config.VARIABLE_COLUMN`: The biomarker identifier.
            - "missings": Count of missing values for each biomarker.
            - "patients_count": Number of unique patients associated with each biomarker.
  <br><br>

- **`bmk_summary(self, df: pd.DataFrame, cohort_ids: set) -> dict:`**
        
    Centralizes the quality control (QC) checks for the biomarker data. This method is intended to receive a DataFrame containing the formatted biomarker data, which will be checked for inconsistencies. If inconsistencies are found, they will be resolved by removing the necessary rows to maintain data integrity.
    Calls various internal methods to perform specific QC checks, such as boundary checks, data type validation, ID coherence, and inconsistency resolution.

    - **Arguments**:
        - `df` (pd.DataFrame): The DataFrame containing formatted biomarker data to be checked.

    - **Returns**:
        - `pd.DataFrame`: The updated DataFrame with any identified inconsistencies resolved by 
        removing affected rows.
    <br><br>

- **`compute_bmk_bounds(self) -> None:`**

    Method that creates the `bmk_bounds` attribute from the previously formatted biomarker data. For each biomarker, it extracts the upper and lower bounds (if present in the data) and stores them in a dictionary.
    Once created, the dictionary is assigned to the `bmk_bounds` attribute. The dictionary structure is as follows:

        {
            <bmk_label>: {
                "<": <lower_bound>, 
                ">": <upper_bound>},
            ...
        }

    - **Logs**:
        - If the biomarker data has not yet been processed, a warning is generated indicating that the data must be formatted before creating `bmk_bounds`.
        - If multiple bounds of the same type are found for the same biomarker, a warning about possible inconsistency in the bounds is issued.
    <br><br>

- **`check_bmk_bounds(self) -> None:`**

    Method responsible for checking the bounds of formatted biomarker data using the previously created `bmk_bounds` attribute. Specifically, it verifies if any values exceed or fall below the established bounds.

    - **Logs**:
        - If the `bmk_bounds` attribute has not yet been created, a warning is issued indicating that this attribute must be created first, then the method returns.
        - If any inconsistencies are found, i.e., values outside the `bmk_bounds` limits, a message with relevant error information is logged.
    <br><br>
    
## General Operation

This module centralizes the reading, processing, and quality control of biomarker data (unlike other modules, where these functionalities are separate). The biomarker data reading is managed by the `BMKDataReader` class (and not by the [`DataReader`](file_reading_utils_doc.md/#class-datareader) class). Once the class is instantiated, biomarker data can be read by calling the `read_bmk_data` method (it is important to verify the directory structure and file naming conventions before reading the data). Once read, the data is stored in the `bmk_data_dict` attribute (to ensure data is read correctly, see the section [Biomarker Data Reading Checks](#biomarker-data-reading-checks)).

To obtain processed biomarker data ready for export, the `BMKDataProcessor` class is required. Instantiating the class requires:

- `bmk_data_dict` (dict): A dictionary containing biomarker data. Keys represent the date of each version, while values are DataFrames containing the biomarker data.
- `cohort_ids_dict` (dict): A dictionary mapping cohort-specific identifiers (keys) to the common liveraim identifiers (values).

Once instantiated, the `process()` method centralizes the processing, transformation, and verification of biomarker data. The merged and formatted biomarker data is stored as a `pd.DataFrame` in the `formatted_bmk_data` attribute. This DataFrame is ready to be exported to `.csv`, `.feather`, or to a MySQL database. The method `export_qc_report` is resposible for exporting the data to the specified file types. `SQLExporter` is respondible of exporting data to MySQL database.

### Biomarker Data Reading Checks

To verify that the biomarker data reading configuration is correct, the `test_BMKDataReader()` function can be called. If any errors occur during reading, it will log them. If there are no errors, it will print the names of the files read (presumably all batches of biomarker data) and their respective hashes. Alternatively, you can execute the `biomarker_utils` module directly from the terminal to achieve the same effect:

```bash
python biomarkers_utils.py
```

This will produce the following output:

```plaintext

Instance of BMKDataReader successfully created.
Dir. path: data/biomarkers_data 
Data successfully read:

{'file_name': 'biomarker_data_20241016.xls', 'hash': 'e79002b53b2214e72643158dc033affe9ca43b443c7b912c03179ed215ef6b6c'}
```

## Usage in main execution

```python

... # Reading of cohorts data

# Reading biomarkers data
bmk_data: dict = BMKDataReader().read_bmk_data().bmk_data_dict

... # processing of cohorts data

# Processing of biomarkers data
bmk_processor = BMKDataProcessor(bmk_data, cohort_ids).process()
bmk_processed_data = bmk_processor.formatted_bmk_data

# Exportation of bmk data to csv and feather files
bmk_processor.export_qc_report()

...# instantitations of SQLExporter

# Exportation to MySQL DB of bmk data
sql_exporter.export_df_to_sql(bmk_processed_data, "biomarkers")


```