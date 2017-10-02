# House Price Example
Simple CRUD and aggregation example on a house price mongo database.
It is using the mongo shell client.

# Importing localhost
Run the command (you must have a mongo server running localhost):
```
$ mongorestore -d homeSales <HomeSalesDirectoryLocation>
```

It will create the database ``homeSales`` containing the data.


# Running example

Run the mongo db client:
```
$ mongo
> use homeSales;
```

Then, for each example you can copy and paste the following:

1. For creating example:
```
db.homeSales.insertOne(
{
      "amount" : 9000,
      "date" : ISODate("1996-09-19T00:00:00Z"),
      "address" : {
              "nameOrNumber" : 9,
              "street" : "NORFOLK PARK COTTAGES",
              "town" : "MAIDENHEAD",
              "county" : "WINDSOR AND MAIDENHEAD",
              "postcode" : "SL6 7DR"
      }
});
```

Get the ``insertedId`` field.

2. For reading example:
```
db.homeSales.find(
{
     "_id": ObjectId("insertedId")
});
```

3. For updating example:
```
db.homeSales.updateOne(
{
   "_id": ObjectId("insertedId")
},
{
   $set:
        {
          "amount": 5000
        }
}
);
```

4. For deleting example:
```
db.homeSales.deleteOne(
{
   "_id": ObjectId("insertedId")
}
);
```

5. For aggregation example 1:
```
db.homeSales.aggregate([
{
  $match: {
				amount: { 
        				$gte:3000000
        }
  }
}, 
{
  $lookup: {
				from: "postcodes", 
        localField: "address.postcode",
        foreignField: "postcode",
    		as: "postcode_docs"
  }
}
]);
```

6. For aggregation example 2:
```
db.homeSales.aggregate([
  {
    $group: 
    {
      _id: {$year: "$date"},
      higestPrice: {$sum: "$amount"},
      lowestPrice: {$min: "$amount"},
      averagePrice: {$avg: "$amount"},
      priceStdDev: {$stdDevPop: "$amount"}
    }
  },
  {
    $sort: {_id: 1}
  }
]);
```
