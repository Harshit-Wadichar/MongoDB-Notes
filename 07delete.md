# MongoDB Delete Methods

This reference explains how to delete documents from a MongoDB collection using `deleteOne`, `deleteMany`, and `findOneAndDelete`, along with syntax, examples, and return values.

---

## 1. `deleteOne()`

Deletes the **first** document that matches the filter.

```js
// Syntax:
db.collection.deleteOne(filter, options);

// Example: delete a single user with the name "Alice"
db.users.deleteOne({ name: "Alice" });
```

* **Options**: `collation`, `writeConcern`, `hint`
* **Returns**: `{ acknowledged: true, deletedCount: 1 }`

---

## 2. `deleteMany()`

Deletes **all documents** matching the filter.

```js
// Syntax:
db.collection.deleteMany(filter, options);

// Example: delete all users who are inactive
db.users.deleteMany({ isActive: false });
```

* **Options**: `collation`, `writeConcern`, `hint`
* **Returns**: `{ acknowledged: true, deletedCount: <number> }`

---

## 3. `findOneAndDelete()`

Finds a single document, deletes it, and returns the **deleted document**.

```js
// Syntax:
db.collection.findOneAndDelete(filter, options);

// Example: delete and return the oldest log entry
const deleted = db.logs.findOneAndDelete(
  { level: "error" },
  { sort: { createdAt: 1 }, projection: { _id: 0, message: 1 } }
);
printjson(deleted);
```

* **Options**:

  * `projection`: return only specific fields
  * `sort`: define which document to delete first
  * `maxTimeMS`: set time limit for operation
* **Returns**: the document that was deleted

---

## 4. Bulk Delete Using `bulkWrite()`

Perform multiple delete operations in a single batch.

```js
const ops = [
  { deleteOne: { filter: { status: "expired" } } },
  { deleteMany: { filter: { archived: true } } }
];

db.documents.bulkWrite(ops);
```

* **Useful For**: batch deletions with mixed write operations
* **Returns**: `{ deletedCount, modifiedCount, upsertedCount, ... }`

---

## 5. Deleting All Documents

To delete **all documents** from a collection:

```js
db.collection.deleteMany({});
```

* Leaves the collection **intact** (schema/indexes remain)

To drop the entire collection:

```js
db.collection.drop();
```

* Removes all documents **and** the collection itself

---

*End of MongoDB Delete Methods Reference*
