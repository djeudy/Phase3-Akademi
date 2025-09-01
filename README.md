# SyriaTel Customer Churn Prediction - Phase 3 Project

## Overview

This project develops a binary classification model to predict customer churn for SyriaTel, a telecommunications company. Using machine learning techniques, we built predictive models to identify customers who are likely to "soon" stop doing business with the company, enabling proactive customer retention strategies.

The analysis follows an iterative modeling approach, comparing multiple algorithms to find the optimal solution for predicting customer churn while balancing business objectives of minimizing revenue loss and retention costs.

## Business and Data Understanding

### Stakeholder Audience
The primary stakeholders for this analysis are **SyriaTel business executives and customer retention teams** who need to:
- Identify at-risk customers before they churn
- Optimize retention marketing spend
- Reduce revenue loss from customer attrition
- Make data-driven decisions about customer relationship management

### Dataset Choice and Characteristics
The SyriaTel customer dataset (`bigml_data.csv`) contains **3,333 customer records** with telecommunications usage patterns and service details. Key characteristics include:

- **Target Variable**: Customer churn (14.5% churn rate - 483 churned customers)
- **Features**: 19 predictor variables including:
  - Customer demographics (state, account length)
  - Service usage patterns (day/evening/night/international minutes and calls)
  - Service plans (international plan, voice mail plan)
  - Customer service interactions (customer service calls)
  - Billing information (charges across different time periods)

**Business Context**: In telecommunications, customer acquisition costs are significantly higher than retention costs. Early identification of at-risk customers enables cost-effective retention strategies, making churn prediction a critical business capability.

## Modeling

### Iterative Modeling Approach
Following Phase 3 requirements, we implemented a systematic three-model approach:

#### Model 1: Baseline Logistic Regression
- **Rationale**: Established a simple, interpretable baseline using logistic regression
- **Configuration**: Standard logistic regression with feature scaling
- **Purpose**: Provide benchmark performance for comparison

#### Model 2: Hyperparameter-Tuned Logistic Regression  
- **Rationale**: Optimize baseline performance through systematic hyperparameter tuning
- **Configuration**: Grid search across regularization strength (C), class balancing, and solver algorithms
- **Optimization Strategy**: 5-fold cross-validation using AUC as scoring metric
- **Best Parameters**: `{'C': 0.1, 'class_weight': 'balanced', 'solver': 'liblinear'}`

#### Model 3: Decision Tree Classifier
- **Rationale**: Test alternative algorithm capable of capturing non-linear relationships
- **Configuration**: Max depth = 5, balanced class weights to prevent overfitting
- **Purpose**: Compare tree-based vs. linear modeling approaches

### Data Preprocessing
- **Feature Engineering**: Removed phone number (non-predictive identifier)
- **Categorical Encoding**: Label encoded state, international plan, and voice mail plan
- **Data Splitting**: 80/20 train-test split with stratification to maintain class balance
- **Scaling**: StandardScaler applied for logistic regression models

## Evaluation

### Model Performance Comparison

| Model | Test AUC | Precision | Recall | F1-Score | Accuracy | Overfitting Risk |
|-------|----------|-----------|--------|----------|----------|------------------|
| **Baseline LR** | **0.8166** | **0.5349** | 0.2371 | 0.3286 | 0.8591 | **Low (0.0086)** |
| Tuned LR | 0.8163 | 0.3510 | **0.7526** | **0.4787** | 0.7616 | Low (0.0117) |
| Decision Tree | 0.8049 | 0.6604 | 0.7216 | 0.6897 | **0.9055** | High (0.1257) |

### Business Impact Analysis
The models demonstrate the classic **precision-recall trade-off** in churn prediction:

- **High Precision Models** (Baseline LR): Minimize false alarms (wasted retention costs) but miss more churning customers (revenue loss)
- **High Recall Models** (Tuned LR): Catch more churning customers but generate more false alarms (higher retention costs)

### Model Selection Rationale
**Selected Model: Baseline Logistic Regression**

**Justification:**
1. **Highest AUC Score** (0.8166): Best overall discriminative ability
2. **Minimal Overfitting** (0.0086 difference): Most reliable generalization to new data
3. **Business Balance**: 53.5% precision provides reasonable confidence in churn predictions while managing false alarm costs
4. **Interpretability**: Linear model coefficients provide clear business insights for stakeholder decision-making

## Conclusion

### Key Findings
1. **Model Performance**: Achieved 81.7% AUC score, indicating good discriminative ability for churn prediction
2. **Business Trade-offs**: Precision-recall trade-off requires business decision on balancing retention costs vs. revenue loss
3. **Feature Importance**: Customer service calls, usage patterns, and plan types emerged as strong churn indicators

### Business Recommendations
1. **Implementation**: Deploy baseline logistic regression model for production churn scoring
2. **Retention Strategy**: Focus retention efforts on customers with churn probability > 50% (53.5% precision threshold)
3. **Cost-Benefit Analysis**: For every 100 customers flagged for retention, expect ~53 to actually be at-risk of churning
4. **Continuous Improvement**: Monitor model performance and retrain quarterly with new customer data

### Next Steps
- **A/B Testing**: Validate model impact through controlled retention campaign testing
- **Feature Engineering**: Explore derived features like usage trend changes over time  
- **Advanced Models**: Investigate ensemble methods for potential performance improvements
- **Real-time Scoring**: Implement model in production system for automated churn risk scoring

This analysis provides SyriaTel with a robust, interpretable churn prediction capability that balances business objectives while maintaining statistical rigor and practical implementability.