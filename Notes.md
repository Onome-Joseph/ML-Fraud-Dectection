#### **Q1. Data Preprocessing and Cleaning**  

- **Handling Missing Values:**  
   - Rows where `oldbalanceDest` corresponds to recipient IDs starting with **"M"** were removed because such IDs are associated with **PAYMENT** transactions, which cannot be fraudulent.  
   - Missing values in **`newbalanceDest`** were investigated:  
     - For `CASH_IN` transaction types, `newbalanceDest` being 0.0 was identified as a missing value, since it does not align with the transaction amount.  
     - These inconsistencies were corrected or considered during the analysis.

- **Outlier Removal:**  An **obvious outlier** was detected and removed to ensure data quality and prevent skewed model results.

- **Feature Selection (Handling Multi-Collinearity):**  
   Multi-collinearity exists between certain features, which can degrade model performance:  
   - **`oldbalanceOrg`** and **`newbalanceOrg`** were analyzed. Only `newbalanceOrg` was retained as it represents the balance **after a transaction**, which is more relevant for fraud detection.  
   - Similarly, between **`oldbalanceDest`** and **`newbalanceDest`**, `newbalanceDest` was selected for the same reason.  

- **Irrelevant Columns Removed:**  
   - Columns like **`isFlaggedFraud`** were excluded as the goal was to develop an independent fraud detection model.  
   - Columns **`nameOrig`** and **`nameDest`** (customer IDs) were dropped due to a large number of unique values, making them unsuitable for inclusion as model features.  

- **Balancing the Dataset:**  The dataset was balanced to ensure equal representation of the output classes (`isFraud = 0` and `isFraud = 1`). This step addresses class imbalance and improves model accuracy.

---
#### Q2. **Fraud Detection Model Description**
The goal of this project was to build a **fraud detection model** that accurately identifies fraudulent financial transactions. The dataset contains various transaction details, such as balances, transaction types, and amounts. The target variable, `isFraud`, determines whether a transaction is fraudulent or not. 
- The final **Random Forest model** achieved an accuracy of approximately **0.80**.  
- Important features identified for predicting fraud include:  
   - Transaction **type**  
   - Transaction **amount**  
   - Sender's **new balance (newbalanceOrig)**  
   - Recipient's **new balance (newbalanceDest)**
    - The model successfully identifies fraudulent patterns while minimizing false positives and false negatives.
  The fraud detection model built using **Random Forest** provides a reliable and efficient solution for detecting fraudulent financial transactions. By carefully selecting features, addressing class imbalance, and fine-tuning hyperparameters, the model achieves strong performance.
---

#### **Q3. Features and Target Variable**  

- **Input Variables (Features):**  
   - **`type`**: The transaction type (e.g., PAYMENT, CASH_IN).  
   - **`amount`**: The amount involved in the transaction.  
   - **`newbalanceOrig`**: The balance of the sender after the transaction.  
   - **`newbalanceDest`**: The balance of the recipient after the transaction.  

- **Output Variable (Target):**   **`isFraud`**: A binary variable indicating whether the transaction is fraudulent (1) or not (0).

#### **Model Selection and Fine-Tuning**  

Three machine learning models were tested to determine the best-performing algorithm:  
1. **Random Forest Classifier**  
2. **Logistic Regression**  
3. **XGBClassifier (Extreme Gradient Boosting)**  

- **Hyperparameter Tuning:**  Hyperparameters were fine-tuned to achieve the best performance. For example, in the Random Forest model, the **`n_estimators`** parameter (number of trees) was optimized.  

- **Model Comparison:**  Random Forest and XGBClassifier achieved similar levels of accuracy (approximately **0.80**), but Random Forest was chosen due to its faster training time.  

---

#### **Q4. Model Performance and Evaluation Metrics**  

The model's performance was evaluated using the following metrics:  
- **Accuracy**: Overall percentage of correct predictions. The accuracy was 80% 
- **Precision**: Proportion of predicted fraudulent transactions that were actually fraudulent. The average precision score was 79%  
- **Recall**: Proportion of actual fraudulent transactions correctly identified by the model. The average recall score was 78%
---
#### Q5 & Q6. **Key factors that predict fraudulent customer**
- Transaction Type (type) – Certain types like "cash-out" or "transfer" may have a higher fraud likelihood.
- Transaction Amount (amount) – Abnormally large or unusual amounts can trigger suspicion.
- Source Account Balance (newbalanceOrig) – Significant balance drops may indicate fraud.
- Destination Account Balance (newbalanceDest) – Unusual balance increases may flag receiving fraudulent transfers.
---
#### Q7 What kind of prevention should be adopted while company update its infrastructure?
- Deploy my fraud detection model to monitor transactions in real time.
- Use the input variables **(type, amount, newbalanceOrig, newbalanceDest)** to flag suspicious transactions immediately.
- Use stress testing to check the accuracy, precision, and recall of my machine learning model under heavy loads.
- Monitor variables like amount or transaction type for shifts in behavior.
---
#### Q8  how would you determine if they work?
- Evaluate the classification metrics (Accuracy, Precision, Recall) of your Random Forest model after the update
- Fraud detection rate; Percentage of fraud successfully identified.
- False negative rate; Fraud cases missed by the model.
- Track metrics related to customer experience:
- Number of complaints regarding blocked legitimate transactions.
