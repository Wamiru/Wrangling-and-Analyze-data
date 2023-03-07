# Wrangling-and-Analyze-data

The dataset that was wrangled is the tweet archive of Twitter user @dog_rates, also known as WeRateDogs. WeRateDogs is a Twitter account that rates people's dogs with a humorous comment about the dog. These ratings almost always have a denominator of 10. The numerators, though? Almost always greater than 10. 11/10, 12/10, 13/10, etc.

There were 3 datasets to be gathered. Each in a different way to harness the skills learnt.

The first dataset, called `twitter_archive_enhanced.csv`, was downloaded manually and uploaded to my workspace. This dataset contains tweet data from the WeRateDogs Twitter of about  5000 tweets.

The second dataset, called `image_predictions.tsv` was downloaded programmatically using the `get()` method of the `requests` library.This contains image predictions to classify the dog breeds using a neural network. 

The final dataset was fetched by querying the twitter api using the tweepy library. This was done by first extracting all the tweet_ids from from the `twitter_archive_enhanced.csv`, then looping through each id and query the Twitter API to get the each tweet_id's json data. This data was then dumped line by line in a text file called `tweet-json.txt`.
Finally, I read the data in the .txt file line by line and appended it in a dataframe called **json_df**

### Assessing Data

I used both the visual and programmatic approaches to assess the data gathered and found the below issues regarding quality and tidiness:
### Quality issues
#### `archive` table
1. `tweet_id` is an object not an integer

2. Missing records in several columns namely:
* `in_reply_to_status_id`
* `in_reply_to_user_id`
* `retweeted_status_id`
* `retweeted_status_user_id`
* `retweeted_status_timestamp`
* `expanded_urls`

3. Duplicates in `name` columm e.g canela

4. `timestamp` column is datetime not object

5. Correct values in `rating_numerator` with decimals

6. Keep original ratings and not retweets

7. `text` column includes texts and urls.

8. `source` column values in html  

#### `image_preds` table

1. `tweet_id` is an object not an integer

2. `img_num` is an object, not an integer

3. Missing records(2075 instead of 2356)


#### `json_df` table 

1. Missing records in table (2354 instead 2356)

2. `favorite_count` and `retweet_count` datatype should be integers not strings/object

### Tidiness issues
1. Columns `doggo`, `floofer`,`pupper` and `puppo` in `archive` table to be merged into one column called dog_stages.

2. `image_preds` and `json_df` table to be merged with the archive table

### Cleaning Data

Before starting this stage I made sure to make copies of the original datasets using the `.copy()` function.

I used the **Define-Test-Code** framework to detail the steps taken in solving all the issues highlighted in the assessing stage.

These are all the steps I took in my cleaning process:

1. I extracted the retweets and replies by checking the values in the ` retweeted_status_id` and `in_reply_to_status_id ` columns and dropping them from the dataframeI Extract the retweets and replies by checking the values in the ` retweeted_status_id` and `in_reply_to_status_id ` columns and dropping them from the dataframe.

2. I dropped the below columns with high numbers of null values: 
* `in_reply_to_status_id`
* `in_reply_to_user_id `
* `retweeted_status_id`
* `retweeted_status_user_id`
* `retweeted_status_timestamp`

3. I converted columns that didn't have the correct datatypes in my assessment to the correct datatypes.

4. I extracted only the texts in the `text` column and removed the unnecessary url information.

5. I cleaned up the values in the `source` column which were entered as html data but I only needed the text information within the html links. I thus proceeded and used BeautifulSoup to extract only the texts in this coulmn.

6. I corrected some values in the `rating_numerator` column which had been entered incorrectly.

7. I wrote a function which I used to merged the dog_stages columns into one column.

8. I then merged the dataframes into one master dataframe

I finally stored my clean dataset in a new csv file for use in the analysis stage.
