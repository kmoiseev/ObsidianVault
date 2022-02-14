Document-oriented database. Supports **MapReduce** data aggregation and declarative query language called **aggregation pipeline**.

### Example:

#### Data:

 
```json
{
 	observationTimestamp: Date.parse("Mon, 25 Dec 1995 12:34:56 GMT"),
 	family:     "Sharks",
 	species:    "Carcharodon carcharias",
 	numAnimals: 3 

} {

	observationTimestamp: Date.parse("Tue, 12 Dec 1995 16:17:18 GMT"), 
	family:     "Sharks", 
	species:    "Carcharias taurus", 
	numAnimals: 4
}
```

#### Task
Search for all the sharks grouped by mounth

#### MapReduce query

```js
db.observations.mapReduce( function map() {

	var year = this.observationTimestamp.getFullYear(); 
	var month = this.observationTimestamp.getMonth() + 1; 
	emit(year + "-" + month, this.numAnimals);

},  
function reduce(key, values) {
	
	return Array.sum(values); 
},
{ 
	query: { family: "Sharks"},
	out: "monthlySharkReport" 

} );
```

#### Aggregation pipeline query
```js
 db.observations.aggregate([
  { $match: { family: "Sharks" } },
  {
    $group: {
      _id: {
        year: { $year: "$observationTimestamp" },
        month: { $month: "$observationTimestamp" },
      },

      totalAnimals: { $sum: "$numAnimals" },
    },
  },
]);

```