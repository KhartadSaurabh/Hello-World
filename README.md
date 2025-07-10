# Hello-World

import pandas as pd
import re

df = pd.read_excel(r"path\Customerdetails.xlsx")
print(df)

df2 = pd.read_excel(r"path\Orderdetails.xlsx")

# Function to clean and validate phone number
def clean_number(number):
    if pd.isna(number):  # Check for NaN or None
        return None
    digits_only = re.sub(r'\D', '', str(number))  # Convert to string and remove non-digit characters
    return digits_only if len(digits_only) == 10 else None  # Return only valid 10-digit numbers

# Apply function to DataFrame
df["Phone"] = df["Phone"].apply(clean_number)

# Display result
print(df)

#df["Phone"] = df["Phone"].str.replace('-','')
#print(df["Phone"])

df[["Street_Adress","State","Zip_code"]] = df["Address"].str.split(',',2,expand=True)

#print(df[["Street_Adress","State","Zip_code"]])
print(df)

df["Paying Cstomer"].unique()
df["Paying Cstomer"] = df["Paying Cstomer"].str.replace("Yes","Y")
df["Paying Cstomer"] = df["Paying Cstomer"].str.replace("N0","N")
df["Paying Cstomer"] = df["Paying Cstomer"].str.replace("Na","N/a")
df["Paying Cstomer"] = df["Paying Cstomer"].str.replace("No","N")

df["Do not Contact"] = df["Do not Contact"].str.replace("No","N")
df["Do not Contact"] = df["Do not Contact"].str.replace("no","N")
df["Do not Contact"] = df["Do not Contact"].str.replace("N0","N")
df["Do not Contact"] = df["Do not Contact"].str.replace("Na","N/a")
df["Do not Contact"] = df["Do not Contact"].str.replace("nan","N/a")

df["Do not Contact"] = df["Do not Contact"].str.replace("Yes","Y")

# Joining two tables
New_Merge = pd.merge(df, df2, on='CustomerID', how='left')


# Corrected bucket function
def assign_bucket(Amount):
    if Amount < 10000:
        return 'Low'
    elif Amount < 50000:
        return 'Medium'
    elif Amount < 75000:
        return 'High'
    else:
        return 'Very High'
		
New_Merge['AmountBucket'] = New_Merge['Amount'].apply(assign_bucket)
print(New_Merge)


# Alternate condition using Lambda
New_Merge['Amount'].apply(lambda x: 'Low' if x <10000 else 'Medium' if x <50000 else 'High' if x <75000 else 'Very High')


#to delete a column 
New_Merge.drop(columns=[['Col1','Col2']],axis=1,inplace=True)
