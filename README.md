# **Project Name**    - Financial Loan Analysis


##### **Contribution**  - Duo
##### **Team Member 1** - Makwana Nirav
##### **Team Member 2** - Kadam Rushikesh


####**Guide** - Monica Gahlawat


# **Project Summary -**
This project focuses on analyzing a financial loan dataset to understand loan
application trends, borrower behavior, and overall portfolio performance. The
analysis covers key KPIs, loan quality classification, and multiple borrower

profiles to support data-driven lending decisions.

The KPI section includes tracking of Total Loan Applications, Total Funded Amount, Total Amount Received, Average Interest Rate, and Average Debt-to-Income Ratio (DTI). The project also evaluates Good Loans vs. Bad Loans by comparing percentages, application counts, funded amounts, and repayments.

Advanced exploratory data analysis (EDA) is performed through line charts, bar charts, donut charts, heatmaps, tree maps, and violin plots. The insights highlight monthly lending trends, regional lending patterns, loan term distribution, employment length impact, loan purpose breakdown, and homeownership influence on borrowing.
# **Problem Statement**

**Analyze the financial loan dataset to understand overall loan performance.

Calculate key KPIs:

* Total Loan Applications

* Total Funded Amount

* Total Amount Received

* Average Interest Rate

* Average Debt-to-Income Ratio (DTI)

* Classify and compare Good Loans vs. Bad Loans using counts, percentages, funded amounts, and received amounts.

* Identify monthly trends in loan issuance using issue dates.

* Analyze regional lending activity across states.

* Study loan distribution by term, employment length, and loan purpose.

* Examine how home ownership affects loan applications and amounts.

* Provide insights that support better lending decisions and portfolio risk management.


#### **Define Your Business Objective?**
* To analyze the financial loan dataset and evaluate overall loan performance.

* To calculate essential KPIs such as total applications, funded amount, received amount, interest rate, and DTI.

* To identify patterns, trends, and risk factors in borrower behavior and loan repayment.

* To compare Good vs. Bad loans to understand portfolio quality.

* To generate data-driven insights that help improve lending decisions, reduce risk, and enhance business performance.Answer Here.
# ***Let's Begin !***
## ***1. Know Your Data***
### Import Libraries
# Import Libraries

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
import warnings

print("succussfully import library")
### Dataset Loading
# Load Dataset
from google.colab import files
uploaded = files.upload()
# Load Dataset
F_loan = pd.read_excel("financial_loan.xlsx")
print("data read succussfully ")
### Dataset First View
# Dataset First Look
F_loan.head()
F_loan.tail()
### Dataset Rows & Columns count
# Dataset Rows & Columns count

print("Numbers of Raws and Columns:", F_loan.shape)
### Dataset Information
print("Numbers of Raws and Columns:", F_loan.shape)
#### Duplicate Values
# Dataset Duplicate Value Count
F_loan.duplicated().sum()
F_loan[F_loan.duplicated()]

#### Missing Values/Null Values
# Missing Values/Null Values Count
F_loan.isnull().sum()

# Visualizing the missing values
F_loan.isnull().sum().sum()

## ***2. Understanding Your Variables***
# Dataset Describe
F_loan.describe()
# **Total Loan Application**
total_loan_application = F_loan['id'].count()
print("total loan application:",total_loan_application)
# **Total Funded Amount**
total_funded_amount = F_loan['loan_amount'].sum()
total_funded_amount_millions = total_funded_amount/1000000
print("total funded amount : ${:.2f}M".format(total_funded_amount_millions))
# **Total Amount Received**
total_applications =F_loan["id"].count()
total_funded_amount = F_loan["loan_amount"].sum()
total_amount_received =  F_loan["total_payment"].sum()
avg_int_rate =  F_loan["int_rate"].mean()
avg_dti =  F_loan["dti"].mean()

