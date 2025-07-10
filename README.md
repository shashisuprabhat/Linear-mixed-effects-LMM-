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
**🔍 Step 3: Key Analyses You Can Do in RStudio**
---
1. Descriptive Statistics
Understand the general behavior of RDMS:
```
summary(df$`RDMS Speed`)
table(df$`Reason for RDMS Warning`)
```
**2. Plotting**
Visual exploration of how RDMS behaves.
---
a. Boxplot of Speed by Warning Reason
```
library(ggplot2)

ggplot(df, aes(x = `Reason for RDMS Warning`, y = `RDMS Speed`)) +
  geom_boxplot(fill = "skyblue") +
  theme_minimal() +
  labs(title = "RDMS Speed by Warning Reason")
```
b. Histogram of RDMS Speed
```
ggplot(df, aes(x = `RDMS Speed`)) +
  geom_histogram(binwidth = 5, fill = "darkgreen", color = "white") +
  theme_minimal()
```
