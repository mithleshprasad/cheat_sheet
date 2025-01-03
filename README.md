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
Designing an **e-commerce system** involves handling multiple components like inventory, payment, user management, and recommendations. Below is the structured breakdown of the system, including architecture and database schemas:

---

### **1. High-Level Architecture**

**Core Components:**
1. **Frontend (Client)**
   - User-facing interface: web, mobile, or both.
   - Allows users to browse products, view carts, place orders, and manage accounts.

2. **Backend (Server)**
   - Manages APIs, business logic, and communication between components.
   - Handles core functionalities like user management, inventory, payment, and recommendations.

3. **Databases**
   - Separate databases for user data, product catalog, orders, and recommendations for scalability.

4. **External Services**
   - Payment Gateway Integration (e.g., Stripe, PayPal).
   - Notification Services (e.g., email/SMS for order updates).
   - CDN for static content delivery (e.g., Cloudflare).

5. **Infrastructure**
   - Load balancers for distributing traffic.
   - Caching for fast responses (e.g., Redis, Memcached).
   - Message queues for asynchronous tasks (e.g., RabbitMQ, Kafka).

---

### **2. Detailed Subsystems**

#### **(a) Inventory Management**
   - **Purpose**:
     - Tracks product stock levels, SKU (Stock Keeping Unit), and availability.
     - Synchronizes stock levels during orders, cancellations, and returns.
   - **Features**:
     - Real-time stock updates.
     - Support for warehouses (multi-location inventory).
   - **Database Schema**:
     ```plaintext
     Table: Products
     - product_id (PK)
     - name
     - description
     - price
     - stock_quantity
     - category_id (FK)
     - warehouse_id (FK)
     - created_at
     - updated_at

     Table: Warehouses
     - warehouse_id (PK)
     - location
     - manager
     - created_at
     ```

---

#### **(b) Payment Management**
   - **Purpose**:
     - Securely process payments and handle refunds.
     - Support multiple payment methods (credit/debit cards, wallets, UPI, etc.).
   - **Features**:
     - Payment confirmation.
     - Fraud detection.
     - Payment status tracking.
   - **Integration**:
     - Use third-party APIs like Stripe, Razorpay, or PayPal.
   - **Database Schema**:
     ```plaintext
     Table: Payments
     - payment_id (PK)
     - order_id (FK)
     - user_id (FK)
     - payment_status (Pending, Success, Failed)
     - payment_method (Credit Card, UPI, Wallet)
     - amount
     - transaction_id
     - created_at
     ```

---

#### **(c) User Management**
   - **Purpose**:
     - Manages user accounts, roles, and preferences.
   - **Features**:
     - User authentication and authorization.
     - Role-based access control (e.g., Admin, Customer).
   - **Database Schema**:
     ```plaintext
     Table: Users
     - user_id (PK)
     - name
     - email (unique)
     - hashed_password
     - phone
     - address (JSON or normalized)
     - role (Admin, Customer)
     - created_at

     Table: Addresses
     - address_id (PK)
     - user_id (FK)
     - street
     - city
     - state
     - postal_code
     - country
     ```

---

#### **(d) Recommendation System**
   - **Purpose**:
     - Provide personalized product suggestions to users.
   - **Techniques**:
     - Collaborative filtering.
     - Content-based filtering.
     - Popularity-based recommendations.
   - **Workflow**:
     - Data collection: user browsing and purchase history.
     - Use machine learning to predict user preferences.
   - **Database Schema**:
     ```plaintext
     Table: Recommendations
     - recommendation_id (PK)
     - user_id (FK)
     - product_id (FK)
     - score (relevance score)
     - created_at
     ```

---

### **3. System Design Workflow**

#### **User Journey**
1. **Browse Products**:
   - Fetch products from the inventory database via REST or GraphQL API.
   - Use caching to reduce response time for frequent queries.

2. **Add to Cart**:
   - Temporarily reserve stock.
   - Update cart data in the session or a dedicated table.

3. **Checkout and Payment**:
   - Initiate a payment transaction using a payment gateway API.
   - Deduct stock quantity after successful payment.

4. **Order Confirmation**:
   - Generate an order record with a unique identifier.
   - Send order confirmation via email/SMS.

5. **Recommendations**:
   - Show personalized product suggestions based on user activity and purchase history.

---

### **4. API Endpoints**

| Endpoint                  | Method | Description                  |
|---------------------------|--------|------------------------------|
| `/products`               | GET    | Fetch list of products.      |
| `/cart`                   | POST   | Add item to cart.            |
| `/checkout`               | POST   | Initiate payment.            |
| `/orders/{order_id}`      | GET    | Fetch order details.         |
| `/users/{user_id}`        | GET    | Get user profile.            |
| `/recommendations/{user}` | GET    | Get personalized suggestions.|

---

### **5. Scalability and Performance**

- **Database Optimization**:
  - Indexing frequently queried fields (e.g., `product_id`, `user_id`).
  - Use read replicas for scaling read-heavy operations.
- **Caching**:
  - Cache frequently viewed products (e.g., Redis).
- **Asynchronous Processing**:
  - Use message queues for tasks like sending emails or generating recommendations.
- **CDN**:
  - Serve static assets like images and CSS from a CDN.

---

### **6. Example High-Level Diagram**

```
[Client (Web/Mobile)]
        |
   [API Gateway]
        |
[Microservices: Inventory, User, Payment, Recommendation]
        |
[Databases: Products, Users, Orders, Recommendations]
        |
[Third-Party Services: Payment Gateway, CDN, Notification Service]
```

Would you like detailed implementation for any specific component?
