# Linear-mixed-effects-LMM-
Analysing the ADAS data
Analyses we can do in RStudio

**Step 1: Load the Data in R**
---
Use this code to load and inspect the Excel file:
```
library(readxl)
library(dplyr)

# Load the Excel file (replace with your actual local path if needed)
df <- read_excel("LMM DF.xlsx")

# Preview structure and first rows
str(df)
head(df)
```
**Step 2: Understand the Columns**
---
Let’s assume your dataset includes columns like:

RDMS Speed: Speed of the vehicle when RDMS warning triggered

Reason for RDMS warning: Categorical variable like "lane departure", "sensor error", etc.

Possibly more: vehicle model, time, weather, driver ID, etc
```
colnames(df)
```

