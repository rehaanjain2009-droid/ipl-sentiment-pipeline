# Model Selection Explanations

## The Vectorizers (Text Preparation)
1. **CountVectorizer (Clean):** Selected as a highly efficient frequency baseline. It simply counts word occurrences, which is great for catching common cricket terminology.
2. **Word TF-IDF (1-2 grams):** Selected to balance context with speed. By looking at unigrams and bigrams, it catches short, meaningful phrases (like "bad bowling") and weighs them by importance rather than just frequency.
3. **Character TF-IDF (3-5 grams):** Selected as the "typo-catcher." IPL fan slang is incredibly messy. Character-level parsing catches misspelled words, abbreviations, and Hinglish slang much better than strict word-level parsing.
4. **HashingVectorizer (Ultra Light):** Selected for maximum scalability. It does not store a vocabulary dictionary in memory, giving it a near-zero memory footprint for large text datasets.

## The Classifiers (Predictors)
1. **Multinomial Naive Bayes (Smooth):** Selected as a probabilistic baseline. It is lightning fast and highly sensitive to rare emotional words, making it a classic choice for sentiment text.
2. **Logistic Regression (Tuned):** Selected as a linear baseline. It was tuned with balanced class weights to prevent overfitting, making it highly reliable for sparse text data.
3. **SGD Classifier (SVM):** Selected for margin-based learning. It acts as a linear Support Vector Machine, which is mathematically proven to handle high-dimensional text data (like our TF-IDF outputs) exceptionally well.
4. **Random Forest (Shallow):** Selected to represent tree-based models. It was strictly capped at a shallow depth (8) so it could find non-linear patterns in the text without eating up massive amounts of memory.
