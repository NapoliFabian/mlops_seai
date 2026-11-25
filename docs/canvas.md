# MACHINE LEARNING CANVAS

**Designed For:** News Category Classification
**Designed By:** Napoli Fabian  
**Date:** 25/11/2026  
**Iteration:** 1

**Prediction Task**

The objective of this project is to address a two-level hierarchical multi-class classification problem based on the automatic categorization of news content. The model is designed to analyze individual news articles and assign each of them to a category and a corresponding subcategory, based on the topics and subjects discussed in the article.

The entities considered in this study are the news articles, defined as textual objects that contain information about events, facts, or topics. Each news article represents a single instance to be classified by the model: from a predictive perspective, the model processes the textual content of each article and determines which categories and subcategories best describe its content.

The possible outcomes consist of a predefined set of categories at the first classification level, each associated with a set of more specific subcategories at the second level. Both levels involve multiple possible classes and the model estimates the probability of each assigning the news article to the most likely category and its corresponding subcategory.

Outcomes are observed at inference time, once the trained model is integrated into a news feed or streaming pipeline. As new articles become available, the system automatically processes their textual content and predicts their category and subcategory. These predictions can then be used to organize news content, facilitate topic-based filtering, and improve the overall news browsing experience for end-users.

**Decisions**

The system converts model predictions into actionable recommendations by providing both a hierarchical classification label and associated confidence scores. For each news article, the model give in output the predicted category and its corresponding subcategory, together with probability values that quantify the model's confidence in each classification.

These confidence scores enable a more informed interpretation of the predictions. High-confidence classifications indicate that the model is strongly confident about the assigned category and subcategory, while lower scores suggest uncertainty and may require additional verification or manual classification.

A confidence threshold can be introduced to determine whether a prediction is reliable enough to be automatically accepted: if the highest predicted probability falls below this predefined threshold, the article can be flagged for manual review. Furthermore, the system can provide the top-k most probable categories or subcategories, allowing end-users to explore alternative classifications when the prediction is uncertain.


**Value Proposition**

The main end beneficiaries of this solution are news organizations, publishers, independent journalists, and end-users who reads news through digital platforms.

The primary objective is to address a common problem in news management: a single news source, such as a newspaper or an independent journalist, can publish articles covering a wide variety of topics and categories: manually assigning categories and subcategories to each article can be time-consuming, costly, and potentially prone to human errors and inconsistencies. Alternatively, developing and maintaining separate classification rules or algorithms for different topics can introduce additional complexity.

The proposed ML solution addresses this problem by automatically processing news articles and assigning them to predefined categories and corresponding subcategories. This reduces the need for manual classification, improves consistency, and allows news providers to manage large volumes of content more efficiently.

The solution can be integrated into the existing workflow of news platforms through an automated classification pipeline or API. Whenever a new article is published or received it's textual content is submitted to the model which returns the predicted category, subcategory and associated confidence scores.

From the end-user perspective, these predictions can be made available through the platform's graphical user interface (GUI). Users can browse news organized by topic, filter articles according to their interests, and access more structured and enriched information. For news providers, the system offers an automated tool to organize and manage their editorial content with reduced manual effort.



**Data Collection**

The initial dataset used in this project is the MN-DS (Multilabeled News Dataset), described in the research paper *"MN-DS: A Multilabeled News Dataset for News Articles Hierarchical Classification"* by Petukhova and Fachada (2023).

The dataset is publicly available in CSV format through the Zenodo repository and contains 10,917 news articles collected between January and December 2019 from 215 different media sources. Each article includes information such as its title, textual content, publication date, source, author, and URL.

The outcomes are already provided through manually assigned labels, following the IPTC NewsCodes Media Topic taxonomy.The dataset contains 17 first-level categories and 109 second-level subcategories. Each article is assigned to a specific second-level category, which is also associated with its corresponding first-level category. This manual annotation process provides the ground-truth labels required for supervised learning.

For the update of data, we could retrive it from the integrated pipeline from applciations that use the model,the system can be extended through automated data insertion mechanisms using news API, RSS feeds or web scraping. 

To maintain data quality and control operational costs, a subset of newly collected articles could be periodically selected for manual verification and annotation. These verified examples can subsequently be incorporated into the training dataset through scheduled model updates .

**Data Sources**

The primary external data source used in this project is the MN-DS (Multilabeled News Dataset), publicly available through the Zenodo repository in CSV format.

The dataset provides both the entities and their corresponding observed outcomes required for supervised learning. Specifically, each news article is represented by a set of attributes stored in the dataset, including:
- `id`: Unique identifier of the news article.
- `title` and `content`: Textual information used as input features by the model.
- `date` and `published_utc`: Publication timestamps.
- `source`, `author`, and `url`: Information about the original publisher and article.
- `category_level_1` and `category_level_2`: Ground-truth labels representing the two hierarchical classification levels.

The classification taxonomy is based on the IPTC NewsCodes Media Topic standard, which provides the predefined categories and subcategories used to annotate the articles.

For the current project, the MN-DS CSV files represent the main source and no internal database was used for the initial model training.


**Impact Simulation**

A correct prediction of a news article's category and subcategory allows the article to be displayed in the most appropriate section of a news platform. This improves content organization and enables users to filter news according to their interests, discover relevant articles more easily, and access a more personalized browsing experience. For news providers, accurate classification also reduces the need for manual content categorization.

