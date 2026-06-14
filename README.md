# ipl-sentiment-pipeline
# IPL Sentiment Analysis Pipeline

## 1. Project Overview
The goal of this project is to build an end-to-end Machine Learning pipeline that can accurately read and classify the sentiment of social media posts (tweets) regarding the Indian Premier League (IPL).

## 2. Task
The core task is an **entity-aware sentiment classification**. The model must take a specific IPL subject (e.g., a team or player) and a text statement, and classify the underlying emotion into one of three categories: `positive`, `negative`, or `neutral`.

## 3. Dataset
The project utilized two datasets:
* **80-Row Dataset (`ipl_sentiment_small_dataset.csv`):** Used for initial model training, baseline testing, and hyperparameter tuning.
* **300-Row Dataset (`ipl_300R_indian_english_sentiment_dataset.csv`):** Used as a final testing set to ensure the models can generalize to larger amounts of unseen, noisy text.
* **Input Format:** The text was engineered into the format `subject_keyword + ' [SEP] ' + statement` to ensure the model understood which entity the sentiment was directed toward.

## 4. Approach
## 4. Approach
To find the most efficient and accurate model, I built a custom grid-search experiment testing a diverse "Dream Team" of 4 Vectorizers and 4 Classifiers. Every vectorizer was paired with every classifier and scored based on a combination of Macro F1 accuracy, training speed, and model size.

### The Vectorizers (Text Preparation)
1. **CountVectorizer (Clean):** Selected as a highly efficient frequency baseline. It simply counts word occurrences, which is great for catching common cricket terminology.
2. **Word TF-IDF (1-2 grams):** Selected to balance context with speed. By looking at unigrams and bigrams, it catches short, meaningful phrases (like "bad bowling") and weighs them by importance rather than just frequency.
3. **Character TF-IDF (3-5 grams):** Selected as the "typo-catcher." IPL fan slang is incredibly messy. Character-level parsing catches misspelled words, abbreviations, and Hinglish slang much better than strict word-level parsing.
4. **HashingVectorizer (Ultra Light):** Selected for maximum scalability. It does not store a vocabulary dictionary in memory, giving it a near-zero memory footprint for large text datasets.

### The Classifiers (Predictors)
1. **Multinomial Naive Bayes (Smooth):** Selected as a probabilistic baseline. It is lightning fast and highly sensitive to rare emotional words, making it a classic choice for sentiment text.
2. **Logistic Regression (Tuned):** Selected as a linear baseline. It was tuned with balanced class weights to prevent overfitting, making it highly reliable for sparse text data.
3. **SGD Classifier (SVM):** Selected for margin-based learning. It acts as a linear Support Vector Machine, which is mathematically proven to handle high-dimensional text data (like our TF-IDF outputs) exceptionally well.
4. **Random Forest (Shallow):** Selected to represent tree-based models. It was strictly capped at a shallow depth (8) so it could find non-linear patterns in the text without eating up massive amounts of memory.

## 5. How to Run
1. Download the `completed_notebook.ipynb` file and open it in Google Colab.
2. Upload the `ipl_sentiment_small_dataset.csv` and `ipl_300R_indian_english_sentiment_dataset.csv` files to the Colab session storage.
3. Click **Runtime > Run all** in the top menu.
4. The notebook will automatically generate the pipelines, train the models, and output a ranked scoreboard.

## 6. Conclusion
Based on the final 300-row experiment, the highest-ranking pipeline was the combination of the **HashingVectorizer** and **Multinomial Naive Bayes**. While other models achieved slightly different accuracies, this specific combination won the overall performance score because it perfectly balanced decent text prediction with a microscopic memory footprint and lightning-fast training speed, making it highly scalable for real-world social media feeds.
