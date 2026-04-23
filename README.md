# Assignment 2 — Data Quality KPI Analysis of the Online Retail Dataset

## Overview

This assignment analyses the **Online Retail** dataset in order to evaluate its **data quality KPIs**.  
The main objective is to understand the overall quality of the dataset by examining key indicators such as completeness, consistency, duplicate records, outliers, latency, and accuracy.

The dataset appears to contain retail transaction records, including invoice details, product codes, quantities, prices, customer identifiers, dates, and countries.

---

## Dataset Description

The dataset contains **541,909 rows** and **8 columns** in its original structure:

- InvoiceNo
- StockCode
- Description
- Quantity
- InvoiceDate
- UnitPrice
- CustomerID
- Country

Later in the notebook, one additional derived column is created:

- YearMonth

So after transformation, the working dataframe contains **9 columns**.

The first and last rows shown in the notebook confirm that the dataset covers transactions from **December 2010 to December 2011**.

### Main descriptive statistics

For the numerical variables shown in `df.describe()`:

#### Quantity
- Count: 541,909
- Mean: 9.552250
- Standard deviation: 218.081158
- Minimum: -80,995
- Maximum: 80,995

#### UnitPrice
- Count: 541,909
- Mean: 4.611114
- Standard deviation: 96.759853
- Minimum: -11,062.06
- Maximum: 38,970.00

#### CustomerID
- Count: 406,829
- Mean: 15,287.690570
- Standard deviation: 1,713.600303
- Minimum: 12,346
- Maximum: 18,287

These values already suggest that the dataset contains some abnormal or extreme observations, especially for **Quantity** and **UnitPrice**.

---

## Data Quality KPI

### 1. Data Completeness

The completeness analysis shows that most fields are complete, but two columns contain missing values:

- **Description**: 1,454 missing values
- **CustomerID**: 135,080 missing values

#### Missing value percentages

- **Description**: **0.268311%**
- **CustomerID**: **24.926694%**
- All other columns: **0.000000%**

#### Interpretation

The dataset is generally complete for transaction-level operational fields such as invoice number, stock code, quantity, invoice date, unit price, and country.  
However, **CustomerID has a very high missing rate**, close to **25%**, which is an important quality issue. This reduces the reliability of customer-based analysis and weakens any KPI linked to customer behavior or identification.

---

### 2. Data Latency

The latency analysis shows:

- **Minimum Invoice Date**: `2010-12-01 08:26:00`
- **Maximum Invoice Date**: `2011-12-09 12:50:00`

This means the dataset covers approximately **one year of transactions**.

The notebook also reports:

- **Data age**: `4921 days`

#### Monthly distribution

The records are distributed by month as follows:

- 2010-12: 42,481
- 2011-01: 35,147
- 2011-02: 27,707
- 2011-03: 36,748
- 2011-04: 29,916
- 2011-05: 37,030
- 2011-06: 36,874
- 2011-07: 39,518
- 2011-08: 35,284
- 2011-09: 50,226
- 2011-10: 60,742
- 2011-11: 84,711
- 2011-12: 25,525

#### Interpretation

The dataset is not recent, so from a latency perspective it is **old data**.  
However, the time coverage is coherent and complete enough for historical analysis.  
The monthly charts also show that transaction activity increases toward the end of 2011, especially in **September, October, and November**.

---

### 3. Data Consistency

The consistency check shows the following data types:

- **InvoiceNo**: object
- **StockCode**: object
- **Description**: object
- **Quantity**: int64
- **InvoiceDate**: datetime64[ns]
- **UnitPrice**: float64
- **CustomerID**: float64
- **Country**: object
- **YearMonth**: period[M]

#### Interpretation

The structure is globally consistent with the meaning of the fields:

- transaction identifiers are stored as objects,
- numerical measures are stored in numeric formats,
- the invoice date has been correctly converted to datetime,
- YearMonth has been derived for monthly aggregation.

This indicates a **good structural consistency** after preprocessing.

---

### 4. Duplicate Records

The duplicate rate is reported as:

- **0.9721189350979592%**

So the dataset contains approximately **0.97% duplicate records**.

#### Interpretation

This duplicate rate is relatively low, but it is not negligible.  
Duplicate records may slightly distort counts, totals, and transaction-based KPIs if they are not handled appropriately.

---

### 5. Outliers

The outlier rate based on **UnitPrice** is reported as:

- **7.312482354048373%**

So approximately **7.31%** of the records are considered outliers for UnitPrice using the IQR method.

#### Interpretation

This is a significant proportion.  
It indicates that prices are not fully stable and that the dataset contains unusual or extreme price values.  
This is consistent with the descriptive statistics, where **UnitPrice** ranges from **-11,062.06** to **38,970.00**.

---

### 6. Data Accuracy

#### Price validity

The notebook reports:

- **Minimum recorded price**: `-11062.06`
- **Maximum recorded price**: `38970.0`
- **Number of records with negative UnitPrice**: `2`

This shows that there are at least a few clearly invalid price values.

#### Overall data accuracy KPI

The notebook reports:

- **Data Accuracy Rate**: **73.42%**
- **Invalid Value Rate**: **26.58%**

#### Additional accuracy indicators

- **Reference Data Match Rate (Country)**: **97.18%**
- **Duplicate Accuracy Rate**: **99.03%**
- **Unit Price Field Accuracy Rate**: **99.54%**

#### Interpretation

The price field itself is highly accurate overall, since only a very small number of records contain negative prices.  
The duplicate accuracy rate is also strong.

However, the **overall data accuracy rate is only 73.42%**, mainly because missing `CustomerID` values strongly reduce the share of fully valid records.  
Therefore, the main accuracy problem is not widespread corruption across all fields, but rather the incompleteness of customer identification.

---

## Summary

The **Online Retail** dataset is generally well structured and usable for analysis.  
It has a coherent schema, correct data types after preprocessing, a complete time range, and a low duplicate rate.

### Main strengths
- Strong structural consistency
- Full availability of most operational columns
- Low duplicate rate
- High unit price field accuracy
- Strong country reference match rate

### Main weaknesses
- A very high proportion of missing **CustomerID** values (**24.93%**)
- Some missing **Description** values
- Extreme values in **Quantity** and **UnitPrice**
- The presence of a few negative prices
- A noticeable **outlier rate of 7.31%** for UnitPrice
- An overall accuracy rate reduced to **73.42%**

---

## Conclusion

The analysis shows that the **Online Retail** dataset has an acceptable overall quality for exploratory and historical analysis, but it is not fully clean.

The dataset is strong in terms of **structure, consistency, and operational completeness**, since most core transaction fields are available and correctly formatted.  
However, its quality is weakened by the large number of missing **CustomerID** values, the presence of duplicate records, and several extreme or abnormal values in key numerical variables such as **Quantity** and **UnitPrice**.

In conclusion, the dataset is usable, but its quality KPIs show that it requires caution, especially for **customer-level analysis**, **price-based analysis**, and any interpretation sensitive to outliers or invalid values.
