# DMDD_Accidents

![WhatsApp Image 2022-11-12 at 11 51 50 PM](https://user-images.githubusercontent.com/113303754/201506389-d17a26b5-5369-4808-bcdc-32895b934ad0.jpeg)

This database comprises of accident data collected from twitter over five years from February 2016 to December 2021, covering 49 states and utilizing numerous APIs that give live traffic event data. The dataset contains about 35 thousand records. A number of intrinsic and contextual attributes, including severity, location, time, temperature, humidity, visibility, crossing etc. are included in each accident record. This database aids in our understanding of how the aforementioned factors affect accidents.

SQL Statements for the conceptual model: 

USER_DETAILS Table:
CREATE TABLE accidents.User_details(
User_ID bigint(50) primary key, 
Username varchar(50) not null, 
Follower_count int(10), 
Location varchar(50));

TWEETS Table:
CREATE TABLE accidents.Tweets_table(
User_ID bigint(50), 
Tweet_ID bigint(50) primary key, 
Tweet_text varchar(500), 
Retweets int(10), 
Retweeted varchar(10), 
Source varchar(100), 
Favourite_count int(10), 
HashTags varchar(300),
CONSTRAINT FK_PersonOrder FOREIGN KEY (User_ID) REFERENCES accidents.User_details(User_ID));

USE CASES
1.	Use Case: Highest re-tweet count based on car accidents
Description: Display the highest re-tweet count 
Actor: Person
Steps:
Post Condition: Successfully displays the re-tweet count 
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect

2.	Use Case: Most frequently used hashtags based on car accidents
Description: Display most frequent hashtags used. 
Actor: Person
Steps:
Post Condition: Successfully displays the re-tweet count 
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect


3.	Use Case: Tweet texts that include accidents caused by carcrashes
Description: Display list of tweet texts that include accidents caused by carcrashes.
Actor: Person
Steps:
Post Condition: Successfully displays the tweet text list 
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect

4.	Use Case: User that has the highest number of followers
Description: Display details of users having highest number of followers 
Actor: Person
Steps:
Post Condition: Successfully displays the user account details.
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect


5.	Use Case: Major cause of carcrashes
Description: Display major cause that leads to car crashes.
Actor: Person
Steps:
Post Condition: Successfully displays the major causes of car crashes.
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect


6.	Use Case: Frequently used source to post tweets 
Description: Display the most frequently used source details to post tweets
Actor: Person
Steps:
Post Condition: Successfully displays the source details
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect

7.	Use Case: Tweet count containing #drunkdriving keyword
Description: Display the most frequently used source details to post tweets
Actor: Person
Steps:
Post Condition: Successfully displays the source details
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect

8.	Use Case: List of user id active in tweeting 
Description: Displays most active users and tweet id of the user that are active in tweeting.
Actor: Person
Steps:
Post Condition: Successfully displays the active username details.
Alternate Path: The request is not correct and system throws an error
Error: Information is incorrect

RELATIONAL-ALGEBRA EXPRESSIONS FOR THE USE CASES

1.	Use Case: Highest re-tweet count based on car accidents

π MAX (retweet_count)
 γ MAX (retweet_count)
  σ tweet_text LIKE "%CarAccidents%" tweets_table

2.	Use Case: Most frequently used hashtags based on car accidents

π hashtags
 σ tweet_text LIKE "%fatal%" AND tweet_text LIKE "%crash%" AND tweet_text LIKE "%drive%" tweets_table

3.	Use Case: Tweet texts that include accidents caused by carcrashes

π tweet_text
 σ tweet_text LIKE "%carcrashes%" tweets_table

4.	Use Case: User that has the highest number of followers

π user_id, username user_details π MAX (follower_count)
 γ MAX (follower_count) user_details

5.	Use Case: Major cause of carcrashes

π tweet_text
 σ tweet_text LIKE "%caused by%" tweets_table

6.	Use Case: Frequently used source to post tweets 

τ COUNT (source) ↓
 γ source, COUNT (source) tweets_table

7.	Use Case: Tweet count containing #drunkdriving keyword

π COUNT (tweet_text)
 γ COUNT (tweet_text)
  σ hashtags LIKE "%drunkdriving%" tweets_table

8.	Use Case: List of user id active in tweeting

τ active_users ↓
 π user_id, COUNT (user_id) → active_users
  γ user_id, COUNT (user_id) tweets_table



SQL STATEMENTS

1.	Use Case: Highest re-tweet count based on car accidents

SELECT max(Retweets) as Count
FROM Tweets_table
WHERE Tweet_text like’%#Accidents%’

2.	Use Case: Most frequently used hashtags based on accident incidents

SELECT HashTags 
FROM Tweets_table
WHERE Tweet_Text like’%fatal%’	
AND Tweet_text like’%crash%’
AND Tweet_text like’%drive%’

3.	Use Case: Tweet texts that include accidents caused by carcrashes

SELECT Tweet_text
FROM Tweets_table
WHERE Tweet_text like’%carcrashes%’

4.	Use Case: User that has the highest number of followers

SELECT User_ID, Username
FROM User_details
Where Follower_count in (select max(Follower_count) from User_Details);

5.	Use Case: Major cause of carcrashes

SELECT Tweet_text as Major causes
FROM Tweets_table
Where Tweet_text like’%caused by%’

6.	Use Case: Frequently used source to post tweets 

SELECT Source, count(Source) as frequently_used_source
FROM Tweets_table
Group by Source
Order by ‘frequently_used_source’ desc

7.	Use Case: Tweet count containing #drunkdriving keyword

SELECT count(Tweet_text) as Count
FROM Tweets_table
Where HashTags like’%drunkdriving%’

8.	Use Case: List of user id active in tweeting 

SELECT User_ID, count(User_ID) as Active_users
FROM Tweets_table
Group by User_ID
Order by Active_users desc









