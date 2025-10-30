# MongoDB CRUD Basics: Insert & Find

A quick reference to creating documents in collections and querying data using various `insert` and `find` methods in the MongoDB shell or `mongosh`.

---

## 1. Creating Documents

### 1.1 `insertOne()`

Insert a single document into the current collection.

```js
// Syntax:
db.collection.insertOne(document, options)

// Example:
db.users.insertOne({
  name: "Alice",
  age: 30,
  email: "alice@example.com",
  createdAt: new Date()
});
```

* **Returns** an acknowledgement with `insertedId`.
* **Options**: `writeConcern`, `bypassDocumentValidation`.

---

### 1.2 `insertMany()`

Insert multiple documents in one request.

```js
// Syntax:
db.collection.insertMany([ doc1, doc2, ... ], options)

// Example:
db.products.insertMany([
  { name: "Widget", price: 9.99 },
  { name: "Gadget", price: 19.99 },
  { name: "Doohickey", price: 4.99 }
]);
```

* **Returns** `insertedCount` and an array of `insertedIds`.
* **Options**: `ordered` (default `true` — stop on first error), `writeConcern`.

---

### 1.3 Bulk Operations (Optional)

For advanced batched writes (insert/update/delete) in one call.

```js
const ops = [
  { insertOne: { document: { a: 1 } } },
  { insertOne: { document: { b: 2 } } }
];

// Execute:
db.collection.bulkWrite(ops);
```

* **More**: see MongoDB docs for `bulkWrite` with `updateOne`, `deleteMany`, etc.

---

## 2. Finding Documents

### 2.1 `find(query, projection)`

Retrieves a cursor to all documents matching `query`.

```js
// Syntax:
const cursor = db.collection.find(filter, options);

// Example: find all active users, only return name and email
const cursor = db.users.find(
  { status: "active" },              // filter
  { projection: { name: 1, email: 1 } } // options
);

// Iterate:
cursor.forEach(doc => printjson(doc));
// Or convert to an array:
const results = cursor.toArray();
```

| Parameter | Description                                             |
| --------- | ------------------------------------------------------- |
| `filter`  | Query selector (e.g. `{ age: { $gt: 18 } }`)            |
| `options` | Object with `projection`, `sort`, `skip`, `limit`, etc. |

---

### 2.2 `findOne(query, projection)`

Returns the **first** matching document or `null`.

```js
// Syntax:
const doc = db.collection.findOne(filter, options);

// Example:
const alice = db.users.findOne({ name: "Alice" });
```

* **Simplest** way to fetch a single document.
* Supports the same `options` object as `find()`.

---

### 2.3 Cursor Helpers

After calling `find()`, use these chainable methods on the cursor:

* **`.sort(spec)`**: order results (e.g. `.sort({ createdAt: -1 })`).
* **`.limit(n)`**: restrict to `n` documents.
* **`.skip(n)`**: skip the first `n` documents.
* **`.count()`**: count matching documents.

```js
// Example: latest 5 products over $10
const top = db.products.find(
  { price: { $gt: 10 } }
)
.sort({ createdAt: -1 })
.limit(5)
.toArray();
```

---

## 3. Tips & Best Practices

* **Validation**: define schema validation to ensure data integrity.
* **Indexes**: create indexes on fields you query frequently for performance.
* **Projections**: always project only needed fields to reduce network payload.
* **Error Handling**: check return values (`acknowledged`, `insertedId`, etc.) when writing scripts.

---

*End of MongoDB Insert & Find Reference*
