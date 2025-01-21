# COPD Analysis

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Results](#results)

### Project Overview
Chronic obstructive pulmonary disease (COPD) is a serious lung condition affecting over 16 million Americans and ranks as the fourth leading cause of death in the United States. While COPD can result from factors like toxic inhalation or asthma, cigarette smoking remains the most common cause.

This project utilizes data from the COPDGene research group, which includes spirometry measurements from thousands of research participants. Spirometry is a critical tool for assessing COPD severity by measuring lung function through forced exhalation.

#### Key Spirometry Metrics Analyzed:
Forced Expiratory Volume (FEV1): Air volume exhaled in 1 second (lower values indicate worse disease).  
Forced Vital Capacity (FVC): Total volume of air exhaled after a full breath.  
FEV1/FVC Ratio: The ratio between FEV1 and FVC (smaller values indicate more severe disease).  
FEV1 (Phase 2): Follow-up FEV1 measurements taken five years later.  

By analyzing this dataset, we aim to identify patterns in COPD progression, assess risk factors, and provide insights that can support early diagnosis and improved treatment strategies.


### Data Sources
COPD Data: The primary dataset used for this analysis is the "copd.csv" file, containing biometric data of many patients. 

### Tools
- Excel - Data cleaning
- R - Analysis

### Data Cleaning/ Preperation
In the initial data preparation phase, we performed the following tasks:

1. Data loading and inspection.  
2. Handling missing values.  
3. Data cleaning and formatting.

### Exploratory Data Analysis

EDA involved exploring the COPD dataset to answer key questions, such as:  

- What is the distribution of COPD severity among participants?  
- How do spirometry measurements (FEV1, FVC, FEV1/FVC ratio) correlate with disease progression?  
- What factors contribute to a decline in lung function over time?

### Data Analysis

``` R
# 95% confidence interval
subsetdat1 <- dat1[dat1$smoking_status %in% c("Current smoker", "Former smoker"), ]
t.test(FEV1_phase2 ~ smoking_status, data = subsetdat1, conf.level = 0.95)

# Mean squared error function
mse <- function(true, pred) {
  return(mean((true - pred)^2))
}

# Random forest
fit3 <- randomForest(FEV1_phase2 ~ .,
                    data = train [, -(1:3)],
                    importance = TRUE,

                    # hyperparameters (change to improve predictions)
                    ntree    = 850,  # number of trees to fit
                    mtry     = 13,   # number of variables to sample per tree
                    nodesize = 10,    # minimum size of terminal nodes
                    maxnodes = NULL, # maximum number of terminal nodes a tree can have
                    )
# Calculate MSE on testing dataset
mse(valid$FEV1_phase2, predict(fit3, newdata = valid))

# Find predictions using random forest
FEV1_phase2_predictions <- predict(fit3, dat2)
preds <- data.frame(sid = dat2$sid, FEV1_phase2_predictions)

# Plot importance graph
options(repr.plot.width = 60, repr.plot.height = 8)

importance <- fit3$importance[, 1]
importance <- sort(importance, decreasing = TRUE) / max(importance)
barplot(importance)

```

### Results
The analysis results are summarized as follows:
1. There is a significant difference in FEV1 Phase 2 in the Current Smoker and Former Smoker categories.
2. FEV1 Phase 2 for the male group is significantly greater than the FEV1 Phase 2 for the female group.
3. FEV1 Phase 2 for the group without emphysema is significantly greater than the FEV1 Phase 2 for the group with emphysema.
4. There is a strong correlation between FEV1 and FEV1 Phase 2. If you have low FEV1, you are more likely to have low FEV1 Phase 2.
5. FEV1, FVC, FEV1 FVC ratio and percentage gas trapped in lungs were the most accurate predictors of FEV1 Phase 2.


   










