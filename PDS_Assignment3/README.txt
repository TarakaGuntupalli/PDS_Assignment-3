1)Insert the given Dataset intlo the file.
data = pd.read_csv('Corona_NLP_test.csv')

2) Preprocess the given data.

# Preprocessing
data['OriginalTweet'] = data['OriginalTweet'].str.replace(r"http\S+", "")
data['OriginalTweet'] = data['OriginalTweet'].str.replace(r"@\S+", "")
data['OriginalTweet'] = data['OriginalTweet'].str.replace(r"[^a-zA-Z0-9\s]", "") # Updated regex
data['OriginalTweet'] = data['OriginalTweet'].str.lower()
data = data[data['OriginalTweet'].notna() & data['OriginalTweet'] != ""]
print(data)

3)Save the cleaned data to a new CSV File.
data.to_csv("Cleaned_Corona_NLP_test.csv", index=False)
data = pd.read_csv("Cleaned_Corona_NLP_test.csv")

4)a) Converting the text corpus into tokens. 

from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
import matplotlib.pyplot as plt
import pandas as pd
data = pd.read_csv('Corona_NLP_test.csv')
data['tokens'] = data['OriginalTweet'].apply(word_tokenize)
print(data['tokens']) 

b)Perform stop word removal.

nltk.download('stopwords')
stop_words = set(stopwords.words('english'))
data['tokens_no_stopwords'] = data['tokens'].apply(lambda x: [word for word in x if word not in stop_words])
print(data['tokens_no_stopwords']) 

c)Count Word frequencies

from collections import Counter
​
# Concatenate all the tweets into a single string
text = " ".join(data["OriginalTweet"].values)
​
# Split the string into words
words = text.split()
​
# Count the word frequencies using Counter
word_freq = Counter(words)
​
# Print the top 10 most frequent words
for word, freq in word_freq.most_common(10):
    print(word, freq)

d)Create word clouds.

from PIL import Image
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
​
​
# Load the dataset
data = pd.read_csv('Corona_NLP_test.csv')
​
# Concatenate all the tweets into a single string
text = " ".join(tweet for tweet in data['OriginalTweet'])
​
# Create a set of stopwords
stopwords = set(STOPWORDS)
stopwords.update(["co", "https", "amp", "coronavirus", "COVID", "19", "Covid", "covid"])
​
# Create a word cloud
wordcloud = WordCloud(stopwords=stopwords, background_color="white").generate(text)
​
# Display the word cloud
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()