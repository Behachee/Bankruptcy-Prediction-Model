# Foundation of Machine Learning
# Final Project Report
AUFFRET Julien - BELEN-HALIMI Théo - PANIJEL Joseph - SHAH Yash 
## ABSTRACT
Bankruptcy prediction, a key topic in finance and management science, has evolved from initial financial statement analysis to utilizing machine learning and deep learning algorithms, driven by advancements in information technology.
This project investigates the predictive capacity of a comprehensive banking dataset for assessing the likelihood of bankruptcy in companies. The data were collected from the Taiwan Economic Journal for the years 1999 to 2009. Company bankruptcy was defined based on the business regulations of the Taiwan Stock Exchange. The dataset includes financial ratios, growth rates, liquidity and solvency indicators, profitability and efficiency metrics, cash flow and working capital measures, and various coverage ratios. Utilizing advanced machine learning techniques, we aim to develop a robust bankruptcy prediction model.
Key features include different variants of Return on Assets (ROA), operating margins, debt-related ratios, growth rates in sales and profits, liquidity and solvency indicators, and coverage ratios. The dataset also has flags for specific financial conditions. Our approach involves extensive data preprocessing, handling outliers, and scaling to ensure the reliability of the model. By considering both financial and non-financial factors, such as industry type and economic indicators, our model seeks to capture the complex interplay of variables influencing bankruptcy risk.
Ultimately, the developed model aims to provide a tool to assess and mitigate the risks associated with bankruptcy, helping in more informed decision-making.

## Introduction/Motivation							
The primary focus of our project is addressing the challenge of predicting bankruptcy in businesses and legal entities. Bankruptcy, a legal process triggered by an entity's inability to meet its financial obligations, allows for debt repayment through the liquidation of assets. The early detection allows for the benefit of creditors, investors, and overall financial stability.
First, it enables stakeholders to take corrective measures and allocate capital efficiently. This predictive capability is integral to risk assessment, financial planning, and investment decision-making. Robust models not only assist creditors in mitigating risks but also empower investors to make well-informed decisions. Furthermore, these models contribute to a deeper understanding of a company's financial health, facilitating timely interventions when needed.
Our project seeks to leverage insights from prior research in financial forecasting and machine learning to develop accurate and reliable models for bankruptcy prediction. By doing so, we aim to provide valuable tools that enhance risk management, financial planning, and decision-making processes in the dynamic landscape of business and finance.
Problem Definition
Our problem statement is the following : given financial data on companies, we aim to predict if this company is bankrupt or not. Building such a model can allow, in the future, to identify companies with similar financial characteristics that have not yet declared bankruptcy (but hence, would be on the verge of it). We view the impact of falsely predicting a company as non-bankrupt as being more prejudicial than the other way around, and therefore put an emphasis on minimizing the number of false negatives (companies that are bankrupt but predicted as non-bankrupt). 
We reviewed several traditional machine learning algorithms to classify companies as bankrupt. The first one was Logistic regression is a linear model that classifies the data points into 0 (not bankrupt), and 1 (bankrupt) using a sigmoid function f(x) = 1/(1+exp(-x)) . We set a threshold (0.5 by default) depending on the output that classifies the points into 0 or 1.
We also tried two ensemble learning methods : Random Forest and LightGBM. Ensemble learning can be classified into two methods : bagging (Random Forest) and boosting (LightGBM). A Bagging algorithm creates random subsets of data randomly, with the same size of the original training dataset (which can create duplicates), a process known as bootstrap. The idea is to train individual models that contain different parts of the data, which will help reduce overfitting, and then combine the outputs of each model to classify using majority voting. Random Forest uses multiple Decision Tree algorithms, which splits the data on certain features based on a criterion (such as Gini impurity). Boosting on the other hand, combines multiple weak learners sequentially (models that perform slightly better than random prediction), and improves these models by giving more weights to data points that were misclassified. 
We try these three types of classification models, which allows us to select an algorithm that has high predictive power. Our approach is to estimate the performance of these different prediction models according to several metrics. Firstly, our dataset is highly unbalanced : out of 6819 companies, only 220 are classified as bankrupt (3.23%). Meaning that we would obtain an accuracy (number of correct predictions over total number of data points) of over 96% if we classified everything as 0. Hence, it reinforces our focus of minimizing our false negative rate. To do that, we look to optimize the probability threshold that a certain point is 1 to actually be classified as 1. This threshold comes at a tradeoff between the number of false positives and false negatives : lowering the threshold for which a data point is classified as 1 will increase the number of false positives and lower the number of false negatives. Hence, the Area under the ROC curve is a useful metric in this case, as it gives us a complete understanding on how our model is predicting on both classes, allows us to summarize the performance of our model over different predictive thresholds, and to compare models versus one another.
Related Work
With the emergence of new techniques in data science, particularly machine learning and deep learning, research into bankruptcy prediction is still very much alive. Long a subject of accounting and finance, Altman E I. in “Financial ratios, discriminant analysis and the prediction of corporate bankruptcy” in 1968, it has become, in recent years, an important research topic, as new techniques could meet the challenges, previously mentioned, posed by the complexity of this prediction.
This field of research dates back nearly 50 years, beginning with discriminant analysis and logistic regression as pioneering methods in bankruptcy prediction, as highlighted by Ohlson J.A. in 1980 with "Financial ratios and the probabilistic prediction of bankruptcy." Since the 1990s, the application of machine learning models like decision trees, neural networks, and Support Vector Machines has become prevalent in predicting firm bankruptcies, a trend further explored by Lin W.Y., Hu Y.H., and Tsai C.F. in their 2011 work, "Machine learning in financial crisis prediction." More recently, deep learning has gained prominence, offering advanced capabilities across various applications. The initial study of bankruptcy prediction using financial statement data was conducted by Beaver in 1966, primarily focusing on comparing financial ratios against set thresholds. This approach, though straightforward, as described by Atiya A.F. in 2001's "Bankruptcy prediction for credit risk using neural networks: A survey and new results," laid the groundwork for the more complex training processes in machine and deep learning. These techniques, through dataset training, yield classifiers with high accuracy, forming the core principle of modern bankruptcy prediction methods.
As the field evolves with fast advancements in techniques and the availability of diverse datasets, a significant aspect of current research is the application of machine and deep learning methods for more refined bankruptcy prediction. Recent studies, such as "Corporate finance risk prediction based on LightGBM" by Di-ni Wang, Lang Li, and Da Zhao in 2022, and "Benchmarking Machine Learning Models to Predict Corporate Bankruptcy" by Alanis, Emmanuel; Chava, Sudheer; and Shah, Agam in 2022, exemplify the innovative use of sophisticated algorithms to enhance predictive accuracy. These methods are not just limited to traditional data sources but also extend to more complex and varied datasets. An important development in this area is the handling of unbalanced data, which has been a challenge in predictive modeling. The study "Bankruptcy prediction on the base of the unbalanced data using multi-objective selection of classifiers" by Yuri Zelenkov and Nikita Volodarskiy in 2021 addresses this issue, demonstrating the potential of using multi-objective selection techniques to improve model performance on imbalanced datasets. This focus on fast evolving technologies and multiple datasets.
Our report is influenced by and supports the findings of Deron Liang, & Al 2016 study, "Financial Ratios and Corporate Governance Indicators in Bankruptcy Prediction: A Comprehensive Study." We focus on corroborating their research on applying machine learning to financial ratios data for bankruptcy prediction. This approach underscores the effectiveness of combining traditional financial analysis with advanced machine learning techniques in risk assessment.

