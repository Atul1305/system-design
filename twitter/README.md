# twitter-design

#Features:
-> Tweet

-> Timeline
	
	-> Home
	-> User
	-> Search

-> Trends

# Statistics
No. Of Users - 300M+
Write : 600 Tweets/Sec
Read : 600,000 Tweets/Sec

# Challenges
Read Heavy, Eventual Consistence and Storage

Length of per tweet: 140 Chars

# Databases
RDBMS and Redis will be used.
-> User Table
-> Tweet Table
-> Follower Table

Redis : <User_Id>_Tweets - [1, 2, 3.....] (List Datastructures)
		<User_Id>_Followers - [1, 2, 3.....]

Db Relation : One to many relation between user to Tweet and Follower tables.

# Design
**Tweet**

Save it to the RDBMS.

**Home Timeline**

	-> Get followers.
	-> Get latest tweets.
	-> Merge & Display.

Solution:Fanout approach

Add tweet of user to the timeline(Built in redis, backed-up in RDBMS) of all its followers. If user do follow any of the celebrities, go and read the celebrity timeline's recent tweet(maintained under redis).

To improve performance further, just fanout messages to the users who were active recently, like who had logged in to their account in last 7 or 10 days.(Maintain a metrics how many users are highly active to twitter, will help to take further design.)

Note: Any user with 1M+ followers is celebrity.
	
**Trends**

	-> Volume of Tweets
	-> Timetaken to generate Tweets
	
Solution: STORM Apache Kafka Streams/Heron

# Architecture
	-> Filter (Kafka Queue) - Do filteration and Violations check
	-> Parse - Do parsing and implement NLP
	-> Geo Location
	-> Count Location 
	-> Count Hashtag
	-> Rank
	-> Redis

# References
https://redislabs.com/blog/redis-mysql-fast-economic-scaling/
https://www.youtube.com/watch?v=wYk0xPP_P_8