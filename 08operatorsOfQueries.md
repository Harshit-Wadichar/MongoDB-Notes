# MongoDB Query Operators

This reference covers **comparison** and **logical** operators used in MongoDB queries. These operators help in filtering documents based on specified conditions.

---

## 1. Comparison Operators

Used to compare field values in queries.

| Operator | Description                   | Example                                |
| -------- | ----------------------------- | -------------------------------------- |
| `$eq`    | Equal to                      | `{ age: { $eq: 25 } }`                 |
| `$ne`    | Not equal to                  | `{ status: { $ne: "inactive" } }`      |
| `$gt`    | Greater than                  | `{ price: { $gt: 100 } }`              |
| `$gte`   | Greater than or equal to      | `{ score: { $gte: 90 } }`              |
| `$lt`    | Less than                     | `{ quantity: { $lt: 50 } }`            |
| `$lte`   | Less than or equal to         | `{ rating: { $lte: 4.5 } }`            |
| `$in`    | Matches any value in an array | `{ color: { $in: ["red", "blue"] } }`  |
| `$nin`   | Not in the array              | `{ category: { $nin: ["archived"] } }` |

### Example:

```js
db.products.find({
  price: { $gte: 50, $lte: 150 },
  category: { $ne: "discontinued" }
});
```

---

## 2. Logical Operators

Used to combine multiple query conditions.

| Operator | Description                                       | Example                                                     |
| -------- | ------------------------------------------------- | ----------------------------------------------------------- |
| `$and`   | Matches documents that satisfy **all** conditions | `{ $and: [ { age: { $gt: 25 } }, { status: "active" } ] }`  |
| `$or`    | Matches documents that satisfy **any** condition  | `{ $or: [ { name: "Alice" }, { name: "Bob" } ] }`           |
| `$not`   | Inverts the result of a condition                 | `{ age: { $not: { $gt: 65 } } }`                            |
| `$nor`   | Matches if **none** of the conditions are true    | `{ $nor: [ { status: "pending" }, { age: { $lt: 18 } } ] }` |

### Example:

```js
db.users.find({
  $or: [
    { role: "admin" },
    { loginAttempts: { $gt: 5 } }
  ]
});
```

---

## 3. Combining Comparison and Logical Operators

```js
// Find active users between ages 20 and 30

db.users.find({
  $and: [
    { status: "active" },
    { age: { $gte: 20, $lte: 30 } }
  ]
});
```

---

## 4. Nested Field Queries

Query inside embedded/nested documents using dot notation.

```js
// Find users where address.city equals "Mumbai"
db.users.find({ "address.city": "Mumbai" });
```

---

## 5. Array Queries with Comparison Operators

```js
// Find documents where any tag equals "urgent"
db.tasks.find({ tags: { $eq: "urgent" } });

// Find documents where array length is 3
// Not a direct operator, but use aggregation or custom logic
```

---

*End of MongoDB Query Operators Reference*
