# MongoDB `find()` and Cursor Methods

Detailed reference to querying collections with `find()` and working with the returned cursor in the MongoDB shell (`mongosh`).

---

## 1. `find()` Basics

* **Syntax:**

  ```js
  cursor = db.collection.find(filter, options)
  ```

* **Parameters:**

  * `filter`: Query selector object (e.g., `{ age: { $gt: 18 } }`).
  * `options`:

    * `projection`: Specify fields to include or exclude (e.g., `{ name: 1, email: 1 }`).
    * `sort`: Sorting specification (e.g., `{ createdAt: -1 }`).
    * `skip`: Number of documents to skip.
    * `limit`: Maximum number of documents to return.
    * `batchSize`: Number of documents per batch from the server.

* **Returns:** A cursor to iterate over matching documents.

**Example:**

```js
// Find active users, return only name & email
const c = db.users.find(
  { status: "active" },
  { projection: { name: 1, email: 1 } }
);
```

---

## 2. Cursor Iteration Methods

Once you have a cursor, these methods help you process results:

| Method         | Description                                    |
| -------------- | ---------------------------------------------- |
| `.toArray()`   | Returns all documents as an array.             |
| `.forEach(fn)` | Executes `fn` for each document.               |
| `.next()`      | Retrieves the next document or throws if none. |
| `.hasNext()`   | Checks if more documents remain.               |
| `.close()`     | Closes the cursor early.                       |

```js
// Example: print each document
c.forEach(doc => printjson(doc));
```

---

## 3. Common Cursor Helper Methods

Chainable methods to refine and control the result set:

* **`.sort(spec)`**: Order by fields.

  ```js
  .sort({ age: 1, createdAt: -1 })
  ```

* **`.limit(n)`**: Return at most `n` documents.

  ```js
  .limit(10)
  ```

* **`.skip(n)`**: Skip first `n` documents.

  ```js
  .skip(20)
  ```

* **`.batchSize(n)`**: Server returns `n` docs per batch.

  ```js
  .batchSize(5)
  ```

* **`.count()`** (deprecated) vs **`countDocuments()`**:

  * `.count()` on a cursor is deprecated in newer servers.
  * For accurate counts, use:

    ```js
    db.collection.countDocuments(filter)
    ```

---

## 4. Pretty Printing

Use `.pretty()` to format JSON output for readability:

```js
db.users.find({}).pretty();
```

---

## 5. Explain Query Plan

Analyze performance by appending `.explain()`:

```js
db.orders.find({ status: "shipped" }).explain("executionStats");
```

Returns detailed execution stats and index usage.

---

## 6. `findOne()` Shortcut

* **Syntax:**

  ```js
  doc = db.collection.findOne(filter, options)
  ```

* Fetches the first matching document without returning a cursor.

```js
const user = db.users.findOne({ email: "alice@example.com" });
```

---

*End of MongoDB `find()` & Cursor Methods Reference*
