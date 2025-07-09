# Linear-mixed-effects-LMM-
Analysing the ADAS data
Analyses we can do in RStudio

**Step 1: Load the Data in R**
---
Use this code to load and inspect the Excel file:
"""library(ggplot2)

ggplot(df, aes(y = `RDMS Speed`)) +
  geom_boxplot(fill = "lightblue") +
  geom_hline(yintercept = 65, color = "red", linetype = "dashed") +
  geom_hline(yintercept = 120, color = "red", linetype = "dashed") +
  labs(title = "RDMS Speed with Outlier Thresholds",
       y = "RDMS Speed") +
  theme_minimal()


