# Executive Summary

The LIVERAIM warehouse is a high-quality, scalable, and interoperable data warehouse specifically designed for research on liver disease. Built to support clinical and biomedical research, the data warehouse integrates patient data from multiple cohorts and standardizes it into a unified, homogeneous format. It follows a ETL (Extract, Transform, Load) type process, ensuring data quality, consistency, and interoperability across multiple patient cohorts. The warehouse is composed of various tables, including patient demographics, medical history, blood test results, and physical examination details, which are stored in either wide or long formats. 

By using modern data processing tools and complying with security standards, LIVERAIM warehouse ensures that sensitive patient data is protected while providing a flexible and scalable research environment. The datasets can be exported in multiple formats and is compatible with major data analysis platforms, making it a valuable tool for the scientific and clinical communities involved in liver disease research. 

# Table of Contents

- ### [Overview](dataflow.md)
      A brief description of the data flow and key concepts.

- ### [LIVERAIM Database Specifications](liveraim_data_warehouse_specifications.md)
      Documentation about the LIVERAIM database structure.

  - ### Modules Documentation

      1. [File Reading utils](modules_documentation/file_reading_utils_doc.md)
            - Utilities for reading files.

      2. [Preprocessing utils](modules_documentation/data_processing_utils_doc.md)
            - Functions and utilities for data preprocessing.

      3. [Cohort Utils](modules_documentation/cohort_utils_doc.md)
            - Tools to create a Cohort object and format cohort data into a common structure.

      4. [Biomarker Utils](modules_documentation/biomarker_utils_doc.md)
            - Tools to process and check biomarkers data.

      5. [Quality Control utils](modules_documentation/qc_checks_utils_doc.md)
            - Tools for data quality control.

      6. [File Exporting utils](modules_documentation/file_exporting_utils_doc.md)
            - Modules for exporting files in various formats.

      7. [SQL Exporting utils](modules_documentation/sql_exporting_utils_doc.md)
            - Utilities for exporting data to SQL databases.

      8. [Configuration module](modules_documentation/configuration_module.md)
            - Documentation for the configuration module.

- ### [Quick Start Guide](quick_start_guide.md)
      A guide to quickly get started with the documentation and modules.
