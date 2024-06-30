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

## Modifying Query Results
### Cursor 
* Pointer to the result set of a query
* db.<collection>.find() results a cursor
* Cursor methods
  * Chained to queries
  * perform actions on the resulting set ( sorting / limiting )

### Sorting
```javascript
db.cars.find({category: "sedan"}).sort({name: 1}); // Capital letters sorted first, then normal letters sorted. Can change this in options 
```

### Limiting 
```javascript
db.cars.find({category: "sedan"}).sort({sales: -1}).limit(3); // Improves Performance by avoiding unnecessary data operations
```

### Projection
```javascript
db.cars.find({}, {field: 1});
db.cars.find({}, {field: 0});
// Either inclusion approach or exclusion approach. Both cannot be mixed in one projection, with the exception of _id.
// _id is always included by default, explicit exclusion is required. 
```

### Counting
```javascript
db.cars.countDocuments(query, options);
db.cars.countDocuments();
db.cars.countDocuments({category: "sedan"});
```

## CRUD Operations using Java Driver
### Documents
* Java driver supports many classes for BSON objects
* Document class is recommended since its flexible and concise data representation
```javascript
Document car = new Document("_id", new ObjectId()) // If ID not given, it will automatically generate 
    .append("name": "Kodiaq")
    .append("manufacturer": "Skoda")
    .append("category": "SUV")
    .append("year": new Date();
```

### Inserting 
```javascript
//Inserting One Document
InsertOneResult result = collection.insertOne(car);
BsonValue id = result.getInsertedId();
System.out.println(id);


//Inserting Many documents
List<Document> cars = Arrays.asList(car1, car2);
InsertManyResult result = collection.insertMany(cars);
result.getInsertedIds().forEach((x, y) -> System.out.println(y.asObjectId())); // x - index of the item inserted, y - ID of the item inserted
```

### Searching
```javascript
collection.find(Filters.and(Filters.gte("balance": 1000), Filters.eq("account_type", "checking"))).forEach(doc -> System.out.println(doc.toJson()));
// Same as
// collection.find(and(gte("balance": 1000), eq("account_type", "checking"))).forEach(doc -> System.out.println(doc.toJson()));
// Filters builder class helps us use query predicates


// In case processing of each document is required:
try(MongoCursor<Document> cursor = collection.find(Filters.and(Filters.gte("balance": 1000), Filters.eq("account_type", "checking"))).iterator()) {
    while(cursor.hasNext()) {
        System.out.println(cursor.next().toJson());
    }
}

// In case only one document is required
Document doc = collection.find(Filters.and(Filters.gte("balance": 1000), Filters.eq("account_type", "checking"))).first(); 
System.out.println(doc.toJson());
```

### Updating 
```javascript
// Single Update Set Operation
Bson query = Filters.eq("account_id", 1);
Bson updates = Updates.combine(Updates.set("account_status", "active"), Updates.inc("balance", 100));
UpdateResult result = collection.updateOne(query, updates);

// Single update array operations
Bson query = Filters.eq("account_id", 1);
Bson updates = Updates.combine(Updates.push("loans", "new_loan"), Updates.pull("loans", "last_loan"), Updates.popLast());
UpdateResult result = collection.updateOne(query, updates);

// Multiple Updates operations
Bson query = Filters.eq("account_type", "savings");
Bson update = Updates.inc("balance", 1000);
UpdateResult result = collection.updateMany(query, updates);
```

### Deleting 
```javascript
// Deleting single document
Bson query = Filters.eq("account_name", "Nitesh");
DeleteResult result = collection.deleteOne(query);
System.out.println(result.getDeletedCount());

// Deleting many documents
Bson query = Filters.eq("account_status", "dormant");
DeleteResult result = collection.deleteMany(query);
System.out.println(result.getDeletedCount());
```

### Transactions
* Multi-document transaction is an operation that requires atomicity of read/write operations
* It is a sequence of operations that represent a single unit of work
* If the transaction is cancelled or fail to complete, all write operations are discarded
* ACID Compliance
  * Atomicity - All or nothing operations,
  * Consistency - Correctness of database,
  * Isolation - Multiple transactions can occur at the same time without breaking the database,
  * Durability - All the updates are stored to a disk permanently and no system failure should affect the database)
* Steps
  1. Start a client session
  2. Define transaction options ( optional )
  3. Define sequence of operations
  4. Start the transaction using the withTransaction() method
  5. Release the resources used by the transaction
