```
{
  "address": {
     "building": "1007",
     "coord": [ -73.856077, 40.848447 ],
     "street": "Morris Park Ave",
     "zipcode": "10462"
  },
  "borough": "Bronx",
  "cuisine": "Bakery",
  "grades": [
     { "date": { "$date": 1393804800000 }, "grade": "A", "score": 2 },
     { "date": { "$date": 1378857600000 }, "grade": "A", "score": 6 },
     { "date": { "$date": 1358985600000 }, "grade": "A", "score": 10 },
     { "date": { "$date": 1322006400000 }, "grade": "A", "score": 9 },
     { "date": { "$date": 1299715200000 }, "grade": "B", "score": 14 }
  ],
  "name": "Morris Park Bake Shop",
  "restaurant_id": "30075445"
}
```


```sql
/* Write a MongoDB query to display all the documents in the 
collection restaurants. */

db.restaurants.find({});

/*
Write a MongoDB query to display the fields restaurant_id, name, 
borough and cuisine for all the documents in the collection restaurant.
*/

db.restaurants.find({},
{restaurant_id:1 , name : 1 , borough : 1 , cuisine : 1 });


/*Write a MongoDB query to display the fields restaurant_id, 
name, borough and cuisine, but exclude the field _id for all 
the documents in the collection restaurant.
*/

db.restaurants.find({},
{_id : 0  , restaurant_id:1 , name : 1 , borough : 1 , cuisine : 1 })


/*
Write a MongoDB query to display the fields restaurant_id, name, borough 
and zip code, but exclude the field _id for all the documents in the 
collection restaurant
*/

db.restaurants.find({} , 
{_id : 0  , restaurant_id:1 , name : 1 , borough : 1 , "address.zipcode": 1 })


/*
Write a MongoDB query to display all the restaurant 
which is in the borough Bronx.
*/
db.restaurants.find({"borough" : "Bronx"});


/*
Write a MongoDB query to display the first 5
 restaurant which is in the borough Bronx.
*/

db.restaurants.find({"borough" : "Bronx"}).limit(5);


/*
Write a MongoDB query to display the next 5 restaurants 
after skipping first 5 which are in the borough Bronx.
*/

db.restaurants.find({"borough" : "Bronx"}).skip(5).limit(5));


```


```sql
/*
Write a MongoDB query to find the restaurants who 
achieved a score more than 90.
*/

db.restaurants.find({"grades.score" : {$gt:90}});
(OR)
db.restaurants.find({grades : { $elemMatch:{"score":{$gt : 90}}}});


/*
Write a MongoDB query to find the restaurants
 that achieved a score, more than 80 but less than 100.
*/

db.restaurants.find({$and : 
[{"grades.score":{$gt:80}} {"grades.score":{$lt:100}}]
});

(OR)

db.restaurants.find({"grades.score":{$gt:80 , $lt:100}});


/*
Write a MongoDB query to find the restaurants 
which locate in latitude value less than -95.754168.
*/

db.restaurants.find({"address.coord" : {$lt : -95.754168} })


/*
Write a MongoDB query to find the restaurants that do not prepare
 any cuisine of 'American' and their grade score more than 70 and 
 **latitude **less than -65.754168.
*/

db.restaurants.find(
{$and : 
[
  {"cuisine" : {$ne : "American" }},
  {"grades.score" : {$gt : 70}},
  {"address.coord" : {$lt : -65.754168 }}
   
]})


/*
Write a MongoDB query to find the restaurants which do not 
prepare any cuisine of 'American' and achieved a score more
 than 70 and located in the **longitude **less than -65.754168.
*/

db.restaurants.find(
{$and : 
[
    {"cuisine" : {$ne : "American" }},
    {"grades.score" : {$gt : 70}},
    {"address.coord" : {$lt : -65.754168 }}
]})

(OR)

db.restaurants.find(
{
    {"cuisine" : {$ne : "American" }},
    {"grades.score" : {$gt : 70}},
    {"address.coord" : {$lt : -65.754168 }}
})

```