print("\n--- KPI Summary ---")
print("Total Applications:", total_applications)
print("Total Funded Amount:", total_funded_amount)
print("Total Amount Received:", total_amount_received)
print("Average Interest Rate:", avg_int_rate)
print("Average DTI:", avg_dti)
### Check Unique Values for each variable.
# Check Unique Values for each variable.
F_loan.nunique()


## ***4. Data Vizualization, Storytelling & Experimenting with charts : Understand the relationships between variables***
###**Chart -** 1 Monthly Trends By Issue Date For Total Funded Amount
monthly_funded = ( F_loan.sort_values('issue_date').assign(month_name = lambda x : x['issue_date'].dt.strftime('%b %y'))
                 .groupby('month_name',sort=False )['loan_amount'].sum().div(1000000).reset_index
                 (name='loan_amount_millions'))

plt.figure(figsize=(10, 5))
plt.fill_between(monthly_funded['month_name'],monthly_funded['loan_amount_millions'],color='skyblue',alpha=0.5)
plt.plot(monthly_funded['month_name'],monthly_funded['loan_amount_millions'],color='blue')

for i, row in monthly_funded.iterrows():
    plt.text (i, row['loan_amount_millions']+0.1,f"{row['loan_amount_millions']:.2f}",
        ha='center', va='bottom',fontsize=9, rotation=0, color='black')

plt.title('total funded amount by month', fontsize=14)
plt.xlabel('month')
plt.ylabel("funded amount in millions")
plt.xticks(ticks= range(len(monthly_funded)),label=monthly_funded['month_name'],rotation=45)
plt.grid(True, linestyle ='--', alpha= 0.6)
plt.tight_layout()
plt.show()

##### 1. Why did you pick the specific chart?
* A line chart is the best way to show month-wise trends over time.

* It clearly highlights growth patterns, seasonality, and changes across months.
##### 2. What is/are the insight(s) found from the chart?
* Funded amount shows a consistent increase from Jan to Dec.

* Only a small dip in Feb, but strong recovery afterward.

* Highest funding occurs in Dec 21.Answer Here
### **Chart -** 2 Monthly Trends By Issue Date For Total Funded Amount
monthly_received = ( F_loan.sort_values('issue_date').assign(month_name = lambda x : x['issue_date'].dt.strftime('%b %y'))
                 .groupby('month_name',sort=False )['total_payment'].sum().div(1000000).reset_index
                 (name='received_amount_millions'))

plt.figure(figsize=(12, 5))
plt.fill_between(monthly_received['month_name'],monthly_received['received_amount_millions'],color='lightgreen',alpha=0.5)
plt.plot(monthly_received['month_name'],monthly_received['received_amount_millions'],color='green' ,linewidth=2)

for i, row in monthly_received.iterrows():
    plt.text (i, row['received_amount_millions']+0.1,f"{row['received_amount_millions']:.2f}",
        ha='center', va='bottom',fontsize=9, rotation=0, color='black')

plt.title('total received amount by month', fontsize=14)
plt.xlabel('month')
plt.ylabel("received amount in millions")
plt.xticks(ticks= range(len(monthly_received)),label=monthly_received['month_name'],rotation=45)
plt.grid(True, linestyle ='--', alpha= 0.6)
plt.tight_layout()
plt.show()
##### 1. Why did you pick the specific chart?
* A line chart clearly shows how the received amount changes month-by-month.

* It highlights repayment trends and seasonal patterns over time.
##### 2. What is/are the insight(s) found from the chart?
* Total received amount shows a steady upward growth from Jan to Dec.

* Minor flat movement in Feb–Apr, but strong increase afterward.

* Highest received amount occurs in Dec 21, showing strong year-end repayment.
## **Chart -** 3 Regional Analysis By State For Total Funded Amount
state_funding = F_loan.groupby('address_state')['loan_amount'].sum().sort_values(ascending=True)
state_funding_thousands = state_funding/100

plt.figure(figsize=(9,12))
bars=plt.barh(state_funding_thousands.index,state_funding_thousands.values,color='lightcoral')

