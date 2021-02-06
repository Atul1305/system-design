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

# Challenge
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

# References
https://redislabs.com/blog/redis-mysql-fast-economic-scaling/
https://www.youtube.com/watch?v=wYk0xPP_P_8


