# MongoDB Update Methods

A quick guide to modifying documents in MongoDB collections using various update operations in the MongoDB shell (`mongosh`).

---

## 1. `updateOne()`

Updates a single document matching the filter.

```js
// Syntax:
db.collection.updateOne(
  <filter>,
  <update>,
  { upsert: <boolean>, writeConcern: <object>, bypassDocumentValidation: <boolean> }
);

// Example: set `status` to "active" for the user with name "Bob"
db.users.updateOne(
  { name: "Bob" },
  { $set: { status: "active" } }
);
```

| Option                     | Description                                           |
| -------------------------- | ----------------------------------------------------- |
| `upsert`                   | Insert document if no match found (default: `false`). |
| `writeConcern`             | Customize write acknowledgment.                       |
| `bypassDocumentValidation` | Skip schema validation.                               |

---

## 2. `updateMany()`

Updates all documents matching the filter.

```js
// Syntax:
db.collection.updateMany(
  <filter>,
  <update>,
  { upsert: <boolean>, ... }
);

// Example: increment `visits` by 1 for all users in New York
db.users.updateMany(
  { city: "New York" },
  { $inc: { visits: 1 } }
);
```

Returns `{ matchedCount, modifiedCount, upsertedId }`.

---

## 3. `replaceOne()`

Replaces an entire document (except `_id`) matching the filter.

```js
// Syntax:
db.collection.replaceOne(
  <filter>,
  <replacementDoc>,
  { upsert: <boolean>, ... }
);

// Example: replace a product document entirely
db.products.replaceOne(
  { sku: "12345" },
  { sku: "12345", name: "New Widget", price: 14.99, stock: 50 }
);
```

---

## 4. `findOneAndUpdate()`

Atomically find a document, update it, and return either the original or updated document.

```js
// Syntax:
db.collection.findOneAndUpdate(
  <filter>,
  <update>,
  {
    projection: <object>,
    sort: <object>,
    returnDocument: "before" | "after",  // default "before"
    upsert: <boolean>,
    maxTimeMS: <number>
  }
);

// Example: mark the oldest pending order as shipped and return the updated doc
const updated = db.orders.findOneAndUpdate(
  { status: "pending" },
  { $set: { status: "shipped", shippedAt: new Date() } },
  { sort: { createdAt: 1 }, returnDocument: "after" }
);
printjson(updated);
```

---

## 5. `findOneAndReplace()`

Atomically find a document, replace it entirely, and return it.

```js
// Syntax:
db.collection.findOneAndReplace(
  <filter>,
  <replacementDoc>,
  { projection, sort, returnDocument, upsert, maxTimeMS }
);

// Example:
const result = db.settings.findOneAndReplace(
  { _id: "config" },
  { _id: "config", theme: "dark", version: 2 },
  { returnDocument: "after" }
);
printjson(result);
```

---

## 6. `findOneAndDelete()`

Finds a document and deletes it, returning the deleted document.

```js
// Syntax:
db.collection.findOneAndDelete(
  <filter>,
  { projection, sort, maxTimeMS }
);

// Example: delete and return the most recently created temp record
db.temps.findOneAndDelete(
  { type: "session" },
  { sort: { createdAt: -1 } }
);
```

---

## 7. Bulk Write Operations

Perform multiple update operations in a single command.

```js
// Syntax:
const ops = [
  { updateOne: { filter: { a: 1 }, update: { $set: { b: 2 } }, upsert: true } },
  { updateMany: { filter: { status: "new" }, update: { $inc: { count: 1 } } } }
];

const result = db.collection.bulkWrite(ops);
printjson(result);
```

* Supports `insertOne`, `updateOne`, `updateMany`, `replaceOne`, `deleteOne`, `deleteMany`.

---

## 8. Common Update Operators

| Operator       | Description                              |
| -------------- | ---------------------------------------- |
| `$set`         | Set field value.                         |
| `$unset`       | Remove field.                            |
| `$inc`         | Increment/decrement numeric field.       |
| `$mul`         | Multiply numeric field.                  |
| `$rename`      | Rename a field.                          |
| `$push`        | Append value to array.                   |
| `$pop`         | Remove first or last element (`-1`/`1`). |
| `$pull`        | Remove all matching array elements.      |
| `$addToSet`    | Add value to array if it doesn't exist.  |
| `$currentDate` | Set field to current date.               |

---

*End of MongoDB Update Methods Reference*
