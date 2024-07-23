# Lending Club Case Study

## Table of Contents

- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Dataset Analysis](#Dataframe-analysis)
- [Approach](#approach)
- [Case Study Analysis](#case-study-analysis)
- [Conclusions](#conclusions)
- [Technologies Used](#technologies-used)
- [Acknowledgements](#acknowledgements)
- [Glossary](#glossary)
- [Team](#team)

## Problem Statement

Lending Club is a consumer finance company like Bajaj Finserv who provides personal loans to urban customers for various reasons depending upon different criteria.

As soon as organization receives a loan request from consumer, it has to make a decision for loan approval based on the profile

_Lending money to the 'potentail risky'_ customers is the biggest cause of financial loss. We can call it _Credit Loss_. It is the money which consumer refuses to pay back to the lender.

When a customer makes a **bad loan**, the customer then marked as **'defaulter'** and the loan is marked as *'charged_off'*loan by the organization.

Main objectives of this case study is to **help the lending organization by identifying potential defaulter** before lending loan and minimize the
**'Credit Loss'**.

Now, there are two usecases where organization may lose credit :-

- Consumers **Likely to repay money**, and if organization will sanction loan for them.
- Consumers **Likely to default loan**, and if Organization will sanction loan for them.

## Objectives

The purpose is to _identify potential defaulters_. Identification of such applicants using data analysis using the given [dataset](**loan.csv**), is the main target of this case study.

This will significantly reduce the credit loss for the organization.

From the given dataset we need to identify the **'Core Driving Factors'** using our analysis.

## DataSet Analysis

The dataset provided has the information about loans which were pased previously with defaulter,fully paid or ongoing status. We need to identify the pattern so that we can minimize bad loans.

The dataset has been generated after loan approval and disbursement and does not have any information on rejection procedure.
There are total 111 factors given from which we can summerize the story in three points

- Loan Amount from customer (col:- loan_amnt)
- Amount approved in past (col:- funded_amnt)
- Disbursed amount (col:- funded_amnt_inv)

### Analysis based on Domain Understanding

#### Leading Attribute (loan_status)

- _Loan Status_ - Key Leading Attribute (_loan_status_). The column has three distinct values
  - Fully-Paid - The customer has successfuly paid the loan
  - Charged-Off - The customer is "Charged-Off" ir has "Defaulted"
  - Current - These customers, the loan is currently in progress and cannot contribute to conclusive evidence if the customer will default of pay in future
    - For the given case study, "Current" status rows will be ignored

#### Decision Matrix (loan_status)

- **Loan Accepted** - Three cases

  - _Fully Paid_ - Customer paid all the amount with interest
  - _Current_ - Customers are paying the installments
  - _Charged-off_ - Customer did not pay the installment thus organization marked it as **'defaulter'**.

- **Loan Rejected** - We don't have information about it as rejected.

#### Important Columns

There are total **111** factors given in the dataset.

- **Customer Details**

  - _Annual Income (annual_inc)_ - Annual income of customer. Main factor to see if they can payoff the loan.

  - _Home Ownership (home_ownership)_ - If a customer owns a house then chances of getting loan will increase as organization will have a mortgage item to recover unpaid loan.

  - _Employment Length (emp_length)_ - Organization can check for job stability, Higher the tenure higher the chance to get loan.

  - _Loan(debt) to Income (dti)_ - It is the ratio between loan and income.Lower DTI, higher the chances to get a loan.

  - _State (addr_state)_ - Location of the borrower. We can use it to create a generic demographic analysis.
 
  - **Credit Utilization(revol_util)** - Revolving credit utilization ratio, also known as credit utilization rate, is the percentage of your total available credit that you're currently using. It's calculated by dividing the total amount of debt you have across all your revolving credit accounts by your total credit limit, and then multiplying that number by 100.

  - _Purpose (purpose)_ - Cause of the loan. Using this we can analysis which purposes having most defaulters.

  - _Loan Title()_ - The loan title provided by the borrower. Suspicious titles will have lower chances to get the loan.

- **Loan Attributes**

  - _Loan Ammount (loan_amnt)_ - The listed amount of the loan applied for by the borrower. If at some point in time, the credit department reduces the loan amount, then it will be reflected in this value.

  - _Installment (installment)_ - The monthly payment owed by the borrower if the loan originates.

  - _Grades(grade)_ - LC assigned loan grade. From here we can identify which grade of loans has most default cases.

  - _Policy(policy_code)_ - publicly available policy_code=1, new products not publicly available policy_code=2.Policy code 2 are loans made to borrowers that do not meet Lending Club's current credit policy standards. Thus policy 2 askers will have low chances to get the loan.

  - _Loan Date (issue_d)_ - The month which the loan was funded.

  - _Verification Status (verification_status)_ - Indicates if income was verified by LC, not verified, or if the income source was verified.

  - _Interest Rate (int_rate)_ - Interest Rate on the loan.

  - _Public Records (public_rec)_ - Derogatory Public Records.Lower value higher the chance of getting loan.

  - _Public Records Bankruptcy (public_rec_bankruptcy)_ - Number of bankruptcy records publically available. Higher the value, lower is the success rate.

  - _Term (term)_ - Total installment temrm, to repaly the loan.

#### Ignored Columns

Other than above any type of behavioural columns and normal descriptive columns will be ignored.
Also, we will check for the NaN or blank values.
The columns which will have null values more than 80%, anyway those will not contribute us. So we will remove them as well.

### Data Set Analysis based on understanding of EDA

#### Rows Analysis

- Summary Rows: No summary rows were there in the dataset
- Header & Footer Rows - No header or footer rows in the dataset
- Extra Rows - No column number, indicators etc. found in the dataset
- Rows where the **loan_status = CURRENT will be dropped** as CURRENT loans are in progress and will not contribute in the decision making of pass or fail of the loan. The rows are dropped before the column analysis as it also cleans up unecessary column related to CURRENT early and columns with NA values can be cleaned in one go
- Find duplicate rows in the dataset and drop if there are

### Columns Analysis of the Dataset

#### Drop Columns

- There are multiple columns with **NA values** only. The **columns will be dropped**.
  - _This is evaluated after dropping rows with `loan_status = Current`_
  - `(next_pymnt_d, mths_since_last_major_derog, annual_inc_joint, dti_joint, verification_status_joint, tot_coll_amt, tot_cur_bal, open_acc_6m, open_il_6m, open_il_12m, open_il_24m, mths_since_rcnt_il, total_bal_il, il_util, open_rv_12m, open_rv_24m, max_bal_bc, all_util, total_rev_hi_lim, inq_fi, total_cu_tl, inq_last_12m, acc_open_past_24mths, avg_cur_bal, bc_open_to_buy, bc_util, mo_sin_old_il_acct, mo_sin_old_rev_tl_op, mo_sin_rcnt_rev_tl_op, mo_sin_rcnt_tl, mort_acc, mths_since_recent_bc, mths_since_recent_bc_dlq, mths_since_recent_inq, mths_since_recent_revol_delinq, num_accts_ever_120_pd, num_actv_bc_tl, num_actv_rev_tl, num_bc_sats, num_bc_tl, num_il_tl, num_op_rev_tl, num_rev_accts, num_rev_tl_bal_gt_0, num_sats, num_tl_120dpd_2m, num_tl_30dpd, num_tl_90g_dpd_24m, num_tl_op_past_12m, pct_tl_nvr_dlq, percent_bc_gt_75, tot_hi_cred_lim, total_bal_ex_mort, total_bc_limit, total_il_high_credit_limit)`
- There are multiple columns where the **values are only zero**, the **columns will be dropped**
- There are columns where the **values are constant**. They dont contribute to the analysis, **columns will be dropped**
- There are columns where the **value is constant but the other values are NA**. The column will be considered as constant. **columns will be dropped**
- There are columns where **more than 65% of data is empty** `(mths_since_last_delinq, mths_since_last_record)` - **columns will be dropped**
- **Drop columns `(id, member_id)`** as they are **index variables and have unique values** and dont contribute to the analysis
- **Drop columns `(emp_title, desc, title)`** as they are **discriptive and text (nouns) and dont contribute to analysis**
- **Drop redundant columns `(url)`**. On closer analysis url is a static path with the loan id appended as query. It's a redundant column to `(id)` column
- **Drop customer behaviour columns which represent data post the approval of loan**
  - They contribute to the behaviour of the customer. Behaviour of the customer is recorded post approval of loan and not available at the time of loan approval. Thus these variables will not be considered in analysis and thus dropped
  - `(delinq_2yrs, earliest_cr_line, inq_last_6mths, open_acc, pub_rec, revol_bal, total_acc, out_prncp, out_prncp_inv, total_pymnt, total_pymnt_inv, total_rec_prncp, total_rec_int, total_rec_late_fee, recoveries, collection_recovery_fee, last_pymnt_d, last_pymnt_amnt, last_credit_pull_d, application_type)`

#### Convert Column Format

We will now check the datatypes of each columns and where we will see that data is **numeric type or there is a concatenation between numeric and string, we will comvert them into numreric datatype for the sake of analysis**.

- **term** - It must be in integer but it is object (String) type thus we will change it accordingly.
- **revol_util** - It must be float without percentage sign
- **int_rate** - It is also object type and we will convert it as float.
- **issue_d** - **_It is object type. For the sake of our analysis we are going to split it and create two different columns with Month and year._**
  Month will be **issue_month** and year **issue_year**. - **issue_month** - It is the first 3 initials of any month. **E.g. January-Jan,March-Mar,August-Aug** likewise. - **issue_year** - It is in 2000. E.g. **11 -> 2011, 08 -> 2008** likewise.
  **_Thus the value e.g. in dataset Dec-11 will be Dec and 2011 in two different columns._** And then we will drop **issue_d** column.

- **emp_length** - emp_length has also data like <1 year 10+ years which is not usable in analysis. Thus we need to normalize this data as well.Assuming <1 Year as 0 and 10+ year as 11 and converting the whole data into integer.
  We have found unique values -> **['< 1 year', '10+ years', '3 years', '8 years', '9 years','5 years', '4 years', '1 year', '6 years', '2 years', '7years']**.

  ***Conversion Assumptions***
  - **<1 year** - *0*
  - **10+ Years** - *11*
  - **Others will be as it is in integer value.**

Methods we will use

- **unique()** - Will give us the unique values of a column
- **apply()** - We can apply different function on the datatypes of columns and rows.
- **reset_index()** - To reset index serials.

#### Added new columns

- **Added loan_status_boolean where Charged Offs are False and Fully Paids are True**

### Ignored Rows and Columns because of missing data

- Columns with high percentage of missing values will be dropped **(65% above for this case study)**
- Columns with less percentage of missing value will be imputed
- Rows with high percentage of missing values will be removed **(65% above for this case study)**

## Approach

- Step 0 - Data Cleaning & Manipulation Checklist
- Step 1 - Dropping Rows - where loan_status = "Current"
- Step 2 - Dropping Columns based on EDA and Domain Knowledge
- Step 3 - Convert the data types
- Step 4 - Identify columns with blank values which need to be imputed
- Step 5 - Analysis of the dataset post cleanup
- Step 6 - Outlier Treatment
- Step 7 - **Analysis** - Univariate, Bivariate and Multivariate Analysis
- Step 8 - **Conclusions** Final Verdict.

## Case Study Analysis

## Analysis Summary Univariate

### Loan Analysis:-

- Most of the loan amount applications fall in the range of 3k to 9.7k.
- Max and Min Interest rate are 5.4% and 22.7% respectively.
- Most of the applications fall under Grade-B.
- Most of the applicants are from 40% to 45% credit utilization group.
- Most of the applicants have asked for 36 months tenure.
- Most of the installments are between 25 to 400.

### Applicants Analysis:-

- Maximum applicants have applied for 10k loan amount.
- Maximum reason for loan is debt_consolidation
- Maximum number of debt asking state is CA.
- Majority of loan applicants do not own house.
- Most of times sanctioned loan maount is funded.
- Maximum applicants fall between 4k to 60k income bracket.

### Yearly & Monthly (Time Series) analysis:-

- Max loan applications have been submitted in year 2011.
- Max loan applications have been submitted in month December.

### Correlation between loan and applicants' attributes

Upon checking the correlation between these data we found that there is a positive correlation between annual income, loan amount and funded amount.
Same is with tenure and loan amount.

But we can not draw a conclusion till now as we need to compare variables with eachother to get a specific pattern.

### Bivariate and Multivariate Analysis Summary :-

- **60 months** tenure has more chances to default.

- Highest risk of charge off's are in the grades of **B and C**.Grade **F and G** have very high chances of charged off. Probablity of charged off is increasing from **A to G**.

- Highest Charge Offs are in the employee **length of 10 Years and above**. High probablity of Charge Off's whose income range is less than 1 years.

- The home_ownership status of **MORTGAGE** and are at the highest risk of Charge Offs. **MORTGAGE status** also has the highest range of loan amounts increasing the risk.

- Highest risk of Charge Off's are the purpose of **debt_consolidation**. **Small Business** applicants have high chances of getting charged off. **Renewable_energy** has lowest risk of Charge Off's in volume.

- Debt applications from **NV (Neveda)** have high risk of Charge Offs.

- Annual Income range of **0-40K** has the highest risk and income range **80000+** has less chances of charged off.

- More **Annual Income** leads to less **dti**

- More **income** leads to more **funded amount**

- High **credit utilization** leads to bad loan.

- The **Very High (15+)** interest rates are in risk of Charge Off.

- 
## Conclusions

Upon analyzing on all the points, We have derived some points which Lending Club can follow to minimize bad loans.

- **Giving loan to the applicants having annual income higer than 40K and low interest rate (below 15%**) may minimize the bad loand.

- **Smaller term (36 months) can minimize the bad loans**.

- **Loan grading has a significant impact on loans**.

- **Loan can be provided who has own house**.

- **Pupose like small business and another loan repayment** has most numbers of defaulters.

- **Public bankruptcy record must be 0 or 1**. Higher the record value, higher the risk of bad loan.

- Address Cities has lower impact on loan status as well.

- **High dti(15+)** leads to more bad loans thus loan funding should be made on annual income.

- Month and year has lower impact.

- **Credit Utilization Ratio (revol_util)** has a big significance. High utilization leads to bad loan. Thus Lending Club should provide
  loans to lower utilization rate applicants.

  
## Technologies Used
- [Python - Version 3.12.4](https://www.python.org/ftp/python/3.12.4/)
- [numpy - Version 2.0.0](https://github.com/numpy)
- [pandas - Version 2.2.2](https://github.com/pandas-dev/pandas)
- [matplotlib - Version 3.9.0](https://github.com/matplotlib)
- [seaborn - Version 0.13.2](https://github.com/seaborn)
- [Jupyter Lab - Version 4.4.2](https://jupyter.org/install)

## Acknowledgements and References

- The project references insights and inferences from live presentation given by [Akashdeep Makkar](https://www.linkedin.com/in/akashdeep-makkar-12110880/)
- The project reference materials are from Upgrad,Kaggle and Udemy.
- 
## Glossary

- EDA - Exploratory Data Analytics

## Team
- [Subhrajit Pyne](https://www.linkedin.com/in/subhrajit-pyne-305a90b4/)
- [Utkarsh Srivastava](https://www.linkedin.com/in/utkarsh-srivastava-82129562/)
