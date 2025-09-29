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
4. seasonality is present, increased activity: 10% in summer and 20% on weekends and holidays
5. support for web and mobile platforms
6. activity:  
6.1 an averege each user creates 5 posts per week  
6.2 an averege each user views 20 posts per day  
6.3 an average each user views the full photo in 50% of posts  
6.4 an averege each user evaluates 10 posts per day  
6.5 an averege each user comment 2 posts per day  
7. limits:  
7.1 number of characters in text is no more than 100 per post    
7.2 number of characters in comment is no more than 100 per post    
7.3 size of photo is no more than 1 MB on preview no more that 0.4 MB  
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
RPS = DAU * posts_per_week * season_coeff / week_seconds = 10 000 000 * 5 * 1.2 / 604 800 = 100
traffic_media = RPS * media_size = 100 * 1 MB = 100 MB/s
meta_size = text + geo point + id = 100 B + 16 B + 4 B = 120 B
traffic_meta = RPS * meta_size = 100 * 120 B = 12 KB/s
```
### View post:
```
RPS = DAU * posts_per_day * season_coeff / day_seconds = 10 000 000 * 20 * 1.2 / 86 400 = 2 800
traffic_media = RPS * preview_media_size + RPS * full_photo_coef * full_media_size = 2 800 * 0.4 MB + 2800 * 0.5 * 1 MB= 2.5 GB/s
meta_size = text + geo point + user_id + post_id + evaluate + timestamp  = 100 B + 16 B + 4 B + 8 B + 4 B + 4 B = 136 B
traffic_meta = RPS * meta_size = 2800 * 136 B = 372 kB/s
```
### Evaluates post:
```
RPS = DAU * eval_per_day / day_seconds = 10 000 000 * 10 / 86 400 = 1 200
traffic_media = RPS * eval_size = 1 200 * 40 B = 48 KB/s
```
### Comment post:
```
RPS = DAU * comment_per_day / day_seconds = 10 000 000 * 2 / 86 400 = 240
traffic = RPS * comment_size = 240 * 250 B = 60 KB/s
```