An incorrect prediction can negatively affect the user experience: an article may be assigned to an inappropriate category, making it difficult for interested users to find it through topic-based filters. This can reduce content visibility, compromise the effectiveness of recommendation systems, and potentially lead to missed relevant information.

Considering the hierarchical structure and the different number of available classes, we expect the model to achieve higher classification performance at the first level, where there are fewer and lartger categories. Good performance is also expected at the second level, although the greater number and variety of subcategories make this classification task more challenging.

To simulate the pre-deployment impact, the MN-DS dataset will be divided into training and testing sets: 70% of the articles from each subcategory will be used for training, while the remaining 30% will be reserved for testing. This stratified splitting strategy ensures that the distribution of the different subcategories is represented in both sets.
The model's performance will be evaluated separately at both classification levels using metrics such as accuracy, precision, recall, and F1-score, with particular attention to the performance across individual categories and subcategories.

Deployment will be considered when the model demonstrates satisfactory classification performance, particularly in terms of macro F1-score and recall across the different classes. We will also evaluate the consistency of the hierarchical predictions, ensuring that each predicted subcategory belongs to the corresponding predicted main category.

Regarding fairness constraints, the model operates on news articles rather than directly on individual users. Therefore, the risk of direct discrimination is relatively limited, as its primary purpose is to categorize news content based on the topics discussed, without making decisions about individuals.

However, news articles may contain references to groups characterized by protected attributes, such as gender, ethnicity, age, or religion. Therefore, a preliminary analysis of the dataset can be conducted to identify potential biases in the content and ensure that the model does not systematically produce discriminatory or unfair classifications.

**Making Predictions**

For individual end-users, predictions are performed in real time when an user accesses or submits a news article through the system's web interface or API: the system immediately processes its textual content, extracts the features, and returns the results consisting of a predicted category and its corresponding subcategory, together with associated confidence scores. This interaction requires low latency to ensure a smooth user experience. The end-to-end processing time, including text preprocessing, feature extraction, and model inference, is expected to remain within a few seconds.

On the other hand, platform-level integrations, such as news publishers or news aggregation services, can perform predictions in batch mode. In this setting, large volumes of articles are periodically processed offline, allowing news providers to automatically organize and categorize their content before making it available to users.

Inference will primarily be performed on CPUs, as prediction generally requires fewer computational resources than model training. This allows for lightweight, scalable deployment on standard servers without requiring dedicated GPU resources. The actual computational requirements will depend on the selected model architecture and the volume of incoming news articles.

**Building Models**

In production, the system can adopt an ensamble architecture consisting of multiple models. A possible approach is to use a primary classifier responsible for identifying the main category of each news article, followed by a specialized classifier that determine the corresponding subcategory within the predicted category.

Alternatively, multiple specialized models can be combined through other techniques, such as majority voting, to improve classification robustness and reduce the impact of individual model errors. The choice between a single hierarchical model and a multi-model architecture will depend on the classification performance, computational requirements, and maintenance costs.

Model updates should be scheduled periodically to account for changes in news content, emerging topics, and the introduction of new terminology, particularly within more specific subcategories. New training data can be collected continuously, while model retraining can be performed at regular intervals or triggered when monitoring reveals a significant degradation in classification performance or changes in the data distribution.

Training time will depend on the selected architecture, dataset size, and number of specialized classifiers.

GPU will primarily be used during model training to accelerate the training phase, particularly when using deep learning architectures such as Transformer-based models. For lighter architectures, such as TF-IDF combined with Logistic Regression, CPU-based training may also be sufficient. Once trained, the models can be deployed for CPU-based inference to reduce operational costs.

**Features**

The main features used for prediction are extracted from the textual content of each news article, primarily including its title and body. These textual fields represent the main input features used by the classification models.

Different text representation techniques can be adopted depending on the selected model architecture.

For traditional machine learning approaches, such as TF-IDF combined with Logistic Regression, the text undergoes preprocessing operations, including normalization, removal of unnecessary special characters and punctuation, and optional stop-word removal. The resulting text is then transformed into numerical TF-IDF vectors, which represent the importance of individual terms within each document relative to the entire dataset.

For Transformer-based architectures, such as BERT, the text is processed using the model-specific tokenizer: this operation converts the input into tokens and generates the corresponding input representations, including token IDs and attention masks. The Transformer then produces contextualized representations that capture semantic and contextual relationships between words.


**Monitoring** 

System performance is monitored using standard classification metrics, including Accuracy, Precision, Recall, and F1-score. Particular attention is given to the Macro F1-score to account for class imbalance and ensure that less frequent categories and subcategories are adequately represented in the evaluation. Since the task involves two hierarchical classification levels, performance is evaluated separately for both the main categories and their corresponding subcategories. Additional monitoring includes the hierarchical consistency of predictions, ensuring that each predicted subcategory belongs to the assigned main category.

Monitoring is performed continuously at an operational level, tracking inference latency, processing throughput, and prediction confidence. Model performance is periodically reassessed using newly verified and labeled data, particularly after dataset updates or model retraining, to detect potential performance degradation and data drift.

From an end-user perspective, the main KPI is the correct assignment rate, which measures how accurately news articles are organized into their respective categories and subcategories.

For news providers, operational indicators include the reduction in manual classification effort, the volume of automatically processed articles, the time required to categorize newly published content, and the correct assignment rate.