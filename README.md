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
 **Other things we can do**
 If you want to identify outliers based on RDMS Speed using your own defined thresholds (i.e., Speed < 65 or Speed > 120), here’s how to:
Flag those values as outliers
Count how many there are
See their exact speeds and reasons
```
library(readxl)
library(dplyr)

# Load the Excel file (adjust the path if needed)
df <- read_excel("LMM DF.xlsx")

# Define outlier conditions
outliers <- df %>%
  filter(`RDMS Speed` < 65 | `RDMS Speed` > 120) %>%
  select(`RDMS Speed`, `Reason for RDMS warning`)

# Number of outliers
num_outliers <- nrow(outliers)

# Print number and the rows with outliers
cat("📌 Number of outliers in RDMS Speed:", num_outliers, "\n\n")
print(outliers)
```

***6. Time Series or Sequential Pattern (if timestamp exists)***
If you have a time column, explore RDMS warnings over time:
```
library(readxl)
library(ggplot2)
library(dplyr)

# Load Excel file
df <- read_excel("LMM DF.xlsx")

# Convert "Time stamp of warning" to POSIXct datetime format
df$`Time stamp of warning` <- as.POSIXct(df$`Time stamp of warning`, format = "%Y-%m-%d %H:%M:%S", tz = "UTC")

# Sort by time (optional)
df <- df %>% arrange(`Time stamp of warning`)

# Plot: RDMS Speed over time, colored by Reason
ggplot(df, aes(x = `Time stamp of warning`, y = `RDMS Speed`, color = `Reason for RDMS warning`)) +
  geom_point(alpha = 0.7) +
  theme_minimal() +
  labs(
    title = "RDMS Speed Over Time by Warning Reason",
    x = "Time of Warning",
    y = "RDMS Speed",
    color = "Reason"
  )

```
***7. Driver/Vehicle Behavior Comparison***
If you have a Driver ID or Vehicle ID:
Driver ID==Sample No. (as per our data)
```
df %>%
  group_by(`Sample No.`) %>%
  summarise(MeanSpeed = mean(`RDMS Speed`, na.rm = TRUE))
```



**Goal of Linear Regression**
---
A linear regression helps you analyze how one or more independent variables predict a dependent variable — in this case, likely:

Dependent variable (Y): RDMS Speed

Independent variables (X): e.g.,

Reason for RDMS Warning (categorical)

Time of Day, Driver ID, etc. (if available)

---
Step 1: Prepare Data
First, make sure categorical variables are treated as factors, and timestamp is parsed correctly if needed.(paste 2nd code after this code and enter)
```
# Treat Reason as a factor (categorical variable)
df$`Reason for RDMS warning` <- as.factor(df$`Reason for RDMS warning`)

# Optional: extract hour if you want to use it
df$Hour <- lubridate::hour(df$`Time stamp of warning`)
```
**Step 2: Run Linear Regression**
✅ Example 1: RDMS Speed ~ Reason for Warning
```
model1 <- lm(`RDMS Speed` ~ `Reason for RDMS warning`, data = df)
summary(model1)
```
**Example 2: RDMS Speed ~ Reason + Hour of Day**
```
model2 <- lm(`RDMS Speed` ~ `Reason for RDMS warning` + Hour, data = df)
summary(model2)
```
**linear relationship between RDMS Speed and RDMS Warning Time** 
---
Plot the data with a regression line.
```
library(readxl)
library(ggplot2)
library(lubridate)
library(dplyr)

# Load data
df <- read_excel("LMM DF.xlsx")

# Convert time column to POSIXct
df$`Time stamp of warning` <- as.POSIXct(df$`Time stamp of warning`, format = "%Y-%m-%d %H:%M:%S", tz = "UTC")

# Create numeric time (e.g., seconds since first warning)
df <- df %>%
  arrange(`Time stamp of warning`) %>%
  mutate(
    Time_Seconds = as.numeric(difftime(`Time stamp of warning`, min(`Time stamp of warning`), units = "secs"))
  )

# Plot linear relationship
ggplot(df, aes(x = Time_Seconds, y = `RDMS Speed`)) +
  geom_point(color = "blue", alpha = 0.6) +
  geom_smooth(method = "lm", se = TRUE, color = "red", linewidth = 1.2) +
  theme_minimal() +
  labs(
    title = "Linear Relationship Between RDMS Speed and Warning Time",
    x = "Time Since First Warning (Seconds)",
    y = "RDMS Speed"
  )
```
**linear relationship between RDMS Speed and RDMS Warning Time, with points colored by Reason for RDMS Warning**
---
Enhanced R Code
```
library(readxl)
library(ggplot2)
library(lubridate)
library(dplyr)

# Load the data
df <- read_excel("LMM DF.xlsx")

# Convert warning time to POSIXct
df$`Time stamp of warning` <- as.POSIXct(df$`Time stamp of warning`, format = "%Y-%m-%d %H:%M:%S", tz = "UTC")

# Create numeric time variable: seconds since the first warning
df <- df %>%
  arrange(`Time stamp of warning`) %>%
  mutate(
    Time_Seconds = as.numeric(difftime(`Time stamp of warning`, min(`Time stamp of warning`), units = "secs")),
    `Reason for RDMS warning` = as.factor(`Reason for RDMS warning`)
  )

# Plot: Speed vs. Time with color by Reason
ggplot(df, aes(x = Time_Seconds, y = `RDMS Speed`, color = `Reason for RDMS warning`)) +
  geom_point(alpha = 0.7) +
  geom_smooth(method = "lm", se = FALSE, size = 1) +
  theme_minimal() +
  labs(
    title = "RDMS Speed vs. Time Colored by warning Reason",
    x = "Time Since First Warning (Seconds)",
    y = "RDMS Speed",
    color = "Warning Reason"
  )
```


