# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
```
import numpy as np
import pandas as pd
from scipy import stats
df=pd.read_csv("Data_set.csv")
print(df.isnull().sum())
cols=['rating','current_overall_rank','watchers']
for col in cols:
    df[col]=df[col].interpolate()
new_cols=['show_name','aired_on','original_network']
for col in new_cols:
    df[col] = df[col].fillna(df[col].mode()[0])
print(df.isnull().sum())
```
<img width="915" height="791" alt="image" src="https://github.com/user-attachments/assets/7ad26382-067a-4154-aad3-3bada5fd02b7" />

```

import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("SAMPLEIDS.csv")

print(df.head())
print(df.shape)
print(df.columns)
print(df.info())
print(df.isnull().sum())
print(df.duplicated().sum())

df = df.drop_duplicates()


for col in df.select_dtypes(include="number").columns:
    if df[col].isnull().any():
        df[col] = df[col].fillna(df[col].median())

for col in df.select_dtypes(include="object").columns:
    if df[col].isnull().any() and not df[col].mode().empty:
        df[col] = df[col].fillna(df[col].mode()[0])

print(df.isnull().sum())

for col in df.select_dtypes(include="number").columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    outliers = df[(df[col] < lower) | (df[col] > upper)]
    print("\nIQR outliers in", col)
    print(outliers[[col]])

for col in df.select_dtypes(include="number").columns:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]
    print("\nZ-score outliers in", col)
    print(outliers[[col]])

df.to_csv("SAMPLEIDS_cleaned.csv", index=False)

print("Cleaned dataset saved successfully!")
```
<img width="951" height="732" alt="Screenshot 2026-10-26 152945" src="https://github.com/user-attachments/assets/3babc9a7-0dfe-4637-820b-30beee2b8fe1" />
<img width="547" height="582" alt="Screenshot 2026-10-26 152957" src="https://github.com/user-attachments/assets/0d7e8321-828f-44c3-8607-59da6384dc90" />
<img width="657" height="723" alt="Screenshot 2026-10-26 153013" src="https://github.com/user-attachments/assets/905c40c8-17c0-4fda-8516-0775fb314863" />
<img width="697" height="795" alt="Screenshot 2026-10-26 153027" src="https://github.com/user-attachments/assets/e45c3ca1-5aac-4f6f-96ed-fbdf0c396313" />

```

import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("heights.csv")

print(df.head())
print(df.describe())
print(df.isnull().sum())

for col in df.select_dtypes(include="number").columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print("\nUnusual measurements using IQR in", col)
    print(outliers[[col]])

for col in df.select_dtypes(include="number").columns:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]

    print("\nUnusual measurements using Z-score in", col)
    print(outliers[[col]])
```
<img width="660" height="590" alt="image" src="https://github.com/user-attachments/assets/3f8802ca-e1fc-4161-ae29-f809c174d271" />

```

import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("iris.csv")

print(df.head())
print(df.describe())

num_cols = df.select_dtypes(include="number").columns

for col in num_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print("\nIQR outliers in", col)
    print(outliers[[col]])

for col in num_cols:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]

    print("\nZ-score outliers in", col)
    print(outliers[[col]])
```
<img width="677" height="308" alt="image" src="https://github.com/user-attachments/assets/de660dca-a722-415e-9ffe-c7a5d2c001dd" />
<img width="473" height="790" alt="image" src="https://github.com/user-attachments/assets/e1d3d6a5-fed2-432f-b3bb-dffe32767dfb" />

```

import pandas as pd

df = pd.read_csv("Loan_data.csv")

print(df.head())
print(df.info())
print(df.isnull().sum())
print(df.duplicated().sum())

df = df.drop_duplicates()

num_cols = ["ApplicantIncome", "CoapplicantIncome", "LoanAmount","Credit_History","Loan_Amount_Term"]
cat_cols = ["Gender", "Married", "Education", "Self_Employed", "Property_Area","Dependents"]

for col in num_cols:
    if col in df.columns:
        df[col] = df[col].fillna(df[col].median())

for col in cat_cols:
    if col in df.columns:
        df[col] = df[col].fillna(df[col].mode()[0])
        df[col] = df[col].str.strip()

print("\nMissing values after cleaning:")
print(df.isnull().sum())

print("\nCleaned dataset:")
print(df.head())

df.to_csv("Loan_data_cleaned.csv", index=False)

print("Cleaned dataset saved successfully!")
```
<img width="781" height="757" alt="Screenshot 2026-10-26 153710" src="https://github.com/user-attachments/assets/a2048eaf-957b-4d0d-b1ba-8b64bd006ff5" />
<img width="717" height="590" alt="Screenshot 2026-10-26 153727" src="https://github.com/user-attachments/assets/ba071480-2b0a-472c-802a-b9778ef5d9b5" />
<img width="787" height="442" alt="Screenshot 2026-10-26 153736" src="https://github.com/user-attachments/assets/37df3587-b7fd-4650-b0ef-f17dd5273972" />


# Result
Thus the given data have been read and performed data cleaning and saved the cleaned data to a file.
