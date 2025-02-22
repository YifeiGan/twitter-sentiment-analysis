# twitter-sentiment-analysis
HW4 Report

Yifei Gan

1 Part 1

1. Part A

Total data samples analyzed: 14,640

Unique Values and Most Frequent Categories: Airline Sentiment:

Unique values: Negative, Neutral, Positive Most frequent sentiment: Negative Frequency: 9,178

Negative Reason:

Unique values: 11 categories

Most frequent reason: Customer Service Issue Frequency: 2,912

Tweet Length Statistics:

Shortest tweet length: 5 characters

Longest tweet length: 150 characters

Tweet Length Distribution:

![](Aspose.Words.f2a78e1c-d73c-4f5d-952a-0188c60d2851.001.png)

Figure 1: Distribution of Tweet Length

2. Part B

Tweet sentiment distribution per airline:

![](Aspose.Words.f2a78e1c-d73c-4f5d-952a-0188c60d2851.002.png)

Figure 2: Distribution of Tweet Length

3. Part C & D

A custom tokenizer was implemented using Python’s re module with the following rules: Tokenize words, handling contractions (e.g., "didn’t" becomes ["did", "n’t"]).

Separate punctuation from words.

Identify and normalize URLs as ’URL’.

Replace mentions (username) with ’MENTION’ and hashtags (#topic) with ’HASHTAG’.

Replace numeric tokens with ’NUMBER’. Comparison with NLTK’s Tokenizer:

Example comparisons revealed differences in han- dling punctuation, URLs, mentions, and contrac- tions. The custom tokenizer demonstrated en- hanced performance for domain-specific tokeniza- tion.

2 Part 2

The data cleaning process involved several steps to ensure high-quality and relevant input for analysis and modeling. The choices made during this pro- cess were justified as follows:

1. Removal of Mentions

Method: Mentions such as @username were re- moved using a regular expression.

Justification: Mentions are not informative for sen- timent analysis as they typically indicate conversa-

tion direction rather than content or sentiment.

2. Removal of Currency Symbols and Amounts Method: A regex pattern identified and removed occurrences of monetary values (e.g., $20.00). Justification: Currency details are rarely relevant for the sentiment or the context being analyzed in tweets.
2. Removal of Email Addresses

Method: Email addresses were filtered out using a regex pattern.

Justification: Email addresses often act as metadata or identifiers that are not critical for sentiment anal- ysis.

4. Emoji and Special Character Cleaning

Method: Non-alphanumeric characters were stripped using regex patterns.

Justification: Emojis and special symbols can in- troduce noise into the analysis, and their sentiment contribution (if needed) can be handled separately.

5. HTML Escape Character Removal Method: HTML entities (e.g., &amp;) were identified and removed.

Justification: These characters are artifacts of web text and provide no semantic value in the analysis.

6. Punctuation Removal

Method: Regex patterns removed punctuation marks.

Justification: Punctuation is typically non- informative for machine learning tasks unless specifically tokenized as part of text structure (e.g., sentiment markers like !).

7. Removal of Dates and Times

Method: Regex identified date/time formats (e.g., 12:30 PM, 01/01/2020) and removed them. Justification: These are unlikely to influence senti- ment directly and can create unwanted variability in the input data.

8. Removal of URLs

Method: URLs were identified using regex and re- placed with an empty string.

Justification: URLs do not provide meaningful in- formation about the sentiment expressed in the tweet.

9. Lemmatization

Method: The WordNetLemmatizer from NLTK was applied to lemmatize all tokens.

Justification: Lemmatization reduces words to their base forms (e.g., running → run), helping normal- ize the text and improve consistency across inputs.

10. Deduplication

Method: Duplicate rows based on cleaned\_text and

airline\_sentiment columns were removed. Justification: Redundant entries can bias the anal- ysis or skew model predictions. Removing dupli- cates ensures a diverse and unbiased dataset.

11. Removal of Empty Rows

Method: Rows with empty or whitespace-only cleaned\_text values were discarded.

Justification: Empty rows provide no information and can interfere with the learning process.

12. Lowercase Conversion

Method: Convert all text to lowercase. Justification: Standardizing text to lowercase en- suresthatwordslike"Good"and"good"aretreated the same, improving consistency in text analysis.

13. Removal of Stop Words

Action: Remove common stop words such as "the," "is," "and," etc., which don’t contribute much to sentiment analysis.

Justification: Removing stop words reduces the noise in the dataset and helps focus on meaningful tokens.

3 Part 3

The ablation study evaluated the impact of individ- ual data cleaning actions and their combinations on the performance of the classifier (SGDClassifier with hinge loss). The study was conducted using cross-validation accuracy on the training data. Key observations are summarized below:

Actions Tested: Lowercasing (lowercase) Punctuation Removal (remove\_punctuation)

Stop Word Removal (remove\_stopwords) Lemmatization (lemmatize)

Combinations of these actions.

Results: Baseline (No Cleaning Actions): Mean CV Accuracy: 0.7950

Lowercase Only: Improved accuracy slightly to 0.7953, suggesting a minor benefit.

Lowercase + Punctuation Removal: Mean CV Ac- curacy peaked at 0.7957, indicating a modest but consistent improvement.

Lowercase + Punctuation Removal + Stop Word Removal: Accuracy dropped to 0.7786, likely due to the loss of context carried by certain stop words. Lemmatization: Alone, lemmatization yielded the best improvement, with a mean CV accuracy of 0.7986. This suggests that reducing words to their base forms improved consistency in feature repre- sentation. Final Evaluation on the Test Set

Best Cleaning Actions:

The final classifier was trained using the combina-

tionoflowercase, punctuationremoval, andlemma- tization, as this combination showed the most bal- anced performance in the ablation study. Performance Metrics:



|Class|Precision|Recall|F1-Score|Support|
| - | - | - | - | - |
|Negative Neutral|0\.79 0.69|0\.94 0.40|0\.86 0.51|892 311|
|Positive|0\.78|0\.66|0\.72|208|
|Accuracy Macro Avg|0\.75|0\.78 0.67|0\.69|1411 1411|
|Weighted Avg|0\.77|0\.78|0\.76|1411|

Table 1: Classification Report

 

837 35 20

Confusion Matrix = 166 125 20 

50 20 138

Lowercasing and punctuation removal provided slight improvements in performance, likely due to improved feature uniformity. The removal of stop words was detrimental, as stop words may pro- vide critical contextual cues for sentiment analysis. Lemmatization was the most impactful individual action, probably because it reduces the sparsity in the feature space.

Although combining multiple actions improved some metrics, excessive cleaning (e.g., removing stop words) reduced accuracy, suggesting that over- cleaning can strip the data of valuable context.

All cleaning actions were assumed to contribute equally to the classifier’s performance. The ab- lation study revealed that this is not always true; some actions may have neutral or negative effects. It was assumed that text cleaning would equally benefit all sentiment classes, but results show un- even performance gains, with "Neutral" sentiment often harder to classify. TF-IDF was assumed to be a suitable feature representation for this dataset. Al- ternative methods (e.g., word embeddings) might yield different outcomes.
