Here’s a concise 3-page MongoDB cheat sheet for quick reference, ideal for including in a `README` file:

---

# MongoDB Cheat Sheet

## 1. **Core MongoDB Commands**

### **Database Operations**
```bash
show dbs                     # List all databases
use <dbName>                 # Switch to a database
db                           # Current database
db.dropDatabase()            # Delete the current database
```

### **Collection Operations**
```bash
show collections             # List all collections
db.createCollection("name")  # Create a collection
db.<collection>.drop()       # Delete a collection
```

---

## 2. **CRUD Operations**

### **Create**
```javascript
db.collection.insertOne({ key: "value" })               // Insert a single document
db.collection.insertMany([{ key: "value1" }, { key: "value2" }]) // Insert multiple documents
```

### **Read**
```javascript
db.collection.find({ key: "value" })                   // Find documents with matching criteria
db.collection.find().pretty()                          // Format output
db.collection.findOne({ key: "value" })                // Find one document
```

### **Update**
```javascript
db.collection.updateOne({ key: "value" }, { $set: { key: "newValue" } }) // Update one document
db.collection.updateMany({ key: "value" }, { $set: { key: "newValue" } }) // Update multiple documents
```

### **Delete**
```javascript
db.collection.deleteOne({ key: "value" })              // Delete one document
db.collection.deleteMany({ key: "value" })             // Delete multiple documents
```

---

## 3. **Query Operators**

### **Comparison**
```javascript
$eq    // Equal: { key: { $eq: value } }
$ne    // Not Equal
$gt    // Greater Than
$lt    // Less Than
$gte   // Greater Than or Equal
$lte   // Less Than or Equal
```

### **Logical**
```javascript
$and   // And: { $and: [{ key1: value1 }, { key2: value2 }] }
$or    // Or: { $or: [{ key1: value1 }, { key2: value2 }] }
$not   // Not
$nor   // Nor
```

---

## 4. **Indexing & Aggregation**

### **Indexing**
```javascript
db.collection.createIndex({ key: 1 })                  // Create ascending index
db.collection.dropIndex({ key: 1 })                    // Drop index
db.collection.getIndexes()                             // List all indexes
```

### **Aggregation**
```javascript
db.collection.aggregate([
  { $match: { key: "value" } },                        // Filter documents
  { $group: { _id: "$field", total: { $sum: 1 } } },   // Group and calculate
  { $sort: { total: -1 } }                             // Sort results
])
```

---

## 5. **Utilities**

### **Backup & Restore**
```bash
mongodump --db <dbName> --out <backupDir>              # Backup database
mongorestore <backupDir>                               # Restore database
```

### **Shell Helpers**
```bash
it                        # Iterate results in shell
cls                       # Clear screen
```

---

## 6. **Mongoose Basics (Node.js)**

### **Setup**
```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/mydb');
```

### **Schema & Model**
```javascript
const schema = new mongoose.Schema({ key: String });
const Model = mongoose.model('CollectionName', schema);
```

### **CRUD**
```javascript
Model.create({ key: 'value' });                      // Create
Model.find({ key: 'value' });                        // Read
Model.updateOne({ key: 'value' }, { key: 'newValue' }); // Update
Model.deleteOne({ key: 'value' });                   // Delete
```

---
