

```python
# Dependencies
import json
import tweepy
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
consumer_key = "Ed4RNulN1lp7AbOooHa9STCoU"
consumer_secret = "P7cUJlmJZq0VaCY0Jg7COliwQqzK0qYEyUF9Y0idx4ujb3ZlW5"
access_token = "839621358724198402-dzdOsx2WWHrSuBwyNUiqSEnTivHozAZ"
access_token_secret = "dCZ80uNRbFDjxdU2EckmNiSckdoATach6Q8zb7YYYE5ER"
```


```python
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, wait_on_rate_limit=True, parser=tweepy.parsers.JSONParser())
```


```python
timestamp=[]
text=[]
screen_name=[]
positive=[]
negative=[]
neutral=[]
symbol=[]
compound=[]
counter=0
target_users=["@nytimes","@BBC","@CBSNews", "@CNN","@FoxNews"]
#public_tweets = api.user_timeline(target_users)

# Loop through all tweets
# for tweet in public_tweets:
#     # Utilize JSON dumps to generate a pretty-printed json
#     print(json.dumps(tweet, sort_keys=True, indent=4))


for user in target_users:
    while counter < 2 :
        for page in range (2):
           
            tweets=api.user_timeline(user, count = 2, page=page)
            for tweet in tweets:
                     timestamp.append(tweet["created_at"])
                     text.append(tweet["text"])
                     screen_name.append(tweet["user"]["screen_name"])
                     symbol.append("Bitcoin")
                     scores = analyzer.polarity_scores(tweet["text"])
                     comp = scores['compound']
                     pos = scores['pos']
                     neut = scores['neu']
                     neg = scores['neg']

                     compound.append(compound)
                     positive.append(pos)
                     negative.append(neg)
                     neutral.append(neut)


    counter+=1
# #    time.sleep(960)
twitter_df = pd.DataFrame({'name':screen_name, 'text':text, 'timestamp':timestamp, 'symbol':symbol,
                             'positive':positive, 'negative':negative, 'neutral':neutral})
    
twitter_df = twitter_df[['name', 'text', 'timestamp', 'symbol', 'positive', 'negative', 'neutral']]
twitter_df.head()
twitter_df.to_csv('pt1.csv')
```


```python
plt.scatter(screen_name=["@nytimes"], screen_name=["@BBC"], screen_name=["@CBSNews"], screen_name=["@CNN"], 
            screen_name=["@FoxNews"])
plt.savefig('twitterbarchart.png')
plt.show()
```


```python
# Import and Initialize Sentiment Analyzer
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()

# Target User Account
users = ["@nytimes","@BBC","@CBSNews", "@CNN","@FoxNews"]

for target_user in users:

    # Variables for holding sentiments
    sentiments = []
    compound_list = []
    positive_list = []
    negative_list = []
    neutral_list = []

    # Loop through 10 pages of tweets (total 200 tweets)
    for page in range(1):

        # Get all tweets from home feed
        public_tweets = api.user_timeline(target_user, page=page)

        # Loop through all tweets
        for tweet in public_tweets:

            text = tweet['text']
            # print(text)

            # Run Vader Analysis on each tweet
            scores = analyzer.polarity_scores(text)
            compound = scores['compound']
            pos = scores['pos']
            neu = scores['neu']
            neg = scores['neg']

            # Add each value to the appropriate list
            compound_list.append(compound)
            positive_list.append(pos)
            negative_list.append(neg)
            neutral_list.append(neu)
            
            sentiments.append({"Company": target_user,
                            "Compound": compound,
                           "Positive": pos,
                           "Negative": neu,
                           "Neutral": neg})

    # Print the Averages
    print('')
    print('User: %s' % target_user)
    print(f'Compound: {np.mean(compound_list)}')
    print(f'Positive: {np.mean(positive_list)}')
    print(f'Neutral: {np.mean(neutral_list)}')
    print(f'Negative: {np.mean(negative_list)}')
```


```python
print(sentiments.append)
sentiments_df = pd.DataFrame.from_dict(sentiments)
sentiments_df.head(50)
sentiments_df.to_csv('sentiments.csv')
# sentiments_pd = pd.DataFrame.from_dict(sentiments)
# sentiments_pd["Compound"]
# print(sentiments_pd["Compound"])
```

    <built-in method append of list object at 0x116791188>





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Company</th>
      <th>Compound</th>
      <th>Negative</th>
      <th>Neutral</th>
      <th>Positive</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@FoxNews</td>
      <td>-0.3582</td>
      <td>0.670</td>
      <td>0.217</td>
      <td>0.113</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@FoxNews</td>
      <td>0.6808</td>
      <td>0.752</td>
      <td>0.000</td>
      <td>0.248</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@FoxNews</td>
      <td>0.3400</td>
      <td>0.769</td>
      <td>0.000</td>
      <td>0.231</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@FoxNews</td>
      <td>0.4939</td>
      <td>0.738</td>
      <td>0.000</td>
      <td>0.262</td>
    </tr>
    <tr>
      <th>5</th>
      <td>@FoxNews</td>
      <td>0.3182</td>
      <td>0.594</td>
      <td>0.182</td>
      <td>0.224</td>
    </tr>
    <tr>
      <th>6</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>@FoxNews</td>
      <td>0.4019</td>
      <td>0.691</td>
      <td>0.115</td>
      <td>0.194</td>
    </tr>
    <tr>
      <th>9</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>@FoxNews</td>
      <td>-0.7351</td>
      <td>0.708</td>
      <td>0.292</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>@FoxNews</td>
      <td>0.6369</td>
      <td>0.826</td>
      <td>0.000</td>
      <td>0.174</td>
    </tr>
    <tr>
      <th>14</th>
      <td>@FoxNews</td>
      <td>0.4939</td>
      <td>0.738</td>
      <td>0.000</td>
      <td>0.262</td>
    </tr>
    <tr>
      <th>15</th>
      <td>@FoxNews</td>
      <td>-0.4019</td>
      <td>0.856</td>
      <td>0.144</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>@FoxNews</td>
      <td>0.6808</td>
      <td>0.752</td>
      <td>0.000</td>
      <td>0.248</td>
    </tr>
    <tr>
      <th>17</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>@FoxNews</td>
      <td>0.0000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>@FoxNews</td>
      <td>-0.7351</td>
      <td>0.763</td>
      <td>0.237</td>
      <td>0.000</td>
    </tr>
  </tbody>
</table>
</div>




```python
x_values = np.arange(len(users))
print(x_values)
print(sentiments_pd["Compound"])
#need to make draw compound list per username
```

    [0 1 2 3 4]
    0     0.0000
    1    -0.3582
    2     0.6808
    3     0.3400
    4     0.4939
    5     0.3182
    6     0.0000
    7     0.0000
    8     0.4019
    9     0.0000
    10    0.0000
    11   -0.7351
    12    0.0000
    13    0.6369
    14    0.4939
    15   -0.4019
    16    0.6808
    17    0.0000
    18    0.0000
    19   -0.7351
    Name: Compound, dtype: float64



```python
plt.bar(x_values, sentiments_df[compound_list], tick_label=users)

plt.xlabel('User')
plt.ylabel('Scores')
plt.title('Scores by Twitter Account')
plt.savefig('sentiment_analysis.png')
# Tell matplotlib that we will be making a bar chart
# Users is our y axis and x_values is our x axis

#plt.bar(x_values, users, color='r', alpha=0.5,
#        tick_label=languages)

plt.show()


```
