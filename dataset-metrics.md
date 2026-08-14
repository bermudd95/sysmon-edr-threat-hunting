# Simulation Data Architecture & Aggregated Metrics

This document outlines the data distribution math used to populate the presentation layer of the interactive spreadsheet.

## 📈 Global Performance Metrics
*   **Total Target Cohort Size:** 500
*   **Total Unique Opens:** 367 (73.4%)
*   **Actionable Link Clicks:** 25 (5.0%)
*   **Credential Submissions:** 25 (5.0%)
*   **Defensive Phishing Reports:** 192 (38.4%)

## 🏢 Departmental Risk Variance Matrix
Aggregated via exact `COUNTIFS` logic looking across the 500-user tracking matrix:

| Operating Department | User Count | Email Open Rate | Click Rate | Credential Harvest Rate | Reporting Rate |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Finance** | 100 | 75.0% | **25.0%** | **25.0%** | **25.0%** |
| **Sales** | 100 | 75.0% | 0.0% | 0.0% | 33.0% |
| **Operations** | 100 | 70.0% | 0.0% | 0.0% | 33.0% |
| **HR** | 100 | 75.0% | 0.0% | 0.0% | 34.0% |
| **IT** | 100 | 67.0% | **0.0%** | **0.0%** | **67.0%** |

*Note: Data points can be verified dynamically by opening the master `.xlsx` sheet located in this directory.*
