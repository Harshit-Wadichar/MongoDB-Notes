# MongoDB Data Types

A reference guide to the built-in BSON data types supported by MongoDB, with explanations and examples.

---

## 1. String

* **Definition:** A sequence of characters stored as UTF-8.
* **Use Case:** Names, descriptions, text fields.

```js
// Example:
db.users.insertOne({
  name: "Alice",
  bio: "Full-stack developer"
});
```

---

## 2. Integer (32-bit) & Long (64-bit)

* **`int32`**: 32-bit signed integer.
* **`int64`** (Long): 64-bit signed integer for larger values.
* **Use Case:** Counters, small to large numbers.

```js
// 32-bit int
db.counters.insertOne({ count32: 123 });

// 64-bit long in mongosh
db.metrics.insertOne({ count64: NumberLong("1234567890123") });
```

---

## 3. Double (Floating Point)

* **Definition:** 64-bit IEEE 754 floating point.
* **Use Case:** Decimal values, percentages, calculations.

```js
db.products.insertOne({
  price: 19.99,
  discount: 0.15
});
```

---

## 4. Decimal128

* **Definition:** High-precision decimal (128-bit) for financial and monetary data.
* **Use Case:** Accounting, currency calculations.

```js
db.transactions.insertOne({
  amount: NumberDecimal("1234.5678901234567890")
});
```

---

## 5. Boolean

* **Definition:** `true` or `false`.
* **Use Case:** Flags, status indicators.

```js
db.features.insertOne({
  isActive: true,
  isDeprecated: false
});
```

---

## 6. Date

* **Definition:** Stored as milliseconds since Unix epoch in UTC.
* **Use Case:** Timestamps, created/updated times.

```js
db.events.insertOne({
  event: "login",
  timestamp: new Date()
});
```

---

## 7. Null

* **Definition:** Represents a null value.
* **Use Case:** Explicit missing or cleared fields.

```js
db.profiles.insertOne({
  nickname: null
});
```

---

## 8. Array

* **Definition:** Ordered list of values, any type.
* **Use Case:** Lists of tags, embedded documents.

```js
db.posts.insertOne({
  title: "Hello World",
  tags: ["intro", "welcome", "mongodb"]
});
```

---

## 9. Object (Embedded Document)

* **Definition:** Nested documents (sub-objects) within a document.
* **Use Case:** Denormalized data, nested structures.

```js
db.orders.insertOne({
  orderId: 1001,
  customer: {
    name: "Bob",
    address: {
      street: "123 Main St",
      city: "Metropolis"
    }
  }
});
```

---

## 10. ObjectId

* **Definition:** 12-byte unique identifier default for `_id`.
* **Use Case:** Primary keys, unique document IDs.

```js
// Automatically generated:
const id = db.collection.insertOne({}).insertedId;
print(id);  // e.g. ObjectId("64a...fxyz")
```

---

## 11. Binary Data (BinData)

* **Definition:** Stores binary data as a byte array.
* **Use Case:** Files, images, encrypted data.

```js
// Example: store a small binary file
const data = new BinData(0, "AQIDBA==");
db.files.insertOne({ file: data });
```

---

## 12. Timestamp

* **Definition:** Special internal timestamp for replication (not the same as Date).
* **Use Case:** Oplog, Change Streams.

```js
// Typically used by MongoDB internally
// Visible via system collections like local.oplog.rs
```

---

## 13. Regular Expression (Regex)

* **Definition:** BSON type for regex patterns.
* **Use Case:** Pattern matching in queries.

```js
db.users.find({
  email: /@example\.com$/i
});
```

---

## 14. MinKey & MaxKey

* **Definition:** Special types to compare lower than all other BSON values (`MinKey`) or higher (`MaxKey`).
* **Use Case:** Query boundary markers.

```js
db.collection.find({ date: { $gt: MinKey() } });
```

---

## 15. JavaScript Code (Code) & Symbol (Deprecated)

* **Code:** Stores JavaScript code for server-side evaluation (`$where`).
* **Symbol:** Deprecated alias; rarely used.

```js
// Code example using $where (not recommended for performance)
db.users.find({ $where: "this.age > 25" });
```

---

*End of MongoDB Data Types Reference*
