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
4. seasonality is present
5. support for web and mobile platforms
6. activity:  
6.1 an averege each user creates 5 posts per week 
6.2 an averege each user views 20 posts per day  
6.3 an averege each user evaluates 10 posts per day  
6.4 an averege each user comment 2 posts per day  
7. limits:  
7.1 number of characters in text is no more than 100 per post    
7.2 size of photo is no more than 5 MB    
7.3 one photo one post
8. timing:
8.1 post publiching: 2-5s  
8.2 like\dislike: 1s  
8.3 comment publishing: 1s  
8.4 subscribe: 1s  
8.5 search: 2-3s


## Basic calculations:

### Create post:
```
RPS = DAU * posts_per_week / week_seconds = 10 000 000 * 5 / 604 800 = 90
traffic = RPS * post_size(photo size) = 90 * 5 MB = 450 MB/s
```
### View post:
```
RPS = DAU * posts_per_day / day_seconds = 10 000 000 * 20 / 86 400 = 2 300
traffic = RPS * post_size(photo size) = 2 300 * 5 MB = 11 GB/s
```
### Evaluates post:
```
RPS = DAU * eval_per_day / day_seconds = 10 000 000 * 10 / 86 400 = 1 200
traffic = RPS * eval_size = 1 200 * 40 B = 48 KB/s
```
### Comment post:
```
RPS = DAU * comment_per_day / day_seconds = 10 000 000 * 2 / 86 400 = 240
traffic = RPS * comment_size = 240 * 250 B = 60 KB/s
```