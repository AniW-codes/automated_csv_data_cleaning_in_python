# automated_csv_data_cleaning_in_python

```python


"""
Create a python application that can take datasets and clean the data
- It should ask for datasets path and name
- it should check number of duplicats and remove all the duplicates 
- it should keep a copy of all the duplicates
- it should check for missing values 
- if any column that is numeric it should replace nulls with mean else it should drop that row
- at end it should save the data as clean data and also return 
- duplicates records, clean_data 
"""

import pandas as pd
import openpyxl
import xlrd
import os


def dataCleaner101(file_path, file_name):

    if not os.path.exists(file_path):
        print("File does not exist")
        return
    else:
        if file_path.endswith(".csv"):
            print("It is a csv file")
            df_og = pd.read_csv(file_path,encoding_errors="ignore")

        elif file_path.endswith(".xlsx"):
            print("Its a .xlsx file")
            df_og = pd.read_excel(file_path)
        else:
            print("Please upload correct format - either xlsx or csv")
            return

    print(f"Data set has {df_og.shape[0]} rows and {df_og.shape[1]} columns")


    #Checking for duplicates

    bool_dup_df = df_og.duplicated()
    int_total_dups = df_og.duplicated().sum()

    print(f"Data set has {int_total_dups} duplicates")

    if int_total_dups > 0:
        dup_records = df_og[bool_dup_df]
        dup_records.to_csv(f"day19_practice_dups_{file_name}.csv",index=None)
    else:
        print("Dataset has no duplicates")

    #Delete duplicates
    df_no_dup = df_og.drop_duplicates()

    #Checking for null/missing values

    df_total_nulls = df_no_dup.isnull().sum().sum()
    null_df = df_no_dup.isnull().sum()
    print(f"The nulls are as follows in the dataset \n {null_df}")
    print(f"Total nulls in dataset are : {df_total_nulls} ")

    #deal with these nulls/missing values

    col_name = df_no_dup.columns

    #tracking nulls and replace them with means if num column
    for x in col_name:
        if df_no_dup[x].dtypes in (int,float):
            df_no_dup[x] = df_no_dup[x].fillna(df_no_dup[x].mean())
        else:
            df_no_dup.dropna(subset=[x],inplace=True)

    print("Data is cleaned now")
    print(f"After cleaning, your data set has {df_no_dup.shape[0]} rows and \n {df_no_dup.shape[1]} columns")

#Extracting into a .csv file finally
    df_no_dup.to_csv(f"day19_practice_final_{file_path}.csv",index=None)
    print("Data is saved")



if __name__ == "__main__":

    file_path = input("Enter file path: ")
    file_name = input("Enter file name: ")

dataCleaner101(file_path,file_name)


```
