#Specific Tips To Promote Composability
 

var pipeline = [
  {"$unset": [
    "_id",
    "address",
  ]},    
    
  // Only match people born on or after 1st January 1970
  {"$match": {
    "dateofbirth": {"$gte": ISODate("1970-01-01T00:00:00Z")},
  }},
  
  /*
  {"$sort": {
    "dateofbirth": -1,
  }},      
    
  {"$limit": 2},  
  */
];

#Better Alternatives To A Project Stage
 
[
  {"$project": {
    // Add some fields
    "transaction_info.date": "$transaction_date",
    "transaction_info.amount": "$transaction_amount",
    "status": {"$cond": {"if": "$reported", "then": "REPORTED", "else": "UNREPORTED"}},
    
    // Remove _id field
    "_id": 0,
  }},
]

#Using Explain Plans:

db.customer_orders.explain("queryPlanner").aggregate(pipeline);
db.customer_orders.explain("executionStats").aggregate(pipeline);

#Summarising Aggregation Expressions

"customer_info": {"$cond": {
                    "if":   {"$eq": ["$customer_info.category", "SENSITIVE"]}, 
                    "then": "$$REMOVE",     
                    "else": "$customer_info",
                 }}
-----------
[
  { _id: 1, width: 2, height: 8 },
  { _id: 2, width: 3, height: 4 },
  { _id: 3, width: 20, height: 1 }
]

var pipeline = [
  {"$match": {
    "$expr": {"$gt": [{"$multiply": ["$width", "$height"]}, 12]},
  }},      
];
#Filtered Top Subset:

use book-filtered-top-subset;
db.dropDatabase();

// Create an index for a persons collection
db.persons.createIndex({"vocation": 1, "dateofbirth": 1});

// Insert records into the persons collection
db.persons.insertMany([
  {
    "person_id": "6392529400",
    "firstname": "Elise",
    "lastname": "Smith",
    "dateofbirth": ISODate("1972-01-13T09:32:07Z"),
    "vocation": "ENGINEER",
    "address": { 
        "number": 5625,
        "street": "Tipa Circle",
        "city": "Wojzinmoj",
    },
  },
  {
    "person_id": "1723338115",
    "firstname": "Olive",
    "lastname": "Ranieri",
    "dateofbirth": ISODate("1985-05-12T23:14:30Z"),    
    "gender": "FEMALE",
    "vocation": "ENGINEER",
    "address": {
        "number": 9303,
        "street": "Mele Circle",
        "city": "Tobihbo",
    },
  },
  {
    "person_id": "8732762874",
    "firstname": "Toni",
    "lastname": "Jones",
    "dateofbirth": ISODate("1991-11-23T16:53:56Z"),    
    "vocation": "POLITICIAN",
    "address": {
        "number": 1,
        "street": "High Street",
        "city": "Upper Abbeywoodington",
    },
  },
  {
    "person_id": "7363629563",
    "firstname": "Bert",
    "lastname": "Gooding",
    "dateofbirth": ISODate("1941-04-07T22:11:52Z"),    
    "vocation": "FLORIST",
    "address": {
        "number": 13,
        "street": "Upper Bold Road",
        "city": "Redringtonville",
    },
  },
  {
    "person_id": "1029648329",
    "firstname": "Sophie",
    "lastname": "Celements",
    "dateofbirth": ISODate("1959-07-06T17:35:45Z"),    
    "vocation": "ENGINEER",
    "address": {
        "number": 5,
        "street": "Innings Close",
        "city": "Basilbridge",
    },
  },
  {
    "person_id": "7363626383",
    "firstname": "Carl",
    "lastname": "Simmons",
    "dateofbirth": ISODate("1998-12-26T13:13:55Z"),    
    "vocation": "ENGINEER",
    "address": {
        "number": 187,
        "street": "Hillside Road",
        "city": "Kenningford",
    },
  },
]);
#Aggregation Pipeline:
var pipeline = [
  // Match engineers only
  {"$match": {
    "vocation": "ENGINEER",
  }},
    
  // Sort by youngest person first
  {"$sort": {
    "dateofbirth": -1,
  }},      
    
  // Only include the first 3 youngest people
  {"$limit": 3},  

  // Exclude unrequired fields from each person record
  {"$unset": [
    "_id",
    "vocation",
    "address",
  ]},    
];

#Execution:

db.persons.aggregate(pipeline);
db.persons.explain("executionStats").aggregate(pipeline);