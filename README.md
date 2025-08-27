# Patients_Blood_Test_Analysis_Using_Power_BI
Patients_Blood_Test_Analysis_Using_Power_BI


```markdown
# 📊 Patient Blood Test Data Analysis Dashboard

## Project Overview

This Power BI dashboard provides a comprehensive analysis of patient blood test data, offering insights into various health metrics such as Hemoglobin, Glucose, WBC, Platelets, and Cholesterol levels. The goal is to visualize patient data effectively for quick assessment, identification of risk factors, and trend analysis, drawing from a healthcare analytics dataset.

## Key Features & Visualizations

The dashboard is designed with several interactive sections to provide a holistic view of patient health:

   Patient Overview:
       Card Visuals for quick stats: Total Patients (e.g., 50 patients), Average Age (e.g., 40.76 years), Gender Distribution (e.g., 50% Male, 50% Female).
       Bar Chart showing the number of patients by Age Groups (e.g., 20-30, 31-40, 41-50, Above 50).
   Hemoglobin Levels Analysis:
       Gauge Chart displaying average Hemoglobin levels with defined ranges (low, normal, high).
       Conditional Formatting Table to highlight patients with Hemoglobin levels outside the normal range (e.g., below 13 for males or below 12 for females).
       "Hemoglobin Range Count of Patients" visual categorizing patients into Low (5), Normal (45).
   Glucose Levels Analysis:
       Bar Chart grouping patients by glucose levels (e.g., Normal <100 mg/dL, Prediabetes 100-125 mg/dL, Diabetes >125 mg/dL).
       "Glucose Range Count of Patients" visual showing patients categorized by High (21), Normal (18), Low (11).
       KPI Card to highlight patients with glucose levels above a certain threshold.
   WBC and Platelet Analysis:
       Scatter Plot for WBC vs. Platelets to identify correlations or outliers.
       Heat Map showing WBC count by Age Group and Gender.
   Cholesterol Insights:
       Histogram for distribution of Cholesterol levels.
       Stacked Column Chart showing Cholesterol levels by Age Group and Gender.
   Patient Risk Analysis:
       A dedicated section for "Patients Risk Analysis" categorizing patients into High, Moderate, and Low risk based on combined Glucose and Hemoglobin categories.
   Interactive Patient Table:
       A Table Visual displaying patient details (Name, Age, Gender, test values) with filters for age range, gender, and specific test abnormalities.
   Abnormal Value Highlight:
       Conditional Formatting is applied to tables to highlight cells with values outside normal ranges for various test parameters.
   Filters and Slicers:
       Interactive slicers for Gender, Age Group, Hemoglobin Range, and Glucose Range are available for dynamic filtering of data.

## DAX Formulas Used

Several Data Analysis Expressions (DAX) were created to enable the rich analytical capabilities of this dashboard. These include both calculated columns (which add new columns to the table based on row-level calculations) and measures (which perform aggregations dynamically based on the report's context).

### Calculated Columns

   `Age Group`
    ```DAX
    Age Group =
    SWITCH(
        TRUE(),
        bloodtest[Age] >= 20 && bloodtest[Age] <= 30, "20-30",
        bloodtest[Age] >= 31 && bloodtest[Age] <= 40, "31-40",
        bloodtest[Age] >= 41 && bloodtest[Age] <= 50, "41-50",
        bloodtest[Age] > 50, "Above 50",
        "Unknown"
    )
    ```
    Purpose: Categorizes each patient into specific age groups based on their `Age` value (e.g., "20-30", "31-40", "41-50", "Above 50"). An "Unknown" category is included for ages outside these ranges or blank.

   `Hemoglobin Range`
    ```DAX
    Hemoglobin Range =
    SWITCH(
        TRUE(),
        bloodtest[Hemoglobin (g/dL)] < 12, "Low",
        bloodtest[Hemoglobin (g/dL)] >= 12 && bloodtest[Hemoglobin (g/dL)] <= 16, "Normal",
        bloodtest[Hemoglobin (g/dL)] > 16, "High",
        "Unknown"
    )
    ```
    Purpose: Categorizes individual patient hemoglobin levels into "Low" (below 12 g/dL), "Normal" (12-16 g/dL), or "High" (above 16 g/dL).

   `Hemoglobin Category by Gender`
    ```DAX
    Hemoglobin Category by Gender =
    SWITCH(
        TRUE(),
        'bloodtest'[Gender] = "Female" && 'bloodtest'[Hemoglobin (g/dL)] < 12.0, "Low (Female)",
        'bloodtest'[Gender] = "Female" && 'bloodtest'[Hemoglobin (g/dL)] >= 12.1 && 'bloodtest'[Hemoglobin (g/dL)] <= 15.1, "Normal (Female)",
        'bloodtest'[Gender] = "Female" && 'bloodtest'[Hemoglobin (g/dL)] > 15.1, "High (Female)",
        'bloodtest'[Gender] = "Male" && 'bloodtest'[Hemoglobin (g/dL)] < 13.0, "Low (Male)",
        'bloodtest'[Gender] = "Male" && 'bloodtest'[Hemoglobin (g/dL)] >= 13.8 && 'bloodtest'[Hemoglobin (g/dL)] <= 17.2, "Normal (Male)",
        'bloodtest'[Gender] = "Male" && 'bloodtest'[Hemoglobin (g/dL)] > 17.2, "High (Male)",
        'bloodtest'[Hemoglobin (g/dL)] >= 11.0 && 'bloodtest'[Hemoglobin (g/dL)] <= 16.0, "Normal (Child)",
        "Unclassified"
    )
    ```
    Purpose: Categorizes individual patient hemoglobin levels based on gender-specific World Health Organization (WHO) guidelines and clinical sources (e.g., <13.0 g/dL for men and <12.0 g/dL for women for "Low").

   `Glucose Category`
    ```DAX
    Glucose Category =
    SWITCH(
        TRUE(),
        'bloodtest'[Fasting Status] = "Fasting" && 'bloodtest'[Glucose (mg/dL)] < 70, "Low (Fasting)",
        'bloodtest'[Fasting Status] = "Fasting" && 'bloodtest'[Glucose (mg/dL)] >= 70 && 'bloodtest'[Glucose (mg/dL)] <= 99, "Normal (Fasting)",
        'bloodtest'[Fasting Status] = "Fasting" && 'bloodtest'[Glucose (mg/dL)] >= 100 && 'bloodtest'[Glucose (mg/dL)] <= 125, "Prediabetes (Fasting)",
        'bloodtest'[Fasting Status] = "Fasting" && 'bloodtest'[Glucose (mg/dL)] > 125, "Diabetic (Fasting)",

        'bloodtest'[Fasting Status] = "Postprandial" && 'bloodtest'[Glucose (mg/dL)] < 70, "Low (Postprandial)",
        'bloodtest'[Fasting Status] = "Postprandial" && 'bloodtest'[Glucose (mg/dL)] >= 70 && 'bloodtest'[Glucose (mg/dL)] <= 140, "Normal (Postprandial)",
        'bloodtest'[Fasting Status] = "Postprandial" && 'bloodtest'[Glucose (mg/dL)] > 140 && 'bloodtest'[Glucose (mg/dL)] <= 200, "Elevated (Postprandial)",
        'bloodtest'[Fasting Status] = "Postprandial" && 'bloodtest'[Glucose (mg/dL)] > 200, "Diabetic (Postprandial)",

        // General or unspecified fasting status
        'bloodtest'[Glucose (mg/dL)] < 70, "Low",
        'bloodtest'[Glucose (mg/dL)] >= 70 && 'bloodtest'[Glucose (mg/dL)] <= 100, "Normal",
        'bloodtest'[Glucose (mg/dL)] > 100 && 'bloodtest'[Glucose (mg/dL)] <= 125, "Prediabetes",
        'bloodtest'[Glucose (mg/dL)] > 125, "Diabetic",
        "Unknown"
    )
    ```
    Purpose: Categorizes patient glucose levels based on clinical guidelines, considering both fasting and postprandial states to assign "Low," "Normal," "Prediabetes," or "Diabetic" categories.

   `Glucose Category (92-118 Range)`
    ```DAX
    Glucose Category (92-118 Range) =
    SWITCH(
        TRUE(),
        'bloodtest'[Glucose (mg/dL)] < 92, "Low",
        'bloodtest'[Glucose (mg/dL)] >= 92 && 'bloodtest'[Glucose (mg/dL)] <= 100, "Normal",
        'bloodtest'[Glucose (mg/dL)] > 100 && 'bloodtest'[Glucose (mg/dL)] <= 118, "High",
        "Unclassified"
    )
    ```
    Purpose: Specifically tailors glucose categorization to a dataset's observed range of 92 to 118 mg/dL, categorizing values into "Low" (below 92), "Normal" (92-100), or "High" (101-118).

   `Patient Risk Category`
    ```DAX
    Patient Risk Category =
    SWITCH(
        TRUE(),
        [Glucose Category] = "Low" && [Hemoglobin Category] = "Low", "High Risk",
        [Glucose Category] = "Low" || [Hemoglobin Category] = "Low", "Moderate Risk",
        [Glucose Category] = "Normal" && [Hemoglobin Category] = "Normal", "Low Risk",
        "Unclassified"
    )
    ```
    Purpose: Assigns a risk level ("High Risk," "Moderate Risk," "Low Risk") to patients based on their previously defined `Glucose Category` and `Hemoglobin Category`. Patients with both Glucose and Hemoglobin categorized as "Low" are given the "High Risk" priority.

### Measures

   `Avg Hemoglobin`
    ```DAX
    Avg Hemoglobin = AVERAGE(bloodtest[Hemoglobin (g/dL)])
    ```
    Purpose: Calculates the average hemoglobin level from the `Hemoglobin (g/dL)` column, aggregating values dynamically based on the visual's context.

   `Categorize Average Hemoglobin`
    ```DAX
    Categorize Average Hemoglobin =
    SWITCH(
        TRUE(),
        [Avg Hemoglobin] < 12, "Low",
        [Avg Hemoglobin] >= 12 && [Avg Hemoglobin] <= 16, "Normal",
        [Avg Hemoglobin] > 16, "High",
        "Unknown"
    )
    ```
    Purpose: Categorizes the overall average hemoglobin value (calculated by `Avg Hemoglobin`) into "Low," "Normal," "High," or "Unknown." This is useful for displaying the general status in cards or tooltips.

   `Target HB`
    ```DAX
    Target HB = 14
    ```
    Purpose: Defines a static target value for hemoglobin as 14 g/dL, serving as a benchmark for comparison.

   `Compare to Target Hemoglobin`
    ```DAX
    Compare to Target Hemoglobin =
    IF(
        [Avg Hemoglobin] > [Target HB], "Above Target",
        IF(
            [Avg Hemoglobin] < [Target HB], "Below Target",
            "On Target"
        )
    )
    ```
    Purpose: Compares the `Avg Hemoglobin` with the `Target HB` to indicate whether the average is "Above Target," "Below Target," or "On Target".

   `Difference from Target HB`
    ```DAX
    Difference from Target HB = [Avg Hemoglobin] - [Target HB]
    ```
    Purpose: Calculates the numerical difference between the average hemoglobin and the target hemoglobin, providing a quantifiable deviation.

## Dataset

The project utilizes a Healthcare Analytics dataset attached to the repository containing patient blood test data.

## Technologies Used

   Power BI Desktop: For data modeling, DAX formula creation, and dashboard visualization.



---
```
