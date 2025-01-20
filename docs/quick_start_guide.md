# Fast Configuration

This section describes the necessary configuration to run the code and generate the data warehouse for WP1. For a comprehensive description of all configuration parameters, refer to the section [Configuration Module](modules_documentation/configuration_module.md).

> **Important Notice**: Before configuring the project, ensure that the prerequisites described in the [Environment Setup](#enviroment-setup) section are met.

> **Note**: If you have downloaded the repository from GitHub, the structure of the entire repository should be correct. In this case, and if you want to run the code with the default configuration, only the following modifications would be needed:
>
> - Configure the `/data/cohort_name/databases` directory for each cohort. See the section [Structure of the `data/cohort_name/databases/` Directory](#structure-of-datacohort_namedatabases-directory).
> - Configure the MySQL DB connection parameters. See the section [MySQL Connection Configuration](#mysql-connection-configuration).

## Structure of the Project Directory

While the configuration allows some flexibility in the project directory structure, it is recommended to maintain the current structure to avoid errors. The project directory currently (as of 08/07/2024) has the following structure:

    LIVERAIM_WP1/
    ├── config/
    │   ├── cohort_config.py
    │   ├── connection_config.py
    │   ├── main_config.py
    │   ├── new_levels_config.py
    │   ├── new_var_config.py
    │   └── reference_names.py
    ├── data/
    │   ├── Alcofib/
    │   ├── Glucofib/
    │   ├── Decide/
    │   └── Liverscreen/
    ├── logs/
    │   ...
    ├── qc_report/
    │   ...
    ├── wp1_documentation/
    │   ...
    ├── requirements.txt
    ├── main.py
    ...
    other python modules

### Structure of `data/cohort_name/` Directory

Each subdirectory within `data/` contains the databases and files related to each cohort. These subdirectories should have the following structure:

    data/
    ├── cohort_name/
    │   ├── databases/
    │   │   ├── cohort_name_database_version_date.extension
    │   │   ...
    │   ├── cohort_name_level_data.xlsx
    │   └── cohort_name_var_data.xlsx
    ...

The `cohort_name_level_data.xlsx` and `cohort_name_var_data.xlsx` files are described in the section [Initial Data and Configuration data](liveraim_data_warehouse_specifications.md#initial-data-and-configuration-data). If you downloaded the repository from GitHub, you should not need to modify these files.

### Structure of `data/cohort_name/databases/` Directory

The `/data/cohort_name/databases` directory should store the files containing the raw data for the databases of each cohort. Each file in the directory corresponds to one of the received database versions. For more information on how different versions are handled, see the section [Managing Raw Data Versions](). It is **very important** (for this particular case) that the file names follow this structure:

    cohort_name_database_version_date.extension

For example, the raw data file for the Liverscreen cohort received on 07/29/2024 in .dta format would be named:

    liverscreen_database_20240729.dta

Note that the name is in lowercase and the date is in the *yyyymmdd* format.

> **Note**: This folder may contain only one file. It is recommended that this corresponds to the latest version of the data.

Since these data are not uploaded to GitHub, it is necessary to manually place them in the correct directory and format. Once this is done, the directory should be correctly configured.

## MySQL Connection Configuration

If you want to export the data warehouse to the MySQL DB (presumably yes), the MySQL connection parameters must be configured. Therefore, it is necessary to have MySQL installed (if you want to generate the DB locally) and to have a username and password. [A section explaining how to download MySQL and create a user is missing].

Once these requirements are met, you need to configure the MySQL DB connection parameters. These are defined in the `config.connection_config` module, but for a quick configuration, you do not need to modify this file. Instead, you should create the following environment variables in your execution environment:

* `USER` (string): Username (must have permission to access the database).
* `PASSWORD` (string): Database access password.
* `DATABASE_NAME` (string): Name of the database you want to access.
* `PORT` (string): Connection port.
* `HOST` (string): Database host name. If running the code locally on a computer, the host is probably 'localhost'. If running on a server, the host may be the server name or its IP address.

For information on setting environment variables, see the section [Creating Environment Variables](#creating-environment-variables).

For more information on MySQL connection configuration, see the section [Connection Config Module](modules_documentation/configuration_module.md#connection_config-module).

Once this is done, you should be able to run the code without any problems. In the [Outputs Configuration](#outputs-configuration) section, you can find some useful parameters to determine which outputs you want to generate.

**[A section explaining how to download MySQL and create a user is missing]**

## Outputs Configuration

There are some parameters in the `config.main_config` module that may be useful for users to quickly customize the outputs of the code execution:

* `EXPORT_QC_RECORD (bool)`: With a value of `True`, it exports the data generated during quality control. For more details, see the section [Quality Control Utils](modules_documentation/qc_checks_utils_doc.md). With a value of `False`, quality control is still performed, but the generated data is not saved after execution (except for those printed in the log).
* `EXPORT_FILES (bool)`: With a value of `True`, it exports the final data (the data warehouse, with the panel structure described in [???]()) in .csv and .feather format files. With a value of `False`, these files are not generated. For more details, see the section [File Exporting Utils Module](modules_documentation/file_exporting_utils_doc.md).
* `CREATE_SQL_DB (bool)`: With a value of `True`, it exports the data to a MySQL database (i.e., the data warehouse is created). With a value of `False`, the data is not exported to MySQL.

Access the `config.main_config` file and modify these variables according to your needs.

> Note: This list is not exhaustive. For more information, see the [Configuration Module](modules_documentation/configuration_module.md) section.

## Creating Environment Variables

<span style="color:red"> *Pending* </span>

# Enviroment setup

In this section, we explain how to set up the virtual environment to run the source code (and generate the data warehouse).

> ***Important Notice:*** It is recommended to work in an active virtual environment when installing the requirements and running the code. This isolates the project and its configuration, ensuring that it does not interfere with other projects. Make sure a Python virtual environment is set up and active before continuing with the ***setup***. If you haven't done so yet, refer to the section [Set Up and Activate a Virtual Environment](#set-up-and-activate-a-virtual-environment).

## Basic Requirements

To run the code, you need to have the following installed:

* `python` version 3.12.2 or higher.
* `pip` version 24.0 or higher.

To check if you have `pip` installed, open your terminal and run the command:
```sh
pip --version
> pip 24.0 from C:**\enviroment\Lib\site-packages\pip (python 3.12)
```

To check your version of `python`, open your terminal and run the command:

```sh
python --version
> Python 3.12.2
```

If you do not have a compatible version or are missing any of the listed requirements, you can see how to install them in the section [Installation of Requirements](#installing-requirements).

> **Note**: It is possible that the code may work with earlier versions of Python, although this has not been tested. It is strongly recommended to update Python to a compatible version. 

## Package Installation

All the packages required to run the code (and their respective versions) are listed in the `requirements.txt` file. When running the main code (`main.py`), it checks if these requirements are met. If any are not installed or have an earlier version than required, they will be installed and/or updated to the latest version. The `setup.py` module handles this process. For more details, see [setup_utils](#module-setup).

However, it may be useful for the user to install the necessary packages beforehand. Run the following command to see the packages installed in the active environment:

```sh
pip list
```

The output should look something like this:

    Package Version
    ------- -------
    pip     24.0

> **Note**: This result only contains `pip` as the installed module because the command was run in a newly created virtual environment.

To install the packages listed in `requirements.txt`, run the command:

```sh
pip install -r requirements.txt
```

Alternatively, you can run the setup_utils module individually. This will check if the necessary requirements are met in the same way as during the execution of main.py. To do this, run the command:

```sh
python setup_utils.py
```

If you now check the installed packages again with the pip list command, you should get something like this:

    Package                Version
    ---------------------- -----------
    et-xmlfile             1.1.0
    feather-format         0.4.1
    greenlet               3.0.3
    mysql-connector-python 9.0.0
    numpy                  2.0.1
    openpyxl               3.1.5
    pandas                 2.2.2
    pip                    24.0
    pyarrow                17.0.0
    pyreadstat             1.2.7
    ...
    pytz                   2024.1
    six                    1.16.0
    SQLAlchemy             2.0.32
    typing_extensions      4.12.2
    tzdata                 2024.1

This is a way to verify that the packages have been installed correctly.

## Installing requirements

If you don't have `python` (or `pip`), you can use the following resources:

- **Installing Python**: You can download Python from the [official Python website](https://www.python.org/). To install it correctly, follow the instructions in the [Python Installation Guide](https://docs.python-guide.org/starting/installation/#installation).
- **Installing pip**: If you have installed Python correctly from python.org, you should already have `pip`. To ensure it is installed or to fix any errors, you can check the guide [Installing Packages](https://packaging.python.org/en/latest/tutorials/installing-packages/).

## Set Up and Activate a Virtual Environment

To create and set up a Python virtual environment, we will use the `venv` module, which is included by default in Python starting from version 3.3. Other tools (e.g., the `virtualenv` module or the `conda` package manager) also allow you to create virtual environments, but they will not be covered in this documentation. The steps are as follows:

1. **Install Python**: Ensure that you have correctly installed Python beforehand. You can verify the installation by checking the current Python version with the command `python --version`.

2. **Navigate to the Project**: Open a terminal and navigate to the project directory where you want to create the virtual environment. Once there, run the command:

        python -m venv environment_name

    This command should create a virtual environment named `environment_name`. A folder with the name of the new environment should appear in the current directory. This folder should have a structure similar to the following:

        environment_name/
        ├── Include/
        ├── Lib/
        ├── Scripts/
        └── pyvenv.cfg

3. **Activate the Virtual Environment**: Once the virtual environment is created, and each time you want to work from it, you will need to activate it. Depending on the operating system you are using, you will need to use a different command. Make sure you are in the directory where the environment you want to activate is located and run one of the following commands:

    * On ***Windows***: From the terminal, run the command:

            enviroment_name\Scripts\activate

    * On ***Linux***: From the terminal, run the command:

            source enviroment_name/bin/activate

    * On ***macOS***: From the terminal, run the command:

            source enviroment_name/bin/activate

If the virtual environment is set up correctly, you should see the environment name in parentheses before the command line in your terminal. This indicates that the environment is active. Your terminal should look something like this:

    (environment_name) C:/Users/user_name/path/to/project

Now all the packages you install will be isolated in this virtual environment and will not interfere with other projects. Additionally, if necessary, you can configure which version of Python you want to use in each virtual environment.

To deactivate the virtual environment and return to the *global environment* or *system environment*, use the `deactivate` command in the terminal. You should see the environment name in parentheses disappear from the terminal.


## Cloning GitHub repository


The GitHub repository that contains the code and configuration files to create the Data Warehouse is private. To clone it, your GitHub account must have access permissions to the repository.

To clone the repository, open your terminal and navigate to the directory where you want to clone the repository. You can clone it using either an HTTPS or SSH connection:

- **HTTPS**: run the following command in the terminal:

        git clone https://github.com/ecaromolina/LiverAim_WP1.git

- **SSH**: run the following command in the terminal:

        git clone git@github.com:ecaromolina/LiverAim_WP1.git

After cloning, you can navigate to the newly created directory named after the repository. For example:
    
    cd LiverAim_WP1

These commands will only copy the branch set as the 'Default Branch'. In general, this will correspond to the code of the latest version of the database. To check all the available branches in the remote repository, you can run the following command:

    git branch -r

To switch to a specific branch after cloning, use:

    git checkout branch-name
