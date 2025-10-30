# MongoDB Sorting & Limiting

A guide to ordering and constraining query results in MongoDB shell (`mongosh`) using `sort()`, `limit()`, and related methods.

---

## 1. Sorting Documents

### 1.1 `.sort()`

Orders the result set according to one or more fields.

```js
// Syntax:
db.collection.find(filter)
  .sort(sortSpec)

// sortSpec: { <field1>: 1|-1, <field2>: 1|-1, ... }
// 1 = ascending, -1 = descending

// Example: get users sorted by age ascending
const usersByAge = db.users.find()
  .sort({ age: 1 })
  .toArray();
```

| Parameter  | Description                      |
| ---------- | -------------------------------- |
| `filter`   | Optional query selector          |
| `sortSpec` | Fields and directions to sort by |

---

## 2. Limiting Number of Results

### 2.1 `.limit()`

Restricts the number of documents returned.

```js
// Syntax:
db.collection.find(filter)
  .limit(n)

// Example: return only the first 5 products
const top5 = db.products.find()
  .limit(5)
  .toArray();
```

* Use before `toArray()` or iteration.
* Commonly paired with `sort()` to get “top N” results.

---

## 3. Skipping Documents

### 3.1 `.skip()`

Skips the first `n` documents in the result set.

```js
// Syntax:
db.collection.find(filter)
  .skip(n)

// Example: skip the first 10 orders
const page2 = db.orders.find()
  .skip(10)
  .limit(10)
  .toArray();  // pagination
```

* Useful for pagination: `.skip(pageSize * (pageNumber - 1)).limit(pageSize)`

---

## 4. Combining Sort, Skip & Limit

Example: retrieve page 3 of posts sorted by creation date (newest first), 10 posts per page:

```js
const pageSize = 10;
const pageNumber = 3;

const page3 = db.posts.find()
  .sort({ createdAt: -1 })
  .skip(pageSize * (pageNumber - 1))
  .limit(pageSize)
  .toArray();

// This returns posts 21–30 by newest first
```

---

## 5. Cursor Count

### 5.1 `.count()` (Deprecated) & `.countDocuments()`

* **`.count()`** on a cursor: counts matching documents (deprecated in server 4.0+).
* **`.countDocuments(filter)`**: recommended to get count of filter.

```js
const total = db.posts.countDocuments({ published: true });
```

---

*End of MongoDB Sorting & Limiting Reference*
