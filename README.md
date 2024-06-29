# MongoDB
## MongoDB Atlas
### Organisations 
* Group and define users and teams
* Grant access to projects

### Projects
* Define and organise resources
* Note: You can use separate projects for development, testing and production

### Creating Database 
1. Build a Database
2. Choose Cloud Provider, Region and Cluster name
3. By default, no user or IP address is configured
4. Create Admin User, Add Current IP.
5. Load Sample Dataset ( Not Required )

### Free-tier Shared Cluster Cloud Providers
1. Amazon Web Services
2. Google Cloud
3. Microsoft Azure

### Atlas Data Explorer 
* Interact and manage data from Atlas UI.
* Create, View, Update - Databases, Collections, Documents

## MongoDB and Document Model
### MongoDB
* General Purpose Document Database
* Stores data in Documents
* Key-value, text, geospatial, time-series, graphs
* Scalability, Resilience, Speed of Development, Privacy and Security
* Document: Basic unit of data
* Collection: Grouping of similar documents ( need not be of same format )
* Database: A container for collection
* Flexible Schema Model
 MongoDB database is at the core of Atlas, which has many more features like full-text search or data visualisations ( multi-cloud developer data platform )

### Document Model
* Displayed in JSON, stored in BSON ( Binary JSON )
* Strings, Objects, Arrays, Boolean, Null, Dates, Numbers, Object IDs, and more
* Object ID - Unique Identifier - Created by default if not mentioned manually *
* Documents contain many fields, and fields could contain many data types in different documents ( Polymorphic Documents )
* Optional Schema Validation available

## Data Modeling
* Process of defining data and their relationships
* Easier to manage data,  make queries efficient, use less memory and CPU, reduce costs
* Data accessed together, should be stored together
* Flexible document data model ( Polymorphic ) - Schema Flexible
* One-to-One, One-to-Many, Many-to-Many
* Embedding ( inserting related data into the document ), Referencing ( referring documents in another colection in the document )
```javascript
// One-to-One
{
    "id": ObjectID("1"),
    "title": "Star Wars", 
    "director": "Lucas",
    "runtime": 121
}

// One-to-Many
{
    "id": ObjectID("1"),
    "title": "Star Wars",
    "director": "Lucas",
    "runtime": 121,
    "cast": ['abc', 'def']
}

// Referencing
{
    "id": ObjectID("1"),
    "title": "Star Wars",
    "director": "Lucas",
    "runtime": 121,
    "cast": [ObjectId(123), ObjectId(456)]
}
```

### Embedding 
Advantages
* Avoids Application Joins
* Provides better read performances
* Can update related data in a single write/update/delete operation
  
Disadvantages - Schema Anti-Patterns
* Can create large documents - latency - slow responses
* Unbounded documents - exceeding document size ( 16 mb - bson limit )
* Data Duplication

### Referencing ( Data Linking, Data Normalisation ) 
Advantages 
* Simple and Sufficient for most cases
* No duplication
* Smaller Documents
  
Disadvantages
* Need to join data from multiple documents
* Need to access different documents to access data increasing cost of queries

### Schema Design Patterns
* Help plan, organise and model data
* Anti-patterns - sub-optimal performance, scalability problems
  1. Masssive Arrays
  2. Massive number of collections
  3. Bloated Documents
  4. Unnecessary Indexes
  5. Queries without Indexes
  6. Data accessed together, but not stored together
* Data Explorer
  1. Free Tier
  2. Shows Schema Anti-Patterns
  3. Collection and index stats for each collection
* Performance Advisor
  1. M10 Tier and higher
  2. Analyses most active collections
  3. Recommends schema improvements
 
## Connecting to MongoDB
### Ways of connecting: 
* Shell
* Compass
* Applications

### Connection String: 
* Standard Format - Standalone clusters, Replica Sets, Sharded Clusters
* DNS Seed list Format - provide DNS server list, change servers in rotation, more flexible

### Connecting String Format
```
mongodb+srv://<username>:<password>@cluster0.usqsf.mongodb.net/?retryWrites=true&w=majority
```
* mongodb+srv: DNS SeedList Format, TLS Security true
* mongodb: Standard Formrat
* <username>:<password>: Username and password for database
* @cluster0.usqf.mongodb.net/: host : optional port ( default: 27017 )
* ?options

### MongoDB Connection with Shell
* Steps
  1. Database
  2. Connect
  3. Connect with Shell
  4. Copy Paste Connection String onto MongoDb Shell
  5. Enter password
* Node Js Repl Environment
  1. JS Variables
  2. JS Functions
  3. JS Conditionals
  4. JS Loops
  5. JS Control Flow Statements

### MongoDB Connection with Compass
* GUI to query, analayse data
* Steps
  1. Database
  2. Connect
  3. Connect with Compass
  4. Open Compass
  5. New Connection
  6. Toggle Edit Connection String 
  7. Copy paste Connection String
  8. Save and Connect
  9. Name Cluster

### MongoDB Connection with Application
* MongoDB Drivers connect application to database
* Drivers
  1. C, C#, c++
  2. Go
  3. Rust
  4. Java
  5. Node JS
  6. PHP
  7. Python
  8. Ruby
  9. Swift
  10. Scala
