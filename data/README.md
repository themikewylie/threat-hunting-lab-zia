# Data Directory

This folder contains the datasets used throughout the lab exercises.

## Dataset Overview

Each CSV file in this directory represents:

* Secure web gateway (Zscaler ZIA-style) proxy transactions
* Approximately **30–60 minutes** of activity
* From a **single user session**

The data simulates realistic enterprise browsing behavior and may include:

* Standard SaaS usage
* Background application traffic
* Web browsing activity
* Noise typical of corporate environments

Some datasets may also contain intentionally embedded anomalies for hunting purposes.

---

## How This Data Is Used

The CSV files in this folder will be used for:

* Baselining normal behavior
* Least Frequency of Occurrence (LFO) analysis
* HTTP/HTTPS transaction review
* Hypothesis-driven hunting exercises

You will analyze these files using:

* Bash (CLI stacking and filtering)
* Jupyter Notebook (Python + pandas)
* Optional spreadsheet pivots

---

## ⚠️ Important

Not everything unusual is malicious.

Your job is to:

1. Establish what “normal” looks like
2. Identify meaningful deviations
3. Defend your findings

Happy hunting!