for bar in bars:
    width = bar.get_width()
    plt.text(width + 10, bar.get_y() + bar.get_height()/2,
            f'{width:,.0f}k', va='center',fontsize = 12)

plt.title('total funded amount by state in millions')
plt.xlabel('funded amount (₹\'000)')
plt.ylabel("state")
plt.tight_layout()
plt.show()
##### 1. Why did you pick the specific chart?
* A horizontal bar chart is ideal to compare values across many categories (states).

* It clearly highlights top-performing and low-performing states.

* Works best when category names (state codes) are long and numerous.
##### 2. What is/are the insight(s) found from the chart?
* Top 3 states contribute the highest funded amounts:

  * CA (California) – extremely high, leading at 784k

  * NY (New York) – 420k

  * TX (Texas) – 312k

* These 3 states alone make up a large portion of total lending.

* Several states like ME, ND, LA, MS, TN contribute very low amounts (<10k).

* Funding is highly concentrated in a few states.
## **Chart -** 4 Interest Rate Distribution by Loan Purpose
plt.figure(figsize=(14,8))
sns.violinplot(x="purpose", y="int_rate", data=F_loan,color='green')
plt.title("Interest Rate Distribution by Loan Purpose")
plt.xlabel("Loan Purpose")
plt.ylabel("Interest Rate")
plt.xticks(rotation=90)
plt.show()

##### 1. Why did you pick the specific chart?
* A violin plot is ideal for showing the distribution and spread of interest rates for each loan purpose.

* It helps compare variation, median, and density across categories in one view.

* It reveals patterns that simple averages cannot show.
##### 2. What is/are the insight(s) found from the chart?
* Interest rates vary between 6%–25% across all loan purposes.

* Small Business, Medical, and Other loans show higher spreads, meaning riskier borrowers or inconsistent pricing.

* Car, Home Improvement, and House loans generally have lower and tighter interest rate ranges, indicating lower risk.

* Debt Consolidation and Credit Card loans have moderate but wider distributions, showing mixed borrower risk profiles.
## **Chart -** 5 Interest Rate Density
plt.figure(figsize=(10,5))
sns.kdeplot(data=F_loan, x="int_rate", fill=True, color="green")
plt.title("Interest Rate Density")
plt.show()
##### 1. Why did you pick the specific chart?
* A density plot (KDE plot) is used to understand the distribution and concentration of interest rates.

* It shows where most interest rates lie and how spread out they are.
##### 2. What is/are the insight(s) found from the chart?
* Most interest rates fall between 8% – 15%, showing a strong concentration in this range.

* There are multiple peaks, indicating different borrower risk groups.

* Very few loans have rates above 20%, meaning high-interest loans are less common.

* The distribution is right-skewed, showing some high-risk borrowers with high rates.Answer Here
## **Chart -** 6 Loan Status Count
plt.figure(figsize=(6,8))
sns.countplot(data=F_loan, x="loan_status", palette="coolwarm")
plt.title("Loan Status Count")
plt.xticks(rotation=45)
plt.show()
##### 1. Why did you pick the specific chart?
* A bar chart is ideal for comparing the number of loans in each status category.

* It quickly shows which loan status dominates (good or bad loans).
##### 2. What is/are the insight(s) found from the chart?
* A large number of Fully Paid loans indicates strong repayment performance and good borrower quality.

* Shows business stability and healthy historical loan recovery.
#### Chart - 7
plt.figure(figsize=(8,6))
sns.scatterplot(data=F_loan, x='annual_income', y='loan_amount', hue='loan_status')
plt.title("Loan Amount vs Annual Income")
plt.show()

## **5. Solution to Business Objective**
#### What do you suggest the client to achieve Business Objective ?
Explain Briefly.
Answer Here.
# **Conclusion**
Write the conclusion here.
### ***Hurrah! You have successfully completed your EDA Capstone Project !!!***