## Methodology
The data that was collected was fairly straightforward to process. We had a dataset of 96 columns (including our target variable ‘bankrupt’) with 6819 data entries, which were all numerical and without null values. As the ensemble models which we used were robust to outliers, the only step used in preprocessing was standardizing the data. We split our dataset into a training set (70% of our data) and a validation set (30% of our data). Then, we decided to test 3 baseline models, Logistic Regression, LightGBM and Random Forest. For each of these models, we plotted a learning curve to ensure that our models did not suffer from overfitting, (meaning that as our training set got larger, we ensured that the increase in training accuracy did not impact the test accuracy negatively). For Logistic Regression, the learning curve indicated that the training and validation accuracy were increasing in a similar fashion until the training set reached a size of about 30% of the entire dataset (4800 data points). For Random Forest and LightGBM, the training accuracy remained constant at 100% accuracy (which is usual with ensemble models), while the validation accuracy increased as the training set grew larger (See Annex 1, 2 and 3). The learning curves therefore excluded the risk of overfitting. We pursued our analysis of the performance of these 3 models by plotting the confusion matrix, the ROC curve and an AUC score to be able to compare the models and understand their performances on both classes. What stood out from the confusion matrices was the low performance on class 1 (meaning companies that are bankrupt). This comes from the fact that our dataset was highly imbalanced, with only 220 companies classified as 1 (3.22% of our data). Therefore, after running these baseline models, we tried to improve their performances with feature engineering.
Seeing as we had a large number of features, we experienced dimensionality reduction techniques, namely Principal Component Analysis and Singular Value Decomposition. The primary goal of a method such as PCA is to transform a high-dimensional dataset into a lower-dimensional space while retaining as much of the original variability as possible. It achieves this by identifying the principal components, which are orthogonal axes along which the data varies the most. These components are linear combinations of the original features. By projecting the data onto these components, PCA allows for a simplified representation that captures the essential patterns and relationships within the data. Using PCA, we managed to keep 99% of our variance and reduce our number of features to 64 components (see Annex 4). Singular Value Decomposition led to the same results. To deal with the imbalance in the dataset, we used a Synthetic Minority Oversampling Technique (SMOTE) on our training set. SMOTE is an algorithm which consists in creating artificial data points to rebalance the minority class and have an equal number of data points in each class. For each example in the minority class, SMOTE creates synthetic examples by interpolating between that example and its nearest neighbors. It selects a minority class example and its k nearest neighbors. It then generates synthetic examples along the line segments connecting the selected example to its neighbors.
Once we had reduced the dimensionality of our data and rebalanced our training set, we refitted our models and used the same metrics (confusion matrix, ROC curve, AUC score) to understand how the performance on the validation set is impacted. Finally, we selected our best predictive model, based on the AUC score and the confusion matrix, (in this case LightGBM), and performed hyperparameter tuning. 
Once we had selected a model based on the best AUC scores, we needed to train the model with better hyper parameters to produce results that could have higher True positives with a considerable amount of False positives. To incorporate this into the Grid search we experimented with various custom scoring methods. To list a few of the candidates, 
Finding the best score = TPR - 0.1*FPR for all thresholds.
Finding the best AUC.
Average of TPR for all thresholds in range 0.5 to 0.3.
Out of all this we used the simplest one i.e. finding the best set of hyperparameters for the model that would produce a model with the best AUC for LightGBM. The list of hyperparameters we experimented with include: max_depth, num_leaves, pos_weight_scale, n_estimators, min_child_samples, subsample, reg_alpha, and reg_lambda.