> Transactions have a 60 second timer limit and will cancel anything more
```javascript
final MongoClient client = MongoClients.create(connectionString);
final ClientSession clientSession = client.startSession(); // Starting a client session

TransactionBody txnBody = new TransactionBody<String>(){ // Defining sequence of operations
    public String execute() { // All CRUD operations inside this body
        MongoCollection<Document> bankingCollection = client.getDatabase("bank").getCollection("accounts");

        Bson fromAccount = eq("account_id", "MDB310054629"); // Withdrawing 200 from one account
        Bson withdrawal = Updates.inc("balance", -200);

        Bson toAccount = eq("account_id", "MDB643731035"); // Depositing 200 in another account
        Bson deposit = Updates.inc("balance", 200);

        System.out.println("This is from Account " + fromAccount.toBsonDocument().toJson() + " withdrawn " + withdrawal.toBsonDocument().toJson());
        System.out.println("This is to Account " + toAccount.toBsonDocument().toJson() + " deposited " + deposit.toBsonDocument().toJson());
        bankingCollection.updateOne(clientSession, fromAccount, withdrawal); // Updating withdrawal
        bankingCollection.updateOne(clientSession, toAccount, deposit); // Updating deposit

        return "Transferred funds from John Doe to Mary Doe";
    }
};

try {
    clientSession.withTransaction(txnBody); // Starting the transaction
} catch (RuntimeException e){
    System.out.println(e); 
}finally{
    clientSession.close(); // Releasing resources
}
```

## Aggregation 
### Aggregation 
* Analysis and summary of data, broken into stages
* Stage is an aggregation operation performed on the data. Doesn't permanently alter the source data
* Aggregation pipeline is a series of stages completed one at a time in order
* Filtered -> Sorted -> Grouped -> Transformed
* Documents that are output from one stage, become the input for the next stage
```javascript
// Structure of an aggregation pipeline
db.collection.aggregate([
    {
        $stage1: {
            { expression1 },
            { expression2 }...
        },
        $stage2: {
            { expression1 }...
        }
    }
])
```

### Aggregation Operations
* $match
```javascript
// Place as early as possible so that it can use indexes. It reduces the number of documents, hence processing. 
db.cars.aggregate([
    {$match: {"name": "Kodiaq"}}
]);
```
* $group
```javascript
// Groups documents by a group key. Output is one document for each unique value of group key
db.<collection>.aggregate([
    {
        $group:
            {
                _id: <expression>, // Group Key
                <field>: { <accumulator> : <expression> }
            }
    }
]);

db.cars.aggregate([
    {
        $group:
            {
                _id: "$name", // Group Key
                totalNoOfCars: { $count : {} }
            }
    }
]);
```
* $sort
```javascript
// Sorts numeric values, strings, dates, timestamps
// 1 : Ascending, -1 : Descending

db.cars.aggregate([
    {
        $sort:
            {
                sales: -1
            }
    }
])
```
* $limit
```javascript
db.cars.aggregate([
    {
        $sort:
            {
                sales: -1
            }
    },
    {
        $limit: 3
    }        
]);
```
* $project
```javascript
// Usually the last stage
// Can include, exclude or set new fields

db.cars.aggregate([
    {
        $project:
            {
                sales: 1, nameOfCar: "$name", id: 0
            }
    }
]);
```
* $set
```javascript
db.cars.aggregate([
    {
        $set:
            {
                sales2024: {$round: { $multiply: [1.0012, "$sales"]}}
            }
    }
]);
```
* $count
```javascript
// Returns a single document with the count value 
db.cars.aggregate([
    {
        $count: "total_cars"
    }
]);
```
* $out
```javascript
// Writes the documents that are returned by an aggregation pipeline into a collection
// Creates a new collection if it doesn't exist
// If collection exists, it gets overwritten
// If the database doesn't exist, it gets created
// If no database is mentioned, the same database is used

db.<collection>.aggregate([
    { $out: {
        db: "<db>",
        coll: "<newcollection>"
    }}
])

db.<collection>.aggregate([
    { $out: "<newcollection>"}
])

db.cars.aggregate([
    {
        $sort:
            {
                sales: -1
            }
    },
    {
        $limit: 3
    },
    {
        $out: "highest_sales"
    }     
]);

// Run "show collections" to list out the collections and verify
```

## Aggregation with Java Driver
```
private static void matchAndGroup(MongoCollection<Document> cars) {
    Bson matchStage = Aggregate.match(Filters.eq("manufacturer": "Skoda"));
    Bson groupStage = Aggregate.group("$category": "SUV", sum("sales");
    cars.aggregate(Arrays.asList(matchStage, groupStage)).forEach(doc -> System.out.println(doc.toJson()));
}

private static void matchSortAndProjectStages(MongoCollection<Document> accounts){
    Bson matchStage =
            Aggregates.match(Filters.and(Filters.gt("balance", 1500), Filters.eq("account_type", "checking")));
    Bson sortStage = Aggregates.sort(Sorts.orderBy(descending("balance")));
    Bson projectStage = Aggregates.project(Projections.fields(Projections.include("account_id", "account_type", "balance"), Projections.computed("euro_balance", new Document("$divide", asList("$balance", 1.20F))), Projections.excludeId()));
    System.out.println("Display aggregation results");
    accounts.aggregate(asList(matchStage,sortStage, projectStage)).forEach(document -> System.out.print(document.toJson()));
}
```



   


