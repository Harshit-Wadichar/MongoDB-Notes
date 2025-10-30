# MongoDB CLI in VS Code Terminal

A quick reference to manage MongoDB databases and collections directly from the VS Code integrated terminal.

---

## Prerequisites

* **MongoDB Shell**: Install the modern shell (`mongosh`) or legacy (`mongo`).
* **VS Code**: Ensure you have VS Code installed with terminal access.

---

## 1. Open the Integrated Terminal

Press <kbd>Ctrl</kbd>+<code>\`</code> (backtick) or select **Terminal → New Terminal** to open VS Code’s integrated terminal.

---

## 2. Connect to MongoDB Shell

### Local Instance

```bash
# Modern shell
mongosh

# Legacy shell
mongo
```

### Remote (MongoDB Atlas)

```bash
mongosh "mongodb+srv://<username>:<password>@<cluster-address>/"
```

Replace `<username>`, `<password>`, and `<cluster-address>` with your credentials.

---

## 3. View All Databases

```js
show dbs
```

Lists every database and its size.

---

## 4. Switch (Use) a Database

```js
use <databaseName>
```

Example:

```js
use myDatabase
```

Switches context to `myDatabase` (creates it when you first insert data).

---

## 5. Create a Collection

### Implicit Creation

```js
db.myCollection.insertOne({ name: "example", createdAt: new Date() });
```

### Explicit Creation

```js
db.createCollection("myCollection");
```

---

## 6. Delete (Drop) a Collection

```js
db.myCollection.drop();
```

Removes the specified collection and all its documents.

---

## 7. Delete (Drop) the Current Database

```js
db.dropDatabase();
```

Drops the database you are currently using, including all collections.

---

## Tips & Best Practices

* Always double-check which database you’re in (`db` prints the current database).
* Use **export/import** commands (`mongodump`/`mongorestore`) for backups before dropping.
* Combine commands in a `.mongodb` playground file for repeatable scripts.

---

*Happy coding!*
