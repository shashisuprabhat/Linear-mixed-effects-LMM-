# Linear-mixed-effects-(LMM)
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
table(df$`Reason for RDMS warning`)
```
**2. Plotting**
Visual exploration of how RDMS behaves.
---
a. Boxplot of Speed by Warning Reason
```
library(ggplot2)

ggplot(df, aes(x = `Reason for RDMS warning`, y = `RDMS Speed`)) +
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
**3. Group-wise Summary**
How does average RDMS speed vary by reason?
```
df %>%
  group_by(`Reason for RDMS warning`) %>%
  summarise(
    Mean_Speed = mean(`RDMS Speed`, na.rm = TRUE),
    Count = n()
  )
```
**4. ANOVA or Kruskal-Wallis**
Check if RDMS Speed significantly varies between different reasons:
```
# If assumptions of normality met
anova_result <- aov(`RDMS Speed` ~ `Reason for RDMS warning`, data = df)
summary(anova_result)

# If data not normally distributed
kruskal.test(`RDMS Speed` ~ `Reason for RDMS warning`, data = df)
```
**5. Outlier Detection**
Check for speed outliers at time of RDMS warnings:
```
boxplot(df$`RDMS Speed`, main = "Outliers in RDMS Speed")
```
***6. Time Series or Sequential Pattern (if timestamp exists)***
If you have a time column, explore RDMS warnings over time:
```
# Assuming column 'Timestamp' exists
df$Timestamp <- as.POSIXct(df$Timestamp)

ggplot(df, aes(x = Timestamp, y = `RDMS Speed`, color = `Reason for RDMS warning`)) +
  geom_point() +
  theme_minimal()
```
***7. Driver/Vehicle Behavior Comparison***
If you have a Driver ID or Vehicle ID:
```
df %>%
  group_by(`Driver ID`) %>%
  summarise(MeanSpeed = mean(`RDMS Speed`, na.rm = TRUE))
```
