## ML Binary Classification

### Task: Predict the lable: Eexited

### Explanations:
#### EDA:
train.csv contains 165034 samples and 14 columns. It does not contain null values. Three columns with string values (Surname, Geography, and Gender) contain spaces. The label "Exited" has an imbalance distribution with 130113 (79%) samples with 0 and 34921 (21%) with 1. Numerical features are also not normally distributed.
#### Preprocessing:
Columns with string values were lowercased and stripped of any white spaces. Since columns like id, CustomerId, and Surname had little use for training, they were dropped. train.csv was then shuffled and split into train and validation sets with a 7-3 ratio. Because of their non-normal distribution, numerical features were normalized instead of standardized. Categorical features were encoded based on their values. Normalization scaler and categorical feature encoders were first fitted on the train set and then used to transform both train and validation sets. To address the imbalance label issue, a new train set was created using an undersampler to balance the label.
#### Model Training:
Four types of models, logistic regression (baseline), random forest, XGBoost, and multi-layer neural network, were created. Each model was experimented on balanced and unbalanced train sets and evaluated on the validation set. The multi-layer neural network was also experimented on the un-normalized and un-encoded train set. It was done by discretizing numerical (continuous) features into bins and one-hot encoding them. Parameter tuning was performed on random forest, XGBoost, and multi-layer neural networks using grid search and cross-validation. The best-performing sets of parameters were used to create the "best" models for evaluations.
#### Model Evaluation:
Due to the imbalance issue, **F1 score** was selected for model evaluation and parameter tuning. The reason was that the accuracy of the majority label would dominate the metric. Using accuracy might also cause models to only capture the distribution of labels. F1 score provided a good balance between precision and recall. Since there was no clear business case for this assignment, a clear choice between precision or recall did not exist. Hence, F1 score was selected. Other metrics like AUC under ROC were also used for evaluation.
#### Evaluation Results & Discussion:
| Model  | Dataset | Precision | Recall | F1 | AUC |
| -------| ------- | --------- | ------ | ---| --- |
| Logistic Regression | Unbalanced&Normalized | 0.68 | 0.34 | 0.46 | 0.81 |
| Logistic Regression | Balanced&Normalized | 0.44 | 0.74 | 0.55 | 0.81 |
| Random Forest | Unbalanced&Normalized | 0.74 | 0.56 | 0.64 | 0.89 |
| Random Forest | Balanced&Normalized | 0.53 | 0.80 | 0.64 | 0.89 |
| XGBoost | Unbalanced&Normalized | 0.75 | 0.56 | 0.64 | 0.89 |
| XGBoost | Balanced&Normalized | 0.53 | 0.80 | 0.64 | 0.89 |
| Multi NN | Unbalanced&Normalized | 0.80 | 0.46 | 0.58 | 0.88 |
| **Multi NN** | Balanced&Normalized | 0.58 | 0.75 | 0.65 | 0.89 |
| Multi NN | Unbalanced&Un-normalized | 0.73 | 0.55 | 0.63 | 0.88 |
| Multi NN | Balanced&Un-normalized | 0.45 | 0.87 | 0.59 | 0.88 |

Based on the table above, random forest, XGBoost, and multi-layer neural networks performed the best with balanced and unbalanced train sets. However, an interesting phenomenon was models fitted on the balanced train set had lower precision but higher recall. Using a train set with balanced labels greatly improved the models' ability to identify all positive (Exited = 1) labels. This also showed that the imbalance inhibited the models' ability to identify positive labels as the majority label (Exited = 0) dominated the dataset. However, it should be noted that this improvement in the ability to identify positive labels did come at the cost of decreased accuracy in positive predictions (lower precision). In the real world, the choice might depend on which metrics (recall vs precision) the specific use case cares about. For this assignment, the choice was made to follow the industry standard practice and select the model fitted on the balanced train set. Hence, the multi-layer neural network fitted on the balanced and normalized train set was chosen since it had the highest F1 Score.

### Directory:

```bash
├── requirements.txt	#list of packages required to run main.py
├── EDA_preprocessing	#conducts EDA on train, splits data, preprocesses data, balances dataset
├── Logistic_Regression	#creates logistic regression models on unbalanced & balanced dataset
├── MNN_discret	#creates multi-layer neural network by discretizing & encoding features into bins
├── MNN_norm	#creates multi-layer neural network with normalized numerical features
├── MNN_norm_param		#param tuning for MNN using cross-validation
├── RandomForest	#creates random forest models on unbalanced & balanced data, conducts param tuning
├── XGBoost	#creates XGBoost models on unbalanced & balanced data, conducts param tuning
├── models
│   ├── Gender_encoder_balanced.joblib
│   ├── Gender_encoder_unbalanced.joblib
│   ├── Geography_encoder_balanced.joblib
│   ├── Geography_encoder_unbalanced.joblib
│   ├── HasCrCard_encoder_balanced.joblib
│   ├── HasCrCard_encoder_unbalanced.joblib
│   ├── IsActiveMember_encoder_balanced.joblib
│   ├── IsActiveMember_encoder_unbalanced.joblib
│   ├── MNN_best_unbalanced.keras
│   ├── logistic_balanced.joblib
│   ├── logistic_unbalanced.joblib
│   ├── mnn_discret_balanced.keras
│   ├── mnn_discret_unbalanced.keras
│   ├── mnn_norm_balanced.keras
│   ├── mnn_norm_unbalanced.keras
│   ├── rf_best_balanced.joblib
│   ├── rf_best_unbalanced.joblib
│   ├── scaler_balanced.joblib
│   ├── scaler_unbalanced.joblib
│   ├── xgb_best_balanced.joblib
│   └── xgb_best_unbalanced.joblib
└── README.md
	
```