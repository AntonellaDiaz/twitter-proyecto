# twitter-proyecto

from config import *
import tweepy
import datetime
import pandas as pd

auth = tweepy.OAuthHandler(TWITTER_CONSUMER_KEY, TWITTER_CONSUMER_SECRET)
auth.set_access_token(TWITTER_ACCESS_TOKEN, TWITTER_ACCESS_TOKEN_SECRET)
api = tweepy.API(auth,wait_on_rate_limit=True)

today = datetime.date.today()
yesterday= today - datetime.timedelta(days=1)

tweets_list = tweepy.Cursor(api.search_tweets, q="vegano since:" + str(yesterday)+ " until:" + str(today),tweet_mode='extended', lang='es').items()

output = []
for tweet in tweets_list:
    text = tweet._json["full_text"]
    print(text)
    favourite_count = tweet.favorite_count
    retweet_count = tweet.retweet_count
    created_at = tweet.created_at
    
    line = {'text' : text, 'favourite_count' : favourite_count, 'retweet_count' : retweet_count, 'created_at' : created_at}
    output.append(line)



df = pd.DataFrame(output)
df.to_csv('output.csv')
