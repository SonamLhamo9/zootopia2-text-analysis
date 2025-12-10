Project Zootopia 2 IMDb Review Text Analysis
1. Overview
This project analyzes 220 IMDb user reviews for Zootopia 2.
The goal is to understand:
How viewers feel about the movie
What topics they talk about
How they react to different characters
I used Python to collect the reviews, clean the text, run sentiment analysis, build topic models, and study character-related emotions.

2. Data Collection
The reviews were collected using the IMDb GraphQL API.
The code below shows the function used to download one page of review data:
def fetch_page(title_id, after=None):
    r = requests.post(url, json=payload, headers=HEADERS)
    return r.json()
I then looped through all pages to collect every review:
df = fetch_all_reviews("tt26443597")
Final dataset size: 220 reviews
Each row includes:
review_id, username, rating, date, summary, text, helpful_up, helpful_down

3. Sentiment Analysis
I used TextBlob and VADER to measure emotions in each review.
df[['blob_sent', 'vader_sent']] = df['text'].apply(
    lambda x: pd.Series(compute_sentiments(x))
)
Key Findings
Most reviews are strongly positive
VADER scores are usually higher than TextBlob
Ratings and sentiment have a weak positive correlation
Positive reviews often use words like amazing, great, emotional, beautiful
Example Plot
Sentiment distribution (VADER):
plt.hist(df['vader_sent'], bins=20)

4. Topic Modeling
I used LDA (Latent Dirichlet Allocation) to find common themes.
lda_model = models.LdaModel(corpus=corpus, id2word=dictionary, num_topics=5)
Main Topics (Simple Summary)
Topic 0: General comments about the movie
Topic 1: Story and production quality
Topic 4: Personal emotional reactions
Topics 2 & 3: Small, less meaningful topics (due to simple text cleaning)
This shows that most reviews focus on animation, story, and character chemistry.

5. Character-Level Sentiment
I searched for sentences mentioning key characters (Judy, Nick, Gazelle, Snake, Bogo).
Then I calculated sentiment for each sentence.
char_summary = char_df.groupby("character")["sentiment"].mean()
Character Insights
Gazelle → Most loved character (very positive sentiment)
Nick & Judy → Positive and stable reactions
Snake → Mixed reactions (some like him, some dislike him)
Chief Bogo → Only character with negative average sentiment
This shows that viewers respond very differently to each character.

6. Repository Structure
/project_zootopia2.qmd # Full analysis with chunk options
/zootopia2_reviews_final.csv # Final cleaned dataset
/README.md # Project introduction and explanation

7. Conclusion
This project shows that Zootopia 2 received very positive feedback from IMDb users.
The sentiment analysis, topic modeling, and character-level study all help us understand:
What viewers liked most
What themes they discussed
Which characters created the strongest emotional reactions
Overall, the movie was well received, especially for its animation, humor, emotional tone, and main characters.# zootopia2-text-analysis
