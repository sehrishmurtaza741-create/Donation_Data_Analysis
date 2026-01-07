# Donation_Data_Analysis
Data Science project 
# ===============================
# Donation Data Analysis Project
# ===============================

# 1. Import Libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# -------------------------------
# 2. Load Data (Messy 30-row dataset)
# -------------------------------
data = {
    "Donation_ID": ["D001","D002","D003","D004","D005","D006","D007","D008","D009","D010",
                    "D010","D011","D012","D013","D014","D015","D016","D017","D018","D019",
                    "D020","D021","D022","D023","D024","D025","D026","D027","D028","D029"],
    "Donor_Gender": ["Female","Male","F","Male","Female","Male","Female","M","Female","Male",
                     "Male","Female","Male","Female","Male","Female","F","Male","Female","F",
                     "Female","Male","F","M","Female","Male","Male","F","Female","M"],
    "Donor_Age": ["25","34","29","","31","twenty two","40","28","35","50",
                  "50","27","","41","24","38","33","30","22","","29","36","","28","40","23","45","31","27","32"],
    "Donation_Type": ["Money","Food","Money","Clothes","Money","Money","Food","Money","Clothes","Money",
                      "Money","Money","Food","Money","Money","Clothes","Food","Money","Food","Money",
                      "Money","Food","Money","Clothes","Money","Money","Money","Food","Clothes","Money"],
    "Donation_Amount": ["5000","","Rs.3000","0","7000 ","2000","zero","4500","","10000",
                        "10000","3500"," ","8000","1500","NULL","2000","500","750","1000",
                        "6000","700","4000","850","900","5000","","2500","450","3000"],
    "City": ["Islamabad","Lahore","Karachi","Rawalpindi","ISB","Lahore","Karachi","isl","Lahore","Karachi",
             "Karachi","Rawalpindi","Islamabad","Lahore","Karachi","Rawalpindi","Islamabad","Karachi","Karachi","Lahore",
             "Rawalpindi","Islamabad","Lahore","Karachi","Islamabad","Karachi","Lahore","Rawalpindi","Islamabad","Lahore"],
    "Donation_Date": ["05-01-2024","2024/01/08","10-01-24","12 Jan 2024","2024-01-15","18-01-2024","2024.01.20","22-01-2024","2024-01-25","28/01/2024",
                      "28/01/2024","02-02-2024","05-02-2024","08 Feb 2024","2024/02/10","12-02-2024","15-02-2024","20-02-2024","23-02-2024","01-03-2024",
                      "05-03-2024","08-03-2024","12-03-2024","15-03-2024","20-03-2024","25-03-2024","28-03-2024","01-04-2024","05-04-2024","10-04-2024"],
    "Payment_Method": ["Cash","Cash","Bank","Cash","Online","CASH","Cash","Bank","CASH","Online",
                       "Online","Cash","Cash","Bank","Online","Bank","Online","Cash","Online","Bank",
                       "Cash","Online","Bank","Cash","Online","Cash","Bank","Online","Bank","Online"]
}

# Convert to DataFrame
df = pd.DataFrame(data)

# -------------------------------
# 3. Data Cleaning
# -------------------------------

# Remove extra spaces from text
df = df.applymap(lambda x: x.strip() if isinstance(x, str) else x)

# Convert Age to numeric
df['Donor_Age'] = pd.to_numeric(df['Donor_Age'], errors='coerce')

# Convert Donation_Amount to numeric
df['Donation_Amount'] = pd.to_numeric(df['Donation_Amount'].replace({'Rs.':'','zero':'0','NULL':'0'}, regex=True), errors='coerce').fillna(0)

# Fill missing values
df['Donor_Gender'] = df['Donor_Gender'].fillna("Unknown")
df['City'] = df['City'].fillna("Unknown")
df['Donation_Type'] = df['Donation_Type'].fillna("Unknown")
df['Payment_Method'] = df['Payment_Method'].fillna("Unknown")

# Remove duplicate rows
df = df.drop_duplicates()

# -------------------------------
# 4. Data Analysis
# -------------------------------

# Total donation amount
total_donation = df['Donation_Amount'].sum()

# Average donation
average_donation = df['Donation_Amount'].mean()

# Donations by city
donation_by_city = df.groupby('City')['Donation_Amount'].sum().sort_values(ascending=False)

# Donations by type
donation_by_type = df.groupby('Donation_Type')['Donation_Amount'].sum().sort_values(ascending=False)

# Age distribution
age_distribution = df['Donor_Age'].dropna()

print("Total Donation:", total_donation)
print("Average Donation:", round(average_donation,2))
print("\nDonation by City:\n", donation_by_city)
print("\nDonation by Type:\n", donation_by_type)
print("\nAge Distribution:\n", age_distribution.describe())

# -------------------------------
# 5. Data Visualization
# -------------------------------

# Bar chart: Donations by City
plt.figure(figsize=(8,5))
sns.barplot(x=donation_by_city.index, y=donation_by_city.values, palette="viridis")
plt.title("Total Donation Amount by City")
plt.xlabel("City")
plt.ylabel("Donation Amount")
plt.xticks(rotation=45)
plt.show()

# Pie chart: Donation Type Distribution
plt.figure(figsize=(6,6))
df['Donation_Type'].value_counts().plot.pie(autopct="%1.1f%%", colors=['#ff9999','#66b3ff','#99ff99'])
plt.title("Donation Type Distribution")
plt.ylabel("")
plt.show()

# Histogram: Donor Age Distribution
plt.figure(figsize=(8,5))
plt.hist(df['Donor_Age'].dropna(), bins=10, color='skyblue', edgecolor='black')
plt.title("Donor Age Distribution")
plt.xlabel("Age")
plt.ylabel("Number of Donors")
plt.show()
