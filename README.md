# TravelSN - System Design
This is a repository with homework for [course](https://balun.courses/courses/system_design) by Systems design.  
TravelSN is a special social network for travelers.

## Functional requirements:
1. Posts publishing with:    
1.1 short text  
1.2 photos   
1.3 geo positions
2. Activity in other travelers posts:  
2.1 like\dislike  
2.2 comments publishing
3. Subscribe to travelers
4. Search for popular travel destinations
5. Viewing:  
5.1 the chosen traveler feed  
5.2 subscription-based user feed
## Non-functional requirements:
1. DAU - 10 000 000
2. availability 99,95%
3. geo distribution is not needed
4. seasonality is present, increased activity: 20% on summer, weekends and holidays
5. support for web and mobile platforms
6. activity:  
6.1 an averege each user creates 5 posts per week  
6.2 On average each user views the feed 5 times a day  
6.3 an averege each user evaluates 10 posts per day  
6.4 an averege each user comment 2 posts per day  
6.5 an averege each user read 40 comment per day  
7. limits:  
7.1 number of characters in text is no more than 100 per post    
7.2 number of characters in comment is no more than 100 per post    
7.3 size of photo is no more than 1 MB on preview no more that 0.4 MB
7.4 size of feed is 10 posts  
7.4 one photo one post
8. timing:
8.1 post publiching: 1-3s  
8.2 like\dislike: 1s  
8.3 comment publishing: 1s  
8.4 subscribe: 1s  
8.5 search by geo points: 1-2s

## Basic calculations:
### Create post:
```
RPS = DAU * create_posts_per_week * season_coeff / week_seconds = 10 000 000 * 5 * 1.2 / 604 800 = 100
traffic_media = RPS * media_size = 100 * 1 MB = 100 MB/s
meta_size = text + geo point + id = 100 B + 16 B + 4 B = 120 B
traffic_meta = RPS * meta_size = 100 * 120 B = 12 KB/s
```
### View post:
```
RPS = DAU * view_feed_per_day * feed_size * season_coeff / day_seconds = 10 000 000 * 5 * 10 * 1.2 / 86 400 = 7 000
traffic_media = RPS * photo_size = 7000 * 1 MB = 6.9 GB/s
meta_size = text + geo point + user_id + post_id + like_count + dislike_count + timestamp  = 100 B + 16 B + 4 B + 8 B + 4 B + 4 B + 4 B = 140 B
traffic_meta = RPS * meta_size = 2800 * 136 B = 372 kB/s
```
### Evaluates post:
```
RPS = DAU * eval_per_day * season_coeff / day_seconds = 10 000 000 * 10 * 1.2 / 86 400 = 1 400
traffic = RPS * (user_id + post_id + like_or_dislike) = 1 400 * (4 B + 8 B + 1 B) = 18 KB/s
```
### Comment post:
```
RPS = DAU * write_comment_per_day * season_coeff / day_seconds = 10 000 000 * 2 * 1.2 / 86 400 = 280
traffic = RPS * (text + post_id) = 280 * (100 B + 16 B) = 32 KB/s
```
### Comment view:
```
RPS = DAU * read_comment_per_day * season_coeff / day_seconds = 10 000 000 * 40 * 1.2 / 86 400 = 5600
traffic = RPS * (text + user_id + post_id + comment_id) = 5600 * (100 B + 4 B + 8 B + 8 B) = 590 kB
```
