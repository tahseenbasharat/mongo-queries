### MongoDB Queries Cheatsheet
This documentation includes the basic queries for MONGODB

### Database Queries
```
// create or use (if exists) particular databse
$ use testMongoDB

// show the current databse we're working with
$ db

// list all the available databases
$ show dbs
Note: Above command only show database which has atleast 1 collection exist

// delete the database   
$ db.dropDatabase()
```

### Collection Queries
```
// create collection
$ db.createCollections("testCollection")

// list all collections inside a database
$ show collections

// insert the data in a collection
$ db.newCollection.insert({"Name": "Test"})
Note: Above command will create the collection if not exist and insert the given data and .insert method is not deprecated mongodb encourage to use insertOne for single record entry for detail please see "Insert Queries" section

// delete the collection
$ db.newCollection.drop() or $ db.getCollection("newCollection").drop()
```

### Insert Queries
```
// insert single record in a collection (employees)
$ db.employees.insertOne({
    "EmpNo":"1",
    "FirstName":"Andrew",
    "LastName":"Neil",
    "Age":"30",
    "Gender":"Male",
    "Skill":"MongoDB",
    "Phone":"408-1234567",
    "Email":"Andrew.Neil@gmail.com",
"Salary":"80000"
  })
Note: We're using insertOne instead of insert. It is deprecated as of MongoDB version 3.2, and you are encouraged to use insertOne or insertMany instead.

// insert multiple records in a collection (employees)
$ db.employees.insertMany([
    {
        "EmpNo":"2",
        "FirstName":"Brian",
        "LastName":"Hall",
        "Age":"27",
        "Gender":"Male",
        "Skill":"Javascript",
        "Phone":"408-1298367",
        "Email":"Brian.Hall@gmail.com",
        "Salary":"60000"
    },
    {
        "EmpNo":"3",
        "FirstName":"Chris",
        "LastName":"White",
        "Age":"40",
        "Gender":"Male",
        "Skill":"Python",
        "Phone":"408-4444567",
        "Email":"Chris.White@gmail.com",
        "Salary":"100000"
    },
    {
        "EmpNo":"4",
        "FirstName":"Debbie",
        "LastName":"Long",
        "Age":"32",
        "Gender":"Female",
        "Skill":"Project Management",
        "Phone":"408-1299963",
        "Email":"Debbie.Long@gmail.com",
        "Salary":"105000"
    },
    {
        "EmpNo":"5",
        "FirstName":"Ethan",
        "LastName":"Murphy",
        "Age":"45",
        "Gender":"Male",
        "Skill":"C#",
        "Phone":"408-3314567",
        "Email":"Ethan.Murphy@gmail.com",
        "Salary":"120000"
    },
    {
        "EmpNo":"6",
        "FirstName":"Felicia",
        "LastName":"Lee",
        "Age":"33",
        "Gender":"Female",
        "Skill":"MongoDB",
        "Phone":"408-8832567",
        "Email":"Felicia.Lee@gmail.com",
        "Salary":"85000"
    },
    {
        "EmpNo":"7",
        "FirstName":"George",
        "LastName":"Cyrus",
        "Age":"36",
        "Gender":"Male",
        "Skill":"MongoDB",
        "Phone":"408-9984567",
        "Email":"George.Cyrus@gmail.com",
        "Salary":"88000"
    },
    {
        "EmpNo":"8",
        "FirstName":"Hannah",
        "LastName":"Johnson",
        "Age":"26",
        "Gender":"Female",
        "Skill":"AngularJS",
        "Phone":"408-7654321",
        "Email":"Hannah.Johnson@gmail.com",
        "Salary":"72000"
    }
  ]) 
```

#### Retrieve Queries
```
// Retrieve records from employees collection
$ db.employees.find().pretty()
Note: pretty() is an optional chain method to retrieve data in structured format

// Retrieve single record
$ db.employees.findOne()
Note: findOne() doesn't have pretty() method as it already returns data in structured format

// Retrieve a record where EmpNo is equal to 2 
$ db.employees.find({
    "EmpNo": "2"
  })

// Another way of doing the same where EmpNo is equal to 2
$ db.employees.find({
    "EmpNo": {$eq: "2"}
  })
    
// Get all the employees where age less than 30
$ db.employees.find({
    "Age": {$lt: "30"}
  })
Note: If you want to include 30 as well than use $lte and you want the records having age greater than 30 then you can use $gt or $gte to include 30 as well

// Get all the employees where age not equals to 30 
$ db.employees.find({
    "Age": {$ne: "30"}
  })
```

