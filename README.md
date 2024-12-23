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
Here's an advanced **Node.js + Express + MongoDB + Middleware + Microservices** cheat sheet to help you with key concepts and patterns.

---

## **Node.js**

### Key Features
- **Event Loop**: Asynchronous, non-blocking I/O.
- **Modules**: Use `require()` or ES6 `import/export`.
- **Process Management**: `process`, `child_process`, and worker threads for multithreading.
- **Streams**: Efficiently handle large files/data.
  ```javascript
  const fs = require('fs');
  const stream = fs.createReadStream('file.txt');
  stream.on('data', chunk => console.log(chunk));
  ```

---

## **Express.js**

### App Setup
```javascript
const express = require('express');
const app = express();
app.use(express.json()); // Middleware for JSON parsing
```

### Routes
- **Basic Routes**
  ```javascript
  app.get('/path', (req, res) => res.send('GET response'));
  app.post('/path', (req, res) => res.json(req.body));
  ```

- **Dynamic Parameters**
  ```javascript
  app.get('/user/:id', (req, res) => {
    const userId = req.params.id;
    res.send(`User ID: ${userId}`);
  });
  ```

- **Middleware**
  ```javascript
  const logger = (req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next();
  };
  app.use(logger);
  ```

- **Error Handling**
  ```javascript
  app.use((err, req, res, next) => {
    res.status(500).send({ error: err.message });
  });
  ```

---

## **MongoDB with Mongoose**

### Mongoose Setup
```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/dbname', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
```

### Schema and Model
```javascript
const userSchema = new mongoose.Schema({
  name: String,
  age: Number,
});
const User = mongoose.model('User', userSchema);
```

### CRUD Operations
- **Create**
  ```javascript
  const user = new User({ name: 'John', age: 30 });
  await user.save();
  ```

- **Read**
  ```javascript
  const users = await User.find({ age: { $gte: 18 } });
  ```

- **Update**
  ```javascript
  await User.updateOne({ name: 'John' }, { age: 31 });
  ```

- **Delete**
  ```javascript
  await User.deleteOne({ name: 'John' });
  ```

---

## **Middleware**

### Common Middlewares
- **Body Parser**
  ```javascript
  app.use(express.json());
  app.use(express.urlencoded({ extended: true }));
  ```

- **CORS**
  ```javascript
  const cors = require('cors');
  app.use(cors());
  ```

- **Custom Middleware**
  ```javascript
  const authenticate = (req, res, next) => {
    if (!req.headers.authorization) return res.status(403).send('Forbidden');
    next();
  };
  app.use(authenticate);
  ```

---

## **Microservices**

### Structure
- **Independent Services**: Each service has its own database, business logic, and API.
- **Communication**:
  - REST APIs
  - Message Brokers (RabbitMQ, Kafka)
  - Event-based (Socket.IO)

### Patterns
- **Service Discovery**: Use tools like Consul or Eureka.
- **API Gateway**: Manage requests and authentication centrally.
  ```javascript
  app.post('/gateway', (req, res) => {
    // Forward requests to the respective service
  });
  ```

- **Database per Service**: No shared DB schema.
- **Fault Tolerance**: Use circuit breakers like **Hystrix**.

---

## **Useful Snippets**

### Async/Await with Error Handling
```javascript
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

### Caching with Redis
```javascript
const redis = require('redis');
const client = redis.createClient();

client.set('key', 'value', redis.print);
client.get('key', (err, data) => console.log(data));
```

### Load Balancer (Using `cluster` module)
```javascript
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
  os.cpus().forEach(() => cluster.fork());
  cluster.on('exit', worker => cluster.fork());
} else {
  const app = express();
  app.listen(3000);
}
```

### JWT Authentication
```javascript
const jwt = require('jsonwebtoken');

const generateToken = user => jwt.sign({ id: user._id }, 'secret', { expiresIn: '1h' });

app.post('/login', (req, res) => {
  const token = generateToken(req.body.user);
  res.json({ token });
});

const verifyToken = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  jwt.verify(token, 'secret', (err, decoded) => {
    if (err) return res.status(401).send('Unauthorized');
    req.user = decoded;
    next();
  });
};
app.use(verifyToken);
```

---

## **Best Practices**

1. **Environment Variables**: Use `dotenv` for config management.
   ```javascript
   require('dotenv').config();
   ```

2. **Rate Limiting**: Prevent abuse with libraries like `express-rate-limit`.
   ```javascript
   const rateLimit = require('express-rate-limit');
   app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }));
   ```

3. **Validation**: Use `Joi` or `express-validator`.
   ```javascript
   const { body, validationResult } = require('express-validator');
   app.post('/data', [
     body('email').isEmail(),
   ], (req, res) => {
     const errors = validationResult(req);
     if (!errors.isEmpty()) return res.status(400).json(errors);
     res.send('Valid');
   });
   ```

4. **Logging**: Use `winston` or `morgan` for structured logging.
   ```javascript
   const morgan = require('morgan');
   app.use(morgan('combined'));
   ```

5. **Testing**: Use tools like `Jest` or `Mocha` for unit tests.

6. **Secure Headers**: Use `helmet` to set HTTP headers.
   ```javascript
   const helmet = require('helmet');
   app.use(helmet());
   ```

---

