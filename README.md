Transaction Retrieval System
----------------------------
- __About__
  - This transaction retrieval application is build on `django-2.1.2`
  - You can check transaction details by transaction ID, by product in last `n` days and by product manufacturing city in last `n` days
  - Transaction data is being generated at a give interval, which is being stored in memory
  - Once a new transaction data file gets dumped at the data location, application will automatically load the changed data in memory
  - User will be always able to see updated transaction records
  - There are `four` urls from where user can see the transaction summary
    - To load and cahce transaction data: `/transaction/transactionSummary/{transaction_id}/`
    - By transaction ID available at: `/transaction/transactionSummary/{transaction_id}/`
    - By `Products` in last `n days` from current date is available at: `/transaction/transactionSummaryByProducts/{last_n_days}/`
    - By `Product Manufacturing City` in last `n days` from current date is available at: `/transaction/transactionSummaryByManufacturingCity/{last_n_days}/`

- __Requirements__
  - `django-2.1.2` or above
  - `python 3.6` or above
  - `yaml 3.13` or above
  - `pandas 0.23.4` or above
  - `requests 2.19.1` or above
  - `watchdog`
  - Web browser (example- `Chrome`, `Firefox`)

- __Modules__
  - Transaction Application
    - This is a django application, which will allow user to see transaction summary
    - It will load all `.csv` files from folder `data`(which can be changed in `config.py`)
    - Once the csv file is loaded will be moved into `data/processed` folder
    - On startup application will load all `.csv` files available in `data` and `data/processed` folder

  - Transaction Data Watcher
    - A watcher at the transaction data store
    - Responsible to call make transaction data loaded in memory as soon as new data gets
      dumped at transaction data storage folder/location
    - It will make hit the URL `/transaction/load_data/` to load and cache new transaction data
      in memory, as soon new transaction data is available at the location

  - Random Data Generator
    - This module is am optional module
    - This is responsible to keep generating random transaction data at a given interval
    - This module is created to test the application architecture and mock the data
    - It will also create and store the reference file at `data/reference/reference.csv` in case if 
      `reference.csv` is not available

  - Logger
    - Logger module will keep logging for each module in separate folders
    - Logs for each modules will be get stored in `applog` folder
    - For each module there will be separate available inside `applog` folder. Example
      - `applog/app/` folder to store all the transaction application logs
      - `applog/watcher/` folder to store logs of the transaction data watcher module
      - `applog/random_data_generator/` folder to store logs of the random data generator module

- __How to Use__
  - Clone this repository
  - Navigate to the cloned repository
  - Open command prompt and run below commands in same order
    - `python manage.py makemigrations`
    - `python manage.py migrate`

  - Common configurations are stored in `config.py`. Please feel free to modify as per your interest

  - Start Random Data Generator Module
    - Run this application if you want to generate random data
    - Run `python random_data_generator.py`
      - Command Line Arguments
        - Optional `-f` or `--frequency` Time interval in seconds to keep generating random transaction data.
          Default value is `120`.
        - Example- `python random_data_generator.py -f 10` or `python random_data_generator.py --frequency=20`
    - This will keep running will keep generating random transaction data at given interval
    - Logs will be stored in folder `applog/random_data_generator/`

  - Start Transaction Application
    - Open a `new command prompt` and run below command
    - `python manage.py runserver`
    - Now transaction application is running at `http://127.0.0.1:8000`
    - Available URLs: Below `four` urls are available
      - `/transaction/load_data/` To load and cache transaction data. Transaction data watcher will
        call this URL as soon a new transaction data pops up at the transaction data location
      - `/transaction/transactionSummary/{transaction_id}/` to get transaction details by passing a `{transaction_id}`
      - `/transaction/transactionSummaryByProducts/{last_n_days}/` to get transaction summary by
        Products in `{last_n_days}` from current date
      - `/transaction/transactionSummaryByManufacturingCity/{last_n_days}/` to get transaction summary
        by Products Manufacturing City in `{last_n_days}` from current date
    - Logs will be stored in folder `applog/app/`

  - Start Transaction Data Watcher
    - Open a `new command prompt` and run below command
    - Run `python monitor.py`
    - Command Line Arguments
      - `--host` host on which transaction application is running. Default `localhost`
      - `-p` or `--port` port on which transaction application is running. Default `8000`
      - Example `python monitor.py --host=127.0.0.1 --port=8080`
    - Logs will be stored in folder `applog/watcher/`
  
 - __Unite Test Coverage__
  - To run unittests, clone this repository
  - Navigate to the cloned repository
  - Open command prompt and run below commands in same order
    - `python manage.py makemigrations` (Run if you did not already ran this command)
    - `python manage.py migrate` (Run if you did not already ran this command)
    - `python manage.py test` To run the unittest tests coverage of the application
