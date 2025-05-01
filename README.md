# **📖 ULTIMATE NOSQL DATABASES README**  
**Master MongoDB, Cassandra, & Redis — From Setup to Scaling**  

---

## **🔍 1. What are NoSQL Databases?**  
### **Definition**  
NoSQL databases are **non-relational databases** designed for scalability, flexibility, and unstructured data. They include:  
- **Document Stores** (MongoDB)  
- **Column-Family Stores** (Cassandra)  
- **Key-Value Stores** (Redis)  

### **Use Cases**  
- **MongoDB**: Content management, real-time analytics.  
- **Cassandra**: Time-series data, high-write workloads (e.g., IoT).  
- **Redis**: Caching, session storage, real-time leaderboards.  

---

# **📦 MONGODB**  
**Document-Oriented Database**  

---

## **🔧 1. Installation & Setup**  
### **Step 1: Install MongoDB**  
- **Windows/macOS**: Download from [MongoDB Community Server](https://www.mongodb.com/try/download/community).  
- **Ubuntu**:  
  ```bash  
  wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -  
  echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list  
  sudo apt update && sudo apt install mongodb-org  
  ```  

### **Step 2: Start MongoDB**  
```bash  
sudo systemctl start mongod  
sudo systemctl enable mongod  
```  

### **Step 3: Connect via Shell**  
```bash  
mongosh  
```  

---

## **📊 2. Basic Usage**  
### **Create a Database**  
```javascript  
use mydb  
```  

### **Insert a Document**  
```javascript  
db.users.insertOne({  
  name: "John",  
  age: 30,  
  hobbies: ["coding", "gaming"]  
});  
```  

### **Query Documents**  
```javascript  
db.users.find({ age: { $gt: 25 } });  
```  

---

## **⚡ 3. Intermediate Skills**  
### **Indexing**  
```javascript  
db.users.createIndex({ age: 1 });  
```  

### **Aggregation Pipeline**  
```javascript  
db.orders.aggregate([  
  { $match: { status: "completed" } },  
  { $group: { _id: "$product", total: { $sum: "$price" } } }  
]);  
```  

---

## **🚀 4. Advanced Techniques**  
### **Sharding**  
1. **Start Config Server**:  
   ```bash  
   mongod --configsvr --replSet configRS --port 27019  
   ```  
2. **Add Shards**:  
   ```bash  
   mongod --shardsvr --replSet shard1 --port 27018  
   ```  

### **Replication**  
```bash  
mongod --replSet "rs0" --port 27017  
```  

---

## **📚 5. Resources**  
- [MongoDB University](https://learn.mongodb.com/) (Free courses)  
- [MongoDB Documentation](https://www.mongodb.com/docs/)  

---

# **🌩️ APACHE CASSANDRA**  
**Column-Family Database**  

---

## **🔧 1. Installation & Setup**  
### **Step 1: Install Cassandra**  
- **Ubuntu**:  
  ```bash  
  echo "deb https://downloads.apache.org/cassandra/debian 40x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list  
  sudo apt update && sudo apt install cassandra  
  ```  

### **Step 2: Start Cassandra**  
```bash  
sudo systemctl start cassandra  
sudo systemctl enable cassandra  
```  

### **Step 3: Connect via CQL Shell**  
```bash  
cqlsh  
```  

---

## **📊 2. Basic Usage**  
### **Create Keyspace & Table**  
```sql  
CREATE KEYSPACE mykeyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};  
USE mykeyspace;  
CREATE TABLE users (id UUID PRIMARY KEY, name TEXT, age INT);  
```  

### **Insert Data**  
```sql  
INSERT INTO users (id, name, age) VALUES (uuid(), 'Alice', 28);  
```  

### **Query Data**  
```sql  
SELECT * FROM users WHERE age > 25;  
```  

---

## **⚡ 3. Intermediate Skills**  
### **Tuning Consistency Levels**  
```sql  
CONSISTENCY QUORUM;  
```  

### **Batch Operations**  
```sql  
BEGIN BATCH  
  INSERT INTO users (...) VALUES (...);  
  UPDATE users SET age = 30 WHERE id = ...;  
APPLY BATCH;  
```  

---

## **🚀 4. Advanced Techniques**  
### **Multi-Node Cluster Setup**  
1. Edit **cassandra.yaml** on each node:  
   ```yaml  
   cluster_name: 'MyCluster'  
   seed_provider: [ "IP1", "IP2" ]  
   listen_address: <node-ip>  
   ```  

### **Time-to-Live (TTL)**  
```sql  
INSERT INTO users (id, name) VALUES (uuid(), 'Bob') USING TTL 86400;  
```  

---

## **📚 5. Resources**  
- [Cassandra Documentation](https://cassandra.apache.org/doc/latest/)  
- [DataStax Academy](https://academy.datastax.com/)  

---

# **⚡ REDIS**  
**In-Memory Key-Value Store**  

---

## **🔧 1. Installation & Setup**  
### **Step 1: Install Redis**  
- **Ubuntu**:  
  ```bash  
  sudo apt install redis-server  
  ```  
- **macOS**:  
  ```bash  
  brew install redis  
  ```  

### **Step 2: Start Redis**  
```bash  
sudo systemctl start redis  
redis-cli  
```  

---

## **📊 2. Basic Usage**  
### **Set/Get Key-Value**  
```bash  
127.0.0.1:6379> SET user:1 "John"  
127.0.0.1:6379> GET user:1  
```  

### **Lists & Hashes**  
```bash  
127.0.0.1:6379> LPUSH tasks "task1"  
127.0.0.1:6379> HSET user:1 name "Alice" age 30  
```  

---

## **⚡ 3. Intermediate Skills**  
### **Pub/Sub Messaging**  
```bash  
# Subscriber  
SUBSCRIBE notifications  

# Publisher  
PUBLISH notifications "Hello, world!"  
```  

### **Expire Keys**  
```bash  
EXPIRE user:1 3600  # Key expires in 1 hour  
```  

---

## **🚀 4. Advanced Techniques**  
### **Persistence (RDB/AOF)**  
- **RDB Snapshotting**:  
  ```bash  
  save 900 1  # Save if 1+ keys change in 15m  
  ```  
- **AOF Logging**:  
  ```bash  
  appendonly yes  
  appendfsync everysec  
  ```  

### **Redis Cluster**  
```bash  
redis-cli --cluster create 192.168.1.1:7000 192.168.1.2:7000 --cluster-replicas 1  
```  

---

## **📚 5. Resources**  
- [Redis Commands](https://redis.io/commands/)  
- [Redis University](https://university.redis.com/)  

---

## **❓ FAQ**  
**Q: When to use MongoDB vs. Cassandra vs. Redis?**  
- **MongoDB**: Flexible JSON docs, medium-scale apps.  
- **Cassandra**: High write-throughput, global scaling.  
- **Redis**: Real-time data, caching, pub/sub.  

**Q: How to secure Redis?**  
```bash  
# Set a password in redis.conf  
requirepass mypassword  
```  

---

## **🎯 Final Tips**  
✅ **MongoDB**: Use compound indexes for query optimization.  
✅ **Cassandra**: Design tables for query patterns (denormalize!).  
✅ **Redis**: Monitor memory usage with `INFO MEMORY`.  

---
