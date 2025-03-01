# Potential Pitfalls of Pagination


## Link

https://jade.ellis.link/blog/2024/07/14/the-potential-pitfalls-of-pagination

## Notes


Offset-based pagination

```
GET /notification/<user_id>/fetch?offset=0&count=100
```
- Data Shifting
- Database scanning


Time-based Pagination

```
GET /api/notifications?start_time=2024-07-13T23:00:00Z&end_time=2024-07-13T23:50:00Z
```
- Excessive requests with sparse data
- Overwhelming large datasets


ID Based Paginations

```
GET /r/subreddit/new?limit=25&after=t3_10omtd
```
- Dated Anchors

