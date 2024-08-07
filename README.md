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
// To get all the documents from the cursor
db.<collection>.find().toArray();
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
* $lookup
```javascript
// This query will output documents from the orders collection with an additional field called customer_info that contains an array of matched documents from the customers collection.
// Doesn't actually add any fields to the original documnent, only adds the additional fields to the pipeline result set
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",        // the collection to join
      localField: "customer_id", // field from the orders collection
      foreignField: "customer_id", // field from the customers collection
      as: "customer_info"       // name of the new array field to add to the orders documents
    }
  }
]);
```

## Aggregation with Java Driver
```javascript
Example 1
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

Example 2
AggregateIterable<Document> result = ordersCollection.aggregate(Arrays.asList(
    Aggregates.match(Filters.eq("status", "A")), // $match
    Aggregates.lookup("customers", "customer_id", "customer_id", "customer_info"), // $lookup
    Aggregates.unwind("$customer_info"), // Unwind the array to deconstruct the array field
    Aggregates.group("$customer_info.region", // $group
        Accumulators.sum("totalAmount", "$amount"),
        Accumulators.addToSet("orders", "$$ROOT")
    ),
    Aggregates.sort(Sorts.descending("totalAmount")), // $sort
    Aggregates.limit(5), // $limit
    Aggregates.project(Projections.fields( // $project
        Projections.excludeId(),
        Projections.include("totalAmount"),
        Projections.include("orders")
    )),
    Aggregates.set(new Document("reportDate", new java.util.Date())), // $set
    Aggregates.count("totalRegions"), // $count
    Aggregates.out("aggregationResults") // $out
));

// Iterate over the result
for (Document doc : result) {
    System.out.println(doc.toJson());
}
```

## Indexes
### Indexes
* Special data structures
* Store small portion of data
* Ordered and easy to search efficiently
* Benefits
  * Speed up Queries
  * Reduce Disk I/O
  * Reduce resources required
  * Support equality matches, range based, and sorted results
* Without Indexes
  * MongoDB reads all documents ( collection scan )
  * Sorts results in memory
* With Indexss
  * Only fetches documents identifies by the index based on the query
  * Returns results faster
* Default Index : includes only the _id field
* Every query should use an index
* Inserting/Updating documents -> Updating index data structure
* Too many indexes -> slower performance since every update requires an index update as well
* Delete unnecessary or redundant indexes
* Types
  * Single Field - includes only single field
  * Compound - includes more than one field
  * Multikey - if any one field is an array field

### Single Field Indexes
* Support queries and sorting on a single field
```javascript
db.<collection>.createIndex({field: 1}) // Ascending
db.<collection>.createIndex({field: -1}) // Descending
// Ascending/Descending doesn't matter in single field since both of them can be used for both ascending and descending sorting
db.<collection>.createIndex({field: 1}, {unique: true}) // To enforce uniqueness
// Throws duplicate key error if duplicate entries are entered
db.<collection>.getIndexes() // Returns all indexes
db.<collection>.explain().find() // This explains the method used to retrieve data
```

### Multi Field Indexes
* Index on an array field
* Can be single field, or compound index
* Only one array field per index
* Decomposes each value in an array and stores
```javascript
// The same way single field/compound indexes are created
db.<collection>.createIndex({field: 1});
// Difference is visible when you use explain() in the stages it takes
```

### Compound Indexes
* Index on multiple fields
* Also supports queries that match on the prefix of the index fields
* Order of the fields matters - Equality -> Sort -> Range
  * Equality
    * Test exact matches on single field
    * Should be placed first
    * Reduces processing time
    * Retrieves fewer documents
  * Sort
    * Index sort eliminates the need for in-memory sorts
* An Index covers a query when MongoDB does not need to fetch the data from memory since all the required data is already returned by the index
* Common projections should be added to the index so that an index covers everything required
```javascript
db.<collection>.createIndex({field1: 1, field2: 1, field3: -1});
// Any query that matches:
// db.<collection>.find({field1: "value"}).sort({randomField: 1});
// Any query that sorts:
// db.<collection>.find({randomField: "value"}).sort({field1: 1});
```

### Deleting Indexes
* Indexes have a write cost
* Too many indexes lead to poor system performance since many indexes have to be updated
* Only delete indexes that aren't being used since it will degrade query performances if there are no other indexes available
* Recreating an index takes time and resources
* Hide the index and analyse the performance before deleting it, since hidden indexes aren't used but are updated.
```javascript
// Hiding index before deleting
db.<collection>.hideIndex(indexName);
db.<collection>.hideIndex({field1: 1, field2: 1, field3: -1});

// Deleting Index
db.<collection>.dropIndex(indexName);
db.<collection>.dropIndex({field1: 1, field2: 1, field3: -1});

// Deleting multiple indexes
db.<collection>.dropIndexes(indexName);
db.<collection>.dropIndexes(['index1', 'index2', ...]);

// Deleting all Indexes
db.<collection>.dropIndexes();
// Deletes everything except the default index, which can never be deleted
```

