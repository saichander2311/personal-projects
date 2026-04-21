# Cache

## What is Cache

Cache is a temporary storage layer that keeps frequently used data closer to the application so it can be accessed faster than reading it from the database every time. It stores copies of data that are likely to be requested again soon. This improves speed and reduces repeated work.

A cache can hold items like user profiles, product details, search results or session data. When the requested data is found in cache, the system returns it immediately. If not, the system fetches it from the database and may store it in cache for future use.

## How cache reduces database load

Without cache, every request may go directly to the database. That creates more read queries, which increases database traffic, CPU usage and response time. Cache reduces this by serving repeated requests from memory instead of the database.

This is especially useful for read-heavy systems where the same data is requested many times. For example, if thousands of users view the same product page, cache can serve that product data many times while the database is accessed only occasionally. That lowers load and helps the database focus on writes and complex queries.

## Where cache operates

Cache usually operates between the application and the database. It can also exist in different layers depending on the system design.

**Common places where cache is used:**

- Application cache - inside the app layer for frequently used objects.
- Browser cache - in the user's browser for static content.
- Server-side cache - on the backend server for repeated responses.
- Distributed cache - such as Redis or Memcached, shared across multiple servers.

In a typical setup, the application first checks the cache. If the data exists there, it is returned instantly. If not, the application queries the database, gets the result and stores it in cache for the next request.

## Before cache integration

Before cache is added, every request goes to the database. If 10,000 users request the same data, the database may receive nearly 10,000 read operations. This can slow down response time, especially during peak traffic.

**Example flow:**

1. User sends request.
2. Application checks database.
3. Database returns data.
4. Application sends response to user.

In this model, the database becomes the main bottleneck. As user traffic grows, performance drops and infrastructure costs can rise.

## After cache integration

After cache is added, the first request for data may go to the database, but later requests can be served from cache. This reduces database hits and improves speed.

**Example flow:**

1. User sends request.
2. Application checks cache.
3. If data exists in cache return it immediately.
4. If not fetch from database.
5. Store fetched data in cache.
6. Return response to user.

This model significantly reduces repetitive database queries and improves scalability.

## Example with 10,000 users

Assume 10,000 users request the same data.

**Before cache:**
- 10,000 requests go to the database.
- Database handles all 10,000 reads.
- Response time is slower under load.
- Database resources are heavily used.

**After cache:**
- First request may go to the database.
- Suppose 80% of requests are served from cache.
- 8,000 requests are returned from cache.
- Only 2,000 requests hit the database.

| Metric                    | Before cache | After cache |
|---------------------------|--------------|-------------|
| User requests             | 10,000       | 10,000      |
| Requests served from cache| 0            | 8,000       |
| Requests hitting database | 10,000       | 2,000       |
| Database load             | High         | Much lower  |
| Response speed            | Slower       | Faster      |

This means the database load is reduced by about 80% in this example. The exact reduction depends on cache hit rate, data freshness rules, and how often the data changes.

## Conclusion

Cache is a performance optimization technique that stores frequently accessed data temporarily so the system does not need to query the database repeatedly. It works between the application and the database and it is especially useful when many users request the same data. In a 10,000-user scenario, cache can dramatically reduce database load and improve response time.
