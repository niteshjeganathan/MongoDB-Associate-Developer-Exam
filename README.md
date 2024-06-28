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