#### AND / OR Conditions Queries
```
// AND condition: where skill eqauls to "MongoDB" and Salary is "80000" 
$ db.employees.find({
    "Skill": {$eq: "MongoDB"},
    "Salary": {$eq: "80000"}
  })
  
// OR condition: where skill equals to "MongoDB" or Salary is "100000"
$ db.employees.find({
    $or: [
      {"Skill": {$eq: "MongoDB"}},
      {"Salary": {$eq: "100000"}}
    ]
  })
  
// OR and AND condition: where skill equals to "MongoDB" and Salary could be "80000" or "85000"
$ db.employees.find({
    "Skill": {$eq: "MongoDB"},
    $or: [
      {"Salary": "80000"}, {"Salary": "85000"}
    ]
  })
```

#### Update Queries
```
// update single record where _id equals to "6581100b8f20c55e0b07250d" 
$ db.employees.updateOne(
    {"_id": {$eq: ObjectId("6581100b8f20c55e0b07250d")}},
    {$set: {"Salary": "90000"}}
  )
  
// update multiple records where skill equals to "MongoDB"
$ db.employees.updateMany(
    {"Skill": {$eq: "MongoDB"}},
    {$set: {"Salary": "90000"}}
  )
  
Note for older version update() was used to update single/multiple record(s)
$ db.employees.update(
    {"Skill": {$eq: "MongoDB"}},
    {$set: {"Salary": "90000"}}
    {multi: true} // this when you want to update multiple records by default it set to false and only updates single record
  )
```

#### Delete Queries
```
// delete a single record by _id
$ db.employees.deleteOne(
    {"_id": {$eq: ObjectId("6581100b8f20c55e0b07250d")}}
  )
 
// delete all records where skill is not equal to "MongoDB" 
$ db.employees.deleteMany(
    {"Skill": {$ne: "MongoDB"}}
  )
  
// Note in older version
$ db.employees.remove(
    {"Skill": {$ne: "MongoDB"}},
    2 // this is optional parameter, by default it delete all the matched records, whereas if mentioned than it will delete n (here n = 2) number of records
  )
```

#### Select Field Queries, Limit & Skip
```
// Select FirstName only in query result
$ db.employees.find({}, {_id: 0, FirstName: 1})
Note: 0 means no selection, 1 means select the field. By default _id will be retrieve if you won't explicity set to 0

// restrict the given data to be rerieved as given limit
$ db.employees.find({}, {_id: 0, FirstName: 1}).limit(1)

// skip the n number of records given in skip method
$ db.employees.find({}, {_id: 0, FirstName: 1}).skip(1)

// sorted result FirstName descending
$ db.employees.find({}, {_id: 0, FirstName: 1}).sort({FirstName: -1})
Note: In sort -1 means descending and 1 ascending

// comination of all above queries
$ db.employees.find({}, {_id: 0, FirstName: 1}).sort({FirstName: -1}).skip(2).limit(2)
```

#### Indexing Queries
```
// creating an index on email field
$ db.employees.ensureIndex({"Email": 1})

// get all indexes on employees collection
$ db.employees.getIndexes()

// delete/drop index from the email field
$ db.employees.dropIndex({"Email": 1})
```

#### Aggregate Querires
```
db.employees.aggregate([{$group: {_id: "$Gender", Total: {$sum: 1}}}])
db.employees.aggregate([{$group: {_id: "$Gender", MaxAge: {$max: "$Age"}}}])
```


#### Backup and Restore
```
// to create a backup for your all databases 
$ mongodump


// to restore database (as in previous step we created backup)
$ mongorestore

// to create a backup for specific database
$ mongodump --db database-name

$ mongorestore --db database-name path-to-the-dump-folder-of-specified-database
i.e. dump/database-name

$ mongodump --db database-name --collection collection-name

$mongorestore --db database-name --collection collection-name path-to-dump-folder-of-sepcified-collection
i.e. dump/database-name/collection-name.bson
```
