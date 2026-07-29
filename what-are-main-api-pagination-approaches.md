
# API Pagination approaches

- [API Pagination approaches](#api-pagination-approaches)
  - [Offset-Based Pagination](#offset-based-pagination)
    - [Pros of offset-based pagination](#pros-of-offset-based-pagination)
    - [Cons of offset-based pagination](#cons-of-offset-based-pagination)
  - [Cursor-Based Pagination](#cursor-based-pagination)
    - [Pros of cursor-based pagination](#pros-of-cursor-based-pagination)
    - [Cons of cursor-based pagination](#cons-of-cursor-based-pagination)
  - [Direct Comparison](#direct-comparison)
  - [Rule of Thumb](#rule-of-thumb)

---

## Offset-Based Pagination

This is the traditional, default method used in most web applications and SQL databases.

- API Format:

```bash
  GET /posts?page=3&limit=10` or `GET /posts?offset=20&limit=10
```

### Pros of offset-based pagination

- `Random Access:`
  > Users can skip straight to page 42 or jump back to page 1 easily.

- `Simple Implementation:`
  > Built-in support across almost all database frameworks and UI pagination controls (`[1] [2] [3] ... [Next]`).

- `Total Page Count:`
  > Easy to calculate the total number of pages upfront (`Total Rows / Limit`).

### Cons of offset-based pagination

- Performance Degradation (The "Deep Paging" Problem):

- `Data Drift / Missed Records:`
  > If a new record is added to page 1 while a user is reading page 1, going to page 2 will show the last item from page 1 again (a duplicate). If an item is deleted, an item gets skipped entirely.

---

## Cursor-Based Pagination

Instead of specifying an offset number, you pass a pointer (a `cursor`) to the specific item where the last page ended. The cursor is typically an encoded unique identifier, like a timestamp combined with an ID.

- API Format:

```bash
GET /posts?cursor=eyJpZCI6MTI1fQ==&limit=10
```

### Pros of cursor-based pagination

- `Consistent, High Performance:` 
  > Scales gracefully to millions of records. The database uses an index scan to jump straight to the matching row (complexity is $O(\log N)$ or $O(1)$ instead of $O(N)$).

- `Stable Data State:`
  > Unaffected by insertions or deletions. New items added at the top do not shift the relative position of the items below the current cursor.

- `Ideal for Infinite Scrolls:`
  > Perfect for feeds (like Twitter, Instagram, or Slack) where users continually scroll down for more content.

### Cons of cursor-based pagination

- `No Random Access:`
  > You cannot jump directly to a specific page number without fetching all preceding pages first

- `Limited Navigation:`
  > Works best for "Next / Previous" navigation; cannot show total page count easily.

- `Complex Implementation:`
  > Harder to implement, especially when sorting by non-unique fields (e.g., sorting by `created_at` requires combining `created_at` + `id` into a composite cursor).

---

## Direct Comparison

| Feature | Offset-Based | Cursor-Based |
| --- | --- | --- |
| `Primary Use Case` | Tables, admin panels, search results | Social feeds, chat logs, infinite scrolls |
| `Performance` | Drops significantly as page number increases | Consistently fast regardless of depth |
| `Data Consistency` | Prone to duplicates/skipped items during real-time writes | Highly stable during real-time writes |
| `Page Navigation` | Can jump to any page number | Sequential navigation only (Next/Prev) |
| `Sorting Complexity` | Easy | Requires unique/indexed sort keys |

---

## Rule of Thumb

> Use `Offset` if you need page numbers and your dataset is relatively small (under ~50,000 records) or infrequently updated.
> Use `Cursor` if you have a massive dataset, high-frequency writes, or are building an infinite scroll UI