## MongoDB Atlas Search
### Database Indexes
* Used by developers and database admins to make their database queries faster
### Search Indexes
* Specifiy how records are referenced for relevance based search
* Types:
  * Dynamic Mapping
    * All fields are indexed, except booleans, ObjectIDs, and timestamps
  * Static Mapping

### Search Index using Dynamic Mapping
* Default option
* Steps:
  * Search tab
  * Create Search Index
  * Choose Visual Editor / JSON Editor
  * Index Analysers, Search Analysers. Many options available. Lucene is industry standard
  * Dynamic Mapping On to index all fields, so no specific field mappings are required
  * Create Search Index Button
* When to use:
  * Dynamic field mapping is used to search all of the fields for the search term, with equal weight on all items

### Search Index using Static Mapping
* The fields being queried are always the same
* When the user only would care about certain fields
* Makes it quicker by minimising the fields to be indexed
* Steps
  * Search Tab
  * Choose Visual Editor / JSON Editor
  * Switch off Dynamic Mapping
  * Select Add Fields to specifiy fields
  * Add Field Name, Add Data type
  * Create Search Index Button

### Search and Compound Operators
* Compound operator enables us to give weights to certain fields
* Options for compound operators:
  * must - only records that meet criteria
  * mustNot - only records that don't meet criteria
  * should - allows us to give weight by customising score
  * filter - doesn't affect score, but returns result that match the clause

```javascript
$search {
  "compound": {
    "must": [{
      "text": {
        "query": "field",
        "path": "habitat"
      }
    }],
    "should": [{
      "range": {
        "gte": 45,
        "path": "wingspan_cm",
        "score": {"constant": {"value": 5}}
      }
    }]
  }
}
```

### Grouping Results using Facets
* Facets are buckets we use to group results
* Grouping results like people, posts, comments
* Datatypes considered for grouping:
  * Numbers
  * Dates
  * Strings
* Configure to use facets in datatypes section while creating static indexes
* Buckets are not visible in the results, but are visible in the search metaData
* metaData:
  * count
  * facets
```javascript
$searchMeta: {
    "facet": {
        "operator": {
            "text": {
            "query": ["Northern Cardinal"],
            "path": "common_name"
            }
        },
        "facets": {
            "sightingWeekFacet": {
                "type": "date",
                "path": "sighting",
                "boundaries": [ISODate("2022-01-01"), 
                    ISODate("2022-01-08"),
                    ISODate("2022-01-15"),
                    ISODate("2022-01-22")],
                "default" : "other"
            }
        }
    }
}
```

### Tokenisation Methods using Autocomplete
* Edge N-Gram Tokenisation
  * Increasing lengths from the beginning of the word
  * M, Mo, Mon, Mong, Mongo ...
* Standard Tokenisation
  * Based on standard word boundaries
  * Less useful since it doesn't generate parital tokens
  * Usually used in conjuction with other tokenisers
* N-gram Tokenisation
  * Breaks text into contiguous texts
  * Mo, on, go, ...

 ## Random Facts
 * Maximum size of a document - 16GB
 * Neospatial datatype is not supported in MongoDB
 * db.collection.explain() and cursor.explain() both works
 * Router read reqeusts to specific shard based on the specified shard key
 * Config Servers keep the metadata of a sharded cluster
 * All operations in MongoDB are atomic at the document level
 * Update with multi: true option can update many documents
 * Default write concern is Acknowledged
 * All operations are recorded in oplog
 * Oplog in 64-bit Linux System can be from 1GB to 50GB
 * Default storage engine is MMAPV1
 * GridFS to store and retrieve files that exceed 16MB limit
 * Document cannot be more than 16MB so that it doesn't use excessive amount of RAM, excessive amount of bandwidtch
 * ObjectID is a 12byte BSON key in which the last 3 bytes represent a 3 byte counter starting with a random value
 * Binary Data is not supported in JSON, but in BSON
 * Mongodump is used for taking backups
 * Mongostat counts the database operations by type
 * Quantity of data contained in the database is stored in dbStats
 * Motop, Gangila, Nagios can be configured to monitor a MongoDB cluster
 * $hint can be used to specifiy a particular index
 * Mongoose is used to model application data in node.js
 * explain() command runs in the queryPlanner mode
 * MongoDB writes take 100ms to the journal
 * Maximum index key limit : 1024bytes
 * Maximum number of indexes : 64
 * 
 

   


