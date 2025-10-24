# Pagination

When you have a endpoint that returns a large list of items, it is often useful to paginate the results. This means breaking the results into smaller chunks, or "pages", that can be retrieved one at a time.

Basic benefit here is to reduce the amount of data transferred in a single request, which can improve performance and reduce load on the server. Less latency and more responsive applications are the end result.

## Pagination Strategies

1. **Offset-Based Pagination**: This is the most common method, where the client specifies an offset (the starting point) and a limit (the number of items to return). For example, `GET /items?offset=20&limit=10` would return items 21-30.
2. **Cursor-Based Pagination**: Instead of using an offset, this method uses a cursor (a unique identifier for a specific item) to mark the starting point for the next page of results. For example, `GET /items?cursor=abc123&limit=10` would return the next 10 items after the item with the cursor `abc123`.
3. **Page Number Pagination**: This method uses page numbers to specify which page of results to return. For example, `GET /items?page=2&limit=10` would return the second page of results, with 10 items per page.
4. **Keyset Pagination**: This method uses the values of the last item in the current page to determine the starting point for the next page. For example, if the last item on the current page has an ID of 50, the next request might be `GET /items?after_id=50&limit=10`.