* Information About Drivers
  1. Documentation
  2. MongoDB Development Center
  3. MongoDB University


### Troubleshooting Connection Errors
* Add IP Address in Network Access Panel in Atlas
```
MongoServerSelectionError: connection <monitor> to 34.239.188.169:27017 closed
```
* Check Correct Database deployment / Check Credentials
```
MongoServerError: bad auth : Authentication failed.
```

## Connecting to MongoDB using Java
### Require Java drivers for:
 1. Asynchronous Code
 2. Synchronous Code

### Drivers take care of:
 1. Simplify connecting and interacting with database
 2. Establish secure connections
 3. Execute database operations on behalf of client applications
 4. Specify connection options

### Official MongoDB Drivers:
 1. Adhere to language best practices
 2. Full functionality of MongoDB deployment
 3. Make upgrading easier

### Connection Steps
1. Add MongoDB driver to Dependencies in pom.xml file ( Can use Maven or Gradle Build System to add the dependency )
2. Obtain valid connection string
```javascript
ConnectionString connectionString = new ConnectionString(mongodb+srv://<username>:<password>@cluster0.usqsf.mongodb.net/
    ?retryWrites=true&w=majority);
MongoClientSettings settings = MongoClientSettings.builder()
    .applyConnectionString(connectionString)
    .serverApi(ServerApi.builder())
        .version(ServerApiVersion.V1)
        .build())
    .build();
MongoClient mongoClient = MongoClient.create(settings);
```
> Application should use single mongo client instance for all database requests
> Instantiating a mongo client instance is very resource expensive, so should avoid creating more than one

### Common Issues when connecting to Atlas Cluster
1. Atlas IP access restrictions
2. Invalid Connection string format ( Unencoded )
3. Incorrect Authentication
4. Firewall Misconfiguarion ( Default port blocked - 27017 )
5. Unclosed Connections

### Troubleshooting - Too many open Connections
1. Restart application
2. Closing any open connections that are not in use
3. Scaling your cluster to a higher tier that supports more connections
4. Limiting the number of connections in connection pool using maxPoolSize connection string option


## CRUD Operations
### Insertion
* insertOne()
```javascript
db.<collection>.insertOne();
```
> If <collection> doesn't exist, it'll create before inserting.

> Returns acknowledge: true, insertedId: ObjectId()
* insertMany()
```javascript
db.<collection>.insertMany([
    <document1>,
    <document2>,
    <document3>
]);
```
> If <collection> doesn't exist, it'll create before inserting.

> Returns acknowledged: true, insertedIds: [ObjectId(), ObjectId(), ObjectId()]

### Searching
* find()
```javascript
db.<collection>.find();
db.<collection>.find({state: {$eq: 'TN'}}); // db.<collection>.find({state:'TN'}) 
db.<collection>.find({state: {$in: ['TN', 'GJ']}});
db.<collection>.find({"state.pop": {$gt: 10000}});
db.<collection>.find({"state.pop": {$gte: 10000}});
db.<collection>.find({"state.pop": {$lt: 10000}});
db.<collection>.find({"state.pop": {$lte: 10000}});
db.<collection>.find({products: {$elemMatch: {$eq: "InvestmentStock"}}}); // db.<collection>.find({products: {$eq: "InvestmentStock"}})
//returns any document with InvestmentStock in it, be it an element of an array or not
db.<collection>.find({$and: [{<expression1>}, {<expression2>}]}); // db.<collection>.find({<expression1>, <expression2>});
db.<collection>.find({$or: [{<expression1>}, {<expression2>}]});
// If using nested 'and' and 'or' operators, use explicit 'and' else expressions get replaced by the most recent one
```
* findOne()

### Replacing and Deleting
* replaceOne()
```javascript
db.<collection>.replaceOne(filter, replacement, options); // replacement has everything except the _id field
```
> Returns acknowledged: true, insertedId: , matchedCount: , modifiedCount: , upsertedCount:

* updateOne()
```javascript
db.<collection>.updateOne(filter, update, options);
```
> $set - Adds new fields/Replaces existing fields

> $push - Appends value to existing array/Creates new array and appends

> Upsert - Inserts new document if no such existing document ( Add {upsert: true} as the options field in the query )

* findAndModify()
```javascript
db.<collection>.findAndModify({query: , update: , new: , upsert: }) // new makes sure that the updated document is returned, which isn't default behaviour. upsert false by default. 
```
> Replaces updateOne() + findOne(). Avoids two round trips and no other user modifications will change the accurate output

* updateMany()
```javascript
db.<collection>.updateMany(filter, update, options);
```
> Not an all-or-nothing operation, won't rollback in case of errors

> Lacks Isolation, ie, updates are visible right as they have been made, hence not always appropriate like transactions

* deleteOne()
```javascript
db.<collection>.deleteOne(filter);
```
> Returns acknowledgement: true, and deletedCount:

* deleteMany()
```javascript
db.<collection>.deleteMany(filter);
```

### Modifying Query Results

   