## Evaluation: How did you evaluate your work? What experiments did you run? Describe clearly your findings.
To evaluate the models we looked for many different metrics, but the main challenge 

## Conclusion What are the conclusions of your work? Are there any highlights? What are some ideas for future work?
Although the development of machine learning techniques has made it possible to predict company failures more accurately for all the stakeholders concerned, it should be remembered that expertise - particularly human expertise - is obviously required for a more in-depth analysis that takes into account the results of the prediction in the economic context of the situation. Indeed, the geopolitical, financial, social or environmental context, and more generally the future uncertainty inherent in human decisions, could have a certain influence on the interpretation of the results, which should not be overlooked.
Finally, a possible improvement to this system could be, as suggested by Radovanovic & Haas, 2023, the study of the socio-economic cost in this prediction. Therefore, financial statistics combined with data on the socio-economic context of the environment across different countries from different continents would be promising.
References
Alanis, E., Chava, S., & Shah, A. (2022). Benchmarking machine learning models to predict corporate bankruptcy. Social Science Research Network. https://doi.org/10.2139/ssrn.4249412
Liang, D., Lu, C. C., Tsai, C., & Shih, G. A. (2016). Financial Ratios and Corporate Governance Indicators in Bankruptcy Prediction : A Comprehensive study. European Journal of Operational Research, 252(2), 561‑572. https://doi.org/10.1016/j.ejor.2016.01.012
Qu, Y., Quan, P., Lei, M., & Shi, Y. (2019). Review of bankruptcy prediction using machine learning and deep learning techniques. Procedia Computer Science, 162, 895‑899. https://doi.org/10.1016/j.procs.2019.12.065
Wang, D., Li, L., & Da, Z. (2022). Corporate Finance Risk Prediction based on LightGBM. Information Sciences, 602, 259‑268. https://doi.org/10.1016/j.ins.2022.04.058
Radovanovic, J., & Haas, C. (2023). The evaluation of bankruptcy prediction models based on socio-economic costs. Expert Systems with Applications, 227, 120275. https://doi.org/10.1016/j.eswa.2023.120275


## ANNEX

Annex 1: Learning Curve for baseline model Logistic Regression
<img width="434" alt="Sans titre" src="https://github.com/Behachee/Bankruptcy-Prediction-Model/assets/140748662/27997a5e-57e8-4c23-9340-71f1f6d99fd2">

Annex 2: Learning Curve for baseline model LightGBM
<img width="459" alt="Sans titre2" src="https://github.com/Behachee/Bankruptcy-Prediction-Model/assets/140748662/603a8167-44a3-47f3-bdb3-baa0926fe35f">

Annex 3: Learning Curve for baseline model LightGBM
<img width="468" alt="Sans titre3" src="https://github.com/Behachee/Bankruptcy-Prediction-Model/assets/140748662/54aa6f36-904b-45dc-aa92-a2784e0de926">

Annex 4: 
<img width="362" alt="Sans titre4" src="https://github.com/Behachee/Bankruptcy-Prediction-Model/assets/140748662/37fd1e4f-23db-4a7a-8ab7-3839fbccd197">