```sql
/*Write a MongoDB query to find the restaurants which do not prepare 
any cuisine of 'American' and achieved a grade point 'A' not belongs
to the borough Brooklyn. The document must be displayed according to
the cuisine in descending order.*/

db.restaurants.find(
{
  "cuisine" : {$ne : "American" },
  "grades.grade": {$eq: "A"},
  "borough" : {$ne : "Brooklyn"}
}).sort({"cuisine":-1})

(OR)

db.restaurants.find(
{$and :
[
  {"cuisine" : {$ne : "American" }},
  {"grades.grade": {$eq: "A"}},
  {"borough" : {$ne : "Brooklyn"}}
]});

```
```sql
/*
Write a MongoDB query to find the restaurant Id, name,
 borough and cuisine for those restaurants which contain
  'Wil' as first three letters for its name.
*/

db.restaurants.find({"name" : /^Wil/},
{"restaurant_id":1 ,borough:1 : cuisine:1 ,name:1,  id:0 })

/*Write a MongoDB query to find the restaurant Id, name, 
borough and cuisine forthose restaurants which contain 'ces' 
as last three letters for its name.
*/

db.restaurants.find( {"name" : /ces$/}, 
{"restaurant_id":1 ,borough:1 : cuisine:1 ,name :1, id:0 })


```
```
/*Write a MongoDB query to find the restaurants which belong 
to the borough Bronx and prepared either American or Chinese dish.
*/

db.restaurants.find({borough:'Bronx' , cuisine: {$in:["American" , "Chinese"]} })


/*
Write a MongoDB query to find the restaurant Id, name, borough 
and cuisine for those restaurants which belong to the borough 
Staten Island or Queens or Bronxor Brooklyn.
*/

db.restaurants.find({borough: {$in : [
  "Staten Island" , "Queens" ,"Bronxor Brooklyn"
]}})


/*
Write a MongoDB query to find the restaurant Id, name, borough 
and cuisine for those restaurants which are not belonging to the
 borough Staten Island or Queens or Bronxor Brooklyn.
*/

db.restaurants.find({borough: {$nin : [
  "Staten Island" , "Queens" ,"Bronxor Brooklyn"
]}})


/*
Write a MongoDB query to find the restaurant Id, name, borough 
and cuisine for those restaurants which achieved a score which 
is not more than 10.
*/

db.restaurants.find({"grades.score":{{$not : {$gt : 10}}})




```
```sql
/*Write a MongoDB query to find the borough with the highest number
 of restaurants that have a grade of "A" and a score greater than or
  equal to 90.*/

db.restaurants.aggregate(
  [
    {$match : {"grades.grade" : {$eq : "A"}} , "grades.score" : {$gte:90}},
    {$group : {_id : "$borough"} , count : {$sum : 1}},
    {
$sort: { count: -1 }},
    {$limit: 1}
  ]
)



/*
Write a MongoDB query to find the top 5 **restaurants in each borough** with 
the highest number of "A" grades.
*/

db.restaurants.aggregate(
  [
    {$unwind : $grades},
    {$match : {"grades.grade" : {$eq : "A"}}},
    {$group : 
      {_id : {borough: "$borough", restaurant_id: "$restaurant_id"},
      gradeCount : {$sum : 1},
     },
    {$sort : {gradeCount : -1}},
    {$group : {_id : "$_id.borough" , tot_res : {
      $push :{restaurant_id: "$_id.restaurant_id" , "gradeCount" : "$gradeCount"}
    }}
    {$project : {
      _id : 0 , 
      tot_res : {$slice: [$tot_res,5]}
    }}
    
  ]
)


/*
Write a MongoDB query to find the top 5 restaurants with the highest
 average score for each cuisine type, along with their average scores.
*/

db.restaurants.aggregate([
  {$unwind :  $"grades"}
  {$group : _id: {cuisine: "$cuisine", restaurant_id: "$restaurant_id"},
            avgScore: {$avg: "$grades.score"}\
   }
   {$sort : {$avgScore :-1}},
   {
     $group : {_id : $_id.cuisine ,
              tot_res : {$push : {res_id : $_id.restaurant_id , avgScore: $avgScore } }
    }},
    {
      $project : {
        _id : 0,
        count : {$slice : ["$tot_res",5]}
      }
    }
])



```
