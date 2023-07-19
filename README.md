# mongo_compass_tutorial

## Description

This is a guide to installing MongoDB Compass and connecting to the Nellodee remote server. As well as a brief overview of some simple mongo queries using the Compass GUI.


## Installation 

Navigate to [Install MongoDB Compass](https://www.mongodb.com/docs/compass/master/install/) <br>
Choose the version corresponding to your operating system.

You shouldn't need to install MongoDB itself to connect to a remote instance of MongoDB but I will leave the link to install MongoDB on your local system here: [Install MongoDB](https://www.mongodb.com/docs/manual/installation/) <br>
If you want to run/test/use MongoDB on your local system, you will need to follow the steps on the link above and make sure you choose the <b>Community Edition</b> corresponding to your operating system.

## Starting Mongod Service on Nellodee

* First check to see if the mongod service is already running by using the following command in the terminal `service mongod status`
    ![Mongo Status Check Ubuntu](/assets/mongo_status_ubuntu.png)

* If the `Active` property says "active (running)" then you can skip to the next step.

* If the `Active` property says "inactive (dead)" then you can run the following command `sudo systemctl start mongod`

## Connecting MongoDB Compass to Nellodee

At this point you should have a working version of the MongoDB Compass GUI installed and opened and confirmed that the mongo service has started on nellodee.

Watch the video to see me walk through the steps to connect to the mongo service on nellodee.



https://github.com/educational-technology-collective/mongo_compass_tutorial/assets/112991266/56e1aeee-dc51-43c4-b80b-e294fb7250c9



1. Click `New Connection` button
2. Click `Advanced Connection Options` expansion button
3. Choose `Proxy/SSH` tab
4. Fill in all four fields: 
    - SSH Hostname: `nellodee.si.umich.edu`
    - SSH Port: `22`
    - SSH Username: `your umich uniqname`
    - SSH Password: `password associated with your uniqname`
5. Click `Save & Connect`
6. You will be prompted to choose a color and enter a name for your connection

At this point you should be connected to the running mongo service on nellodee and see a list of the databases stored in mongo on nellodee. If you are already familiar with MongoDB queries then you should be good to go. 

If not, I will link a cheat sheet to standard mongo queries to get you started.

[MongoDB Queries](https://www.mongodb.com/docs/compass/current/query/filter/)





# Aggregations

<b>Each method below can be opened up for a more detailed explanation and examples</b>

<details>
    <summary>
    <h2><a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match" target="_blank">$match</a></h2>
    <img src="https://www.mongodb.com/docs/manual/assets/link.svg" alt="Permalink to this definition">
    <p>Filters the documents to pass only the documents that match the specified condition(s) to the next pipeline stage.</p>
    <h4>SQL Equivalent: </br> 
        <pre>
    SELECT * 
    FROM collection 
    WHERE condition;
        </pre>
    </h4>
   
  </summary>

    

---
The [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) stage has the following prototype form:

```
{ $match: { <query> } }
```

[`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) takes a document that specifies the query conditions. The query syntax is identical to the [read operation query](https://www.mongodb.com/docs/manual/tutorial/query-documents/#std-label-read-operations-query-argument) syntax; i.e. [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) does not accept [raw aggregation expressions](https://www.mongodb.com/docs/manual/meta/aggregation-quick-reference/#std-label-aggregation-expressions). Instead, use a [`$expr`](https://www.mongodb.com/docs/manual/reference/operator/query/expr/#mongodb-query-op.-expr) query expression to include aggregation expression in [`$match`.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match)


##### Examples
---
The examples use a collection named `articles` with the following documents:

```
{ "_id" : ObjectId("512bc95fe835e68f199c8686"), "author" : "dave", "score" : 80, "views" : 100 }
{ "_id" : ObjectId("512bc962e835e68f199c8687"), "author" : "dave", "score" : 85, "views" : 521 }
{ "_id" : ObjectId("55f5a192d4bede9ac365b257"), "author" : "ahn", "score" : 60, "views" : 1000 }
{ "_id" : ObjectId("55f5a192d4bede9ac365b258"), "author" : "li", "score" : 55, "views" : 5000 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b259"), "author" : "annT", "score" : 60, "views" : 50 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25a"), "author" : "li", "score" : 94, "views" : 999 }
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25b"), "author" : "ty", "score" : 95, "views" : 1000 }
```

### Equality Match[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#equality-match "Permalink to this heading")

The following operation uses [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) to perform a simple equality match:

```
db.articles.aggregate(
    [ { $match : { author : "dave" } } ]
);
```

The [`$match`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/match/#mongodb-pipeline-pipe.-match) selects the documents where the `author` field equals `dave`, and the aggregation returns the following:

```
{ "_id" : ObjectId("512bc95fe835e68f199c8686"), "author" : "dave", "score" : 80, "views" : 100 }
{ "_id" : ObjectId("512bc962e835e68f199c8687"), "author" : "dave", "score" : 85, "views" : 521 }
```


---
---
    
</details>


<details>

    
<summary>
    <h2><a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group" target="_blank">$group</a></h2>
    <img src="https://www.mongodb.com/docs/manual/assets/link.svg" alt="Permalink to this definition">
    <p>The `$group` stage separates documents into groups according to a "group key". The output is one document for each unique group key.
    </p>
    <h4> SQL Equivalent: </br>
    <pre>
    SELECT key_field, accumulator_function(field) AS computed_field
    FROM collection
    GROUP BY key_field;
    </pre>
    </h4>
  </summary>


A group key is often a field, or group of fields. The group key can also be the result of an expression. Use the `_id` field in the `$group` pipeline stage to set the group key. See below for [usage examples.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#std-label-ex-agg-group-stage)

In the `$group` stage output, the `_id` field is set to the group key for that document.

The output documents can also contain additional fields that are set using [accumulator expressions.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#std-label-accumulators-group)

## NOTE

[`$group`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group) does _not_ order its output documents.

The [`$group`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group) stage has the following prototype form:

```
{
  $group:
    {
      _id: <expression>, // Group key
      <field1>: { <accumulator1> : <expression1> },
      ...
    }
 }
```

The `<accumulator>` operator must be one of the following accumulator operators:
[list of valid accumulators](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#std-label-accumulators-group)

### Examples
---

In mongosh create a sample collection named sales with the following documents:

```
db.sales.insertMany([
  { "_id" : 1, "item" : "abc", "price" : NumberDecimal("10"), "quantity" : NumberInt("2"), "date" : ISODate("2014-03-01T08:00:00Z") },
  { "_id" : 2, "item" : "jkl", "price" : NumberDecimal("20"), "quantity" : NumberInt("1"), "date" : ISODate("2014-03-01T09:00:00Z") },
  { "_id" : 3, "item" : "xyz", "price" : NumberDecimal("5"), "quantity" : NumberInt( "10"), "date" : ISODate("2014-03-15T09:00:00Z") },
  { "_id" : 4, "item" : "xyz", "price" : NumberDecimal("5"), "quantity" :  NumberInt("20") , "date" : ISODate("2014-04-04T11:21:39.736Z") },
  { "_id" : 5, "item" : "abc", "price" : NumberDecimal("10"), "quantity" : NumberInt("10") , "date" : ISODate("2014-04-04T21:23:13.331Z") },
  { "_id" : 6, "item" : "def", "price" : NumberDecimal("7.5"), "quantity": NumberInt("5" ) , "date" : ISODate("2015-06-04T05:08:13Z") },
  { "_id" : 7, "item" : "def", "price" : NumberDecimal("7.5"), "quantity": NumberInt("10") , "date" : ISODate("2015-09-10T08:43:00Z") },
  { "_id" : 8, "item" : "abc", "price" : NumberDecimal("10"), "quantity" : NumberInt("5" ) , "date" : ISODate("2016-02-06T20:20:13Z") },
])
```

The following aggregation operation uses the [`$group`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group) stage to count the number of documents in the `sales` collection:

```
db.sales.aggregate( [  {    $group: {       _id: null,       count: { $count: { } }    }  }] )
```

The operation returns the following result:

```
{ "_id" : null, "count" : 8 }
```

This aggregation operation is equivalent to the following SQL statement:

```
SELECT COUNT(*) AS count FROM sales
```

---
---

</details>

<details>
    <summary>
        <h2><a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project" target="_blank">$project</a></h2>
    <img src="https://www.mongodb.com/docs/manual/assets/link.svg" alt="Permalink to this definition">
    <p>Passes along the documents with the requested fields to the next stage in the pipeline. The specified fields can be existing fields from the input documents or newly computed fields.</p>
    <h4> SQL Equivalent: </br>
    <pre>
    SELECT field1, field2, ...
    FROM collection;
    </pre>
    </h4>
    </summary>




The [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage has the following prototype form:

```
{ $project: { <specification(s)> } }
```

The [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) takes a document that can specify the inclusion of fields, the suppression of the `_id` field, the addition of new fields, and the resetting of the values of existing fields. Alternatively, you may specify the _exclusion_ of fields.

The [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) specifications have the following forms:

|Form|Description|
|---|---|
|`<field>: <1 or true>`|Specifies the inclusion of a field. Non-zero integers are also treated as `true`.|
|`_id: <0 or false>`|Specifies the suppression of the `_id` field.<br><br>To exclude a field conditionally, use the [`REMOVE`](https://www.mongodb.com/docs/manual/reference/aggregation-variables/#mongodb-variable-variable.REMOVE) variable instead. For details, see [Exclude Fields Conditionally.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#std-label-remove-var)|
|`<field>: <expression>`|Adds a new field or resets the value of an existing field.<br><br>If the expression evaluates to `$$REMOVE`, the field is excluded in the output. For details, see [Exclude Fields Conditionally.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#std-label-remove-var)|
|`<field>:<0 or false>`|Specifies the exclusion of a field.<br><br>To exclude a field conditionally, use the [`REMOVE`](https://www.mongodb.com/docs/manual/reference/aggregation-variables/#mongodb-variable-variable.REMOVE) variable instead. For details, see [Exclude Fields Conditionally.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#std-label-remove-var)<br><br>If you specify the exclusion of a field other than `_id`, you **cannot** employ any other [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) specification forms. This restriction does not apply to conditionally exclusion of a field using the [`REMOVE`](https://www.mongodb.com/docs/manual/reference/aggregation-variables/#mongodb-variable-variable.REMOVE) variable.<br><br>See also the [`$unset`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unset/#mongodb-pipeline-pipe.-unset) stage to exclude fields.

## Considerations[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#considerations "Permalink to this heading")

### Include Existing Fields[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#include-existing-fields "Permalink to this heading")

- The `_id` field is, by default, included in the output documents. To include any other fields from the input documents in the output documents, you must explicitly specify the inclusion in [`$project`.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project)
    
- If you specify an inclusion of a field that does not exist in the document, [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) ignores that field inclusion and does not add the field to the document.
    

### Suppress the `_id` Field[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#suppress-the-_id-field "Permalink to this heading")

By default, the `_id` field is included in the output documents. To exclude the `_id` field from the output documents, you must explicitly specify the suppression of the `_id` field in [`$project`.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project)

### Exclude Fields[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#exclude-fields "Permalink to this heading")

If you specify the exclusion of a field or fields, all other fields are returned in the output documents.

```
{ $project: { "<field1>": 0, "<field2>": 0, ... } } // Return all but the specified fields
```

If you specify the exclusion of a field other than `_id`, you cannot employ any other [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) specification forms: i.e. if you exclude fields, you cannot also specify the inclusion of fields, reset the value of existing fields, or add new fields. This restriction does not apply to conditional exclusion of a field using the [`REMOVE`](https://www.mongodb.com/docs/manual/reference/aggregation-variables/#mongodb-variable-variable.REMOVE) variable.

See also the [`$unset`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unset/#mongodb-pipeline-pipe.-unset) stage to exclude fields.

#### Exclude Fields Conditionally[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#exclude-fields-conditionally "Permalink to this heading")

You can use the variable [`REMOVE`](https://www.mongodb.com/docs/manual/reference/aggregation-variables/#mongodb-variable-variable.REMOVE) in aggregation expressions to conditionally suppress a field. For an example, see [Conditionally Exclude Fields.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#std-label-remove-example)

## Examples[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#examples "Permalink to this heading")
---

### Include Specific Fields in Output Documents[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#include-specific-fields-in-output-documents "Permalink to this heading")

Consider a `books` collection with the following document:

```
{  "_id" : 1,  title: "abc123",  isbn: "0001122223334",  author: { last: "zzz", first: "aaa" },  copies: 5}
```

The following [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage includes only the `_id`, `title`, and the `author` fields in its output documents:

```
db.books.aggregate( [ { $project : { title : 1 , author : 1 } } ] )
```

The operation results in the following document:

```
{ "_id" : 1, "title" : "abc123", "author" : { "last" : "zzz", "first" : "aaa" } }
```

### Suppress `_id` Field in the Output Documents[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#suppress-_id-field-in-the-output-documents "Permalink to this heading")

The `_id` field is always included by default. To exclude the `_id` field from the output documents of the [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage, specify the exclusion of the `_id` field by setting it to `0` in the projection document.

Consider a `books` collection with the following document:

```
{  "_id" : 1,  title: "abc123",  isbn: "0001122223334",  author: { last: "zzz", first: "aaa" },  copies: 5}
```

The following [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage excludes the `_id` field but includes the `title`, and the `author` fields in its output documents:

```
db.books.aggregate( [ { $project : { _id: 0, title : 1 , author : 1 } } ] )
```

The operation results in the following document:

```
{ "title" : "abc123", "author" : { "last" : "zzz", "first" : "aaa" } }
```

### Exclude Fields from Output Documents[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#exclude-fields-from-output-documents "Permalink to this heading")

Consider a `books` collection with the following document:

```
{  "_id" : 1,  title: "abc123",  isbn: "0001122223334",  author: { last: "zzz", first: "aaa" },  copies: 5,  lastModified: "2016-07-28"}
```

The following [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage excludes the `lastModified` field from the output:

```
db.books.aggregate( [ { $project : { "lastModified": 0 } } ] )
```

See also the [`$unset`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unset/#mongodb-pipeline-pipe.-unset) stage to exclude fields.

### Exclude Fields from Embedded Documents[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#exclude-fields-from-embedded-documents "Permalink to this heading")

Consider a `books` collection with the following document:

```
{  "_id" : 1,  title: "abc123",  isbn: "0001122223334",  author: { last: "zzz", first: "aaa" },  copies: 5,  lastModified: "2016-07-28"}
```

The following [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage excludes the `author.first` and `lastModified` fields from the output:

```
db.books.aggregate( [ { $project : { "author.first" : 0, "lastModified" : 0 } } ] )
```

Alternatively, you can nest the exclusion specification in a document:

```
db.bookmarks.aggregate( [ { $project: { "author": { "first": 0}, "lastModified" : 0 } } ] )
```

Both specifications result in the same output:

```
{ "_id" : 1, "title" : "abc123", "isbn" : "0001122223334", "author" : { "last" : "zzz"   }, "copies" : 5,}
```

See also the [`$unset`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unset/#mongodb-pipeline-pipe.-unset) stage to exclude fields.

### Conditionally Exclude Fields[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#conditionally-exclude-fields "Permalink to this heading")

You can use the variable [`REMOVE`](https://www.mongodb.com/docs/manual/reference/aggregation-variables/#mongodb-variable-variable.REMOVE) in aggregation expressions to conditionally suppress a field.

Consider a `books` collection with the following document:

```
{  "_id" : 1,  title: "abc123",  isbn: "0001122223334",  author: { last: "zzz", first: "aaa" },  copies: 5,  lastModified: "2016-07-28"}{  "_id" : 2,  title: "Baked Goods",  isbn: "9999999999999",  author: { last: "xyz", first: "abc", middle: "" },  copies: 2,  lastModified: "2017-07-21"}{  "_id" : 3,  title: "Ice Cream Cakes",  isbn: "8888888888888",  author: { last: "xyz", first: "abc", middle: "mmm" },  copies: 5,  lastModified: "2017-07-22"}
```

The following [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage uses the [`REMOVE`](https://www.mongodb.com/docs/manual/reference/aggregation-variables/#mongodb-variable-variable.REMOVE) variable to excludes the `author.middle` field only if it equals `""`:

```
db.books.aggregate( [ {  $project: {  title: 1, "author.first": 1, "author.last" : 1,  "author.middle": { $cond: { if: { $eq: [ "", "$author.middle" ] }, then: "$$REMOVE",    else: "$author.middle" } } } }] )
```

The aggregation operation results in the following output:

```
{ "_id" : 1, "title" : "abc123", "author" : { "last" : "zzz", "first" : "aaa" } }
{ "_id" : 2, "title" : "Baked Goods", "author" : { "last" : "xyz", "first" : "abc" } }{ "_id" : 3, "title" : "Ice Cream Cakes", "author" : { "last" : "xyz", "first" : "abc", "middle" : "mmm" } }
```


---
---
</details>

<details>
    <summary>
        <h2><a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set" target="_blank">$set</a></h2>
    <img src="https://www.mongodb.com/docs/manual/assets/link.svg" alt="Permalink to this definition">
    <p>Adds new fields to documents. `$set` outputs documents that contain all existing fields from the input documents and newly added fields.</p>
    <h4> No exact SQL Equivalent but you can use UPDATE similarly: </br>
    <pre>
    UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;
    </pre>
    </h4>
    </summary>

_New in version 4.2_.



The [`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) stage is an alias for [`$addFields`.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/addFields/#mongodb-pipeline-pipe.-addFields)

Both stages are equivalent to a [`$project`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/project/#mongodb-pipeline-pipe.-project) stage that explicitly specifies all existing fields in the input documents and adds the new fields.

[`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) has the following form:

```
{ $set: { <newField>: <expression>, ... } }
```

Specify the name of each field to add and set its value to an [aggregation expression](https://www.mongodb.com/docs/manual/meta/aggregation-quick-reference/#std-label-aggregation-expressions). For more information on expressions, see [Expressions.](https://www.mongodb.com/docs/manual/meta/aggregation-quick-reference/#std-label-aggregation-expressions)

## IMPORTANT

If the name of the new field is the same as an existing field name (including `_id`), `$set` overwrites the existing value of that field with the value of the specified expression.

## Behavior[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#behavior "Permalink to this heading")

[`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) appends new fields to existing documents. You can include one or more `$set` stages in an aggregation operation.

To add field or fields to embedded documents (including documents in arrays) use the dot notation. See [example.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#std-label-set-add-field-to-embedded)

To add an element to an existing array field with [`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set), use with [`$concatArrays`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/concatArrays/#mongodb-expression-exp.-concatArrays). See [example.](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#std-label-set-add-element-to-array)

## Examples[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#examples "Permalink to this heading")

### Using Two `$set` Stages[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#using-two--set-stages "Permalink to this heading")

Create a sample `scores` collection with the following:

```
db.scores.insertMany([   { _id: 1, student: "Maya", homework: [ 10, 5, 10 ], quiz: [ 10, 8 ], extraCredit: 0 },   { _id: 2, student: "Ryan", homework: [ 5, 6, 5 ], quiz: [ 8, 8 ], extraCredit: 8 }])
```

The following operation uses two [`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) stages to include three new fields in the output documents:

```
db.scores.aggregate( [   {     $set: {        totalHomework: { $sum: "$homework" },        totalQuiz: { $sum: "$quiz" }     }   },   {     $set: {        totalScore: { $add: [ "$totalHomework", "$totalQuiz", "$extraCredit" ] } }   }] )
```

The operation returns the following documents:

```
{  "_id" : 1,  "student" : "Maya",  "homework" : [ 10, 5, 10 ],  "quiz" : [ 10, 8 ],  "extraCredit" : 0,  "totalHomework" : 25,  "totalQuiz" : 18,  "totalScore" : 43}{  "_id" : 2,  "student" : "Ryan",  "homework" : [ 5, 6, 5 ],  "quiz" : [ 8, 8 ],  "extraCredit" : 8,  "totalHomework" : 16,  "totalQuiz" : 16,  "totalScore" : 40}
```

### Adding Fields to an Embedded Document[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#adding-fields-to-an-embedded-document "Permalink to this heading")

Use dot notation to add new fields to embedded documents.

Create a sample collection `vehicles` with the following:

```
db.vehicles.insertMany([   { _id: 1, type: "car", specs: { doors: 4, wheels: 4 } },   { _id: 2, type: "motorcycle", specs: { doors: 0, wheels: 2 } },   { _id: 3, type: "jet ski" }])
```

The following aggregation operation adds a new field `fuel_type` to the embedded document `specs`.

```
db.vehicles.aggregate( [   { $set: { "specs.fuel_type": "unleaded" } }] )
```

The operation returns the following results:

```
{ _id: 1, type: "car", specs: { doors: 4, wheels: 4, fuel_type: "unleaded" } }{ _id: 2, type: "motorcycle", specs: { doors: 0, wheels: 2, fuel_type: "unleaded" } }{ _id: 3, type: "jet ski", specs: { fuel_type: "unleaded" } }
```

### Overwriting an existing field[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#overwriting-an-existing-field "Permalink to this heading")

Specifying an existing field name in a [`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) operation causes the original field to be replaced.

Create a sample collection called `animals` with the following:

```
db.animals.insertOne( { _id: 1, dogs: 10, cats: 15 } )
```

The following [`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) operation overrides the `cats` field:

```
db.animals.aggregate( [  { $set: { "cats": 20 } }] )
```

The operation returns the following document:

```
{ _id: 1, dogs: 10, cats: 20 }
```

It is possible to replace one field with another. In the following example the `item` field substitutes for the `_id` field.

Create a sample collection called `fruits` contains the following documents:

```
db.fruits.insertMany([   { "_id" : 1, "item" : "tangerine", "type" : "citrus" },   { "_id" : 2, "item" : "lemon", "type" : "citrus" },   { "_id" : 3, "item" : "grapefruit", "type" : "citrus" }])
```

The following aggregration operation uses `$set` to replace the `_id` field of each document with the value of the `item` field, and replaces the `item` field with a string `"fruit"`.

```
db.fruits.aggregate( [  { $set: { _id : "$item", item: "fruit" } }] )
```

The operation returns the following:

```
{ "_id" : "tangerine", "item" : "fruit", "type" : "citrus" }{ "_id" : "lemon", "item" : "fruit", "type" : "citrus" }{ "_id" : "grapefruit", "item" : "fruit", "type" : "citrus" }
```

### Add Element to an Array[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#add-element-to-an-array "Permalink to this heading")

Create a sample `scores` collection with the following:

```
db.scores.insertMany([   { _id: 1, student: "Maya", homework: [ 10, 5, 10 ], quiz: [ 10, 8 ], extraCredit: 0 },   { _id: 2, student: "Ryan", homework: [ 5, 6, 5 ], quiz: [ 8, 8 ], extraCredit: 8 }])
```

You can use [`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) with a [`$concatArrays`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/concatArrays/#mongodb-expression-exp.-concatArrays) expression to add an element to an existing array field. For example, the following operation uses [`$set`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#mongodb-pipeline-pipe.-set) to replace the `homework` field with a new array whose elements are the current `homework` array concatenated with another array containing a new score `[ 7 ]`.

```
db.scores.aggregate([   { $match: { _id: 1 } },   { $set: { homework: { $concatArrays: [ "$homework", [ 7 ] ] } } }])
```

The operation returns the following:

```
{ "_id" : 1, "student" : "Maya", "homework" : [ 10, 5, 10, 7 ], "quiz" : [ 10, 8 ], "extraCredit" : 0 }
```

### Creating a New Field with Existing Fields[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/set/#creating-a-new-field-with-existing-fields "Permalink to this heading")

Create a sample `scores` collection with the following:

```
db.scores.insertMany([   { _id: 1, student: "Maya", homework: [ 10, 5, 10 ], quiz: [ 10, 8 ], extraCredit: 0 },   { _id: 2, student: "Ryan", homework: [ 5, 6, 5 ], quiz: [ 8, 8 ], extraCredit: 8 }])
```

The following aggregation operation adds a new field `quizAverage` to each document that contains the average of the `quiz` array.

```
db.scores.aggregate( [   {      $set: {         quizAverage: { $avg: "$quiz" }      }   }] )
```

The operation returns the following documents:

```
[   {      _id: 1,      student: 'Maya',      homework: [ 10, 5, 10 ],      quiz: [ 10, 8 ],      extraCredit: 0,      quizAverage: 9   },   {      _id: 2,      student: 'Ryan',      homework: [ 5, 6, 5 ],      quiz: [ 8, 8 ],      extraCredit: 8,      quizAverage: 8   }]
```


---
---
</details>


<details>
    <summary>
        <h2><a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort" target="_blank">$sort</a></h2>
    <img src="https://www.mongodb.com/docs/manual/assets/link.svg" alt="Permalink to this definition">
    <p>Sorts all input documents and returns them to the pipeline in sorted order.</p>
    <h4> SQL Equivalent: </br>
    <pre>
    SELECT *
    FROM collection
    ORDER BY field1 ASC, field2 DESC, ...;
    </pre>
    </h4>
    </summary>



The [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) stage has the following prototype form:

```
{ $sort: { <field1>: <sort order>, <field2>: <sort order> ... } }
```

[`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) takes a document that specifies the field(s) to sort by and the respective sort order. `<sort order>` can have one of the following values:

|Value|Description|
|---|---|
|`1`|Sort ascending.|
|`-1`|Sort descending.|
|`{ $meta: "textScore" }`|Sort by the computed `textScore` metadata in descending order. See [Text Score Metadata Sort](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#std-label-sort-pipeline-metadata) for an example.|

If sorting on multiple fields, sort order is evaluated from left to right. For example, in the form above, documents are first sorted by `<field1>`. Then documents with the same `<field1>` values are further sorted by `<field2>`.

## Behavior[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#behavior "Permalink to this heading")

### Limits[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#limits "Permalink to this heading")

You can sort on a maximum of 32 keys.

### Sort Consistency[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#sort-consistency "Permalink to this heading")

MongoDB does not store documents in a collection in a particular order. When sorting on a field which contains duplicate values, documents containing those values may be returned in any order.

If consistent sort order is desired, include at least one field in your sort that contains unique values. The easiest way to guarantee this is to include the `_id` field in your sort query.

Consider the following `restaurant` collection:

```
db.restaurants.insertMany( [   { "_id" : 1, "name" : "Central Park Cafe", "borough" : "Manhattan"},   { "_id" : 2, "name" : "Rock A Feller Bar and Grill", "borough" : "Queens"},   { "_id" : 3, "name" : "Empire State Pub", "borough" : "Brooklyn"},   { "_id" : 4, "name" : "Stan's Pizzaria", "borough" : "Manhattan"},   { "_id" : 5, "name" : "Jane's Deli", "borough" : "Brooklyn"},] )
```

The following command uses the [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) stage to sort on the `borough` field:

```
db.restaurants.aggregate(   [     { $sort : { borough : 1 } }   ])
```

In this example, sort order may be inconsistent, since the `borough` field contains duplicate values for both `Manhattan` and `Brooklyn`. Documents are returned in alphabetical order by `borough`, but the order of those documents with duplicate values for `borough` might not the be the same across multiple executions of the same sort. For example, here are the results from two different executions of the above command:

```
{ "_id" : 3, "name" : "Empire State Pub", "borough" : "Brooklyn" }{ "_id" : 5, "name" : "Jane's Deli", "borough" : "Brooklyn" }{ "_id" : 1, "name" : "Central Park Cafe", "borough" : "Manhattan" }{ "_id" : 4, "name" : "Stan's Pizzaria", "borough" : "Manhattan" }{ "_id" : 2, "name" : "Rock A Feller Bar and Grill", "borough" : "Queens" }{ "_id" : 5, "name" : "Jane's Deli", "borough" : "Brooklyn" }{ "_id" : 3, "name" : "Empire State Pub", "borough" : "Brooklyn" }{ "_id" : 4, "name" : "Stan's Pizzaria", "borough" : "Manhattan" }{ "_id" : 1, "name" : "Central Park Cafe", "borough" : "Manhattan" }{ "_id" : 2, "name" : "Rock A Feller Bar and Grill", "borough" : "Queens" }
```

While the values for `borough` are still sorted in alphabetical order, the order of the documents containing duplicate values for `borough` (i.e. `Manhattan` and `Brooklyn`) is not the same.

To achieve a _consistent sort_, add a field which contains exclusively unique values to the sort. The following command uses the [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) stage to sort on both the `borough` field and the `_id` field:

```
db.restaurants.aggregate(   [     { $sort : { borough : 1, _id: 1 } }   ])
```

Since the `_id` field is always guaranteed to contain exclusively unique values, the returned sort order will always be the same across multiple executions of the same sort.

## Examples[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#examples "Permalink to this heading")

### Ascending/Descending Sort[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#ascending-descending-sort "Permalink to this heading")

For the field or fields to sort by, set the sort order to `1` or `-1` to specify an ascending or descending sort respectively, as in the following example:

```
db.users.aggregate(   [     { $sort : { age : -1, posts: 1 } }   ])
```

This operation sorts the documents in the `users` collection, in descending order according by the `age` field and then in ascending order according to the value in the `posts` field.

When comparing values of different [BSON types](https://www.mongodb.com/docs/manual/reference/bson-types/#std-label-bson-types) in sort operations, MongoDB uses the following comparison order, from lowest to highest:

1. MinKey (internal type)
    
2. Null
    
3. Numbers (ints, longs, doubles, decimals)
    
4. Symbol, String
    
5. Object
    
6. Array
    
7. BinData
    
8. ObjectId
    
9. Boolean
    
10. Date
    
11. Timestamp
    
12. Regular Expression
    
13. MaxKey (internal type)
    

For details on the comparison/sort order for specific types, see [Comparison/Sort Order.](https://www.mongodb.com/docs/manual/reference/bson-type-comparison-order/#std-label-bson-types-comparison-order)

### Text Score Metadata Sort[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#text-score-metadata-sort "Permalink to this heading")

For a pipeline that includes a [`$text`](https://www.mongodb.com/docs/manual/reference/operator/query/text/#mongodb-query-op.-text) search, you can sort by descending relevance score using the [`{ $meta: "textScore" }`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/meta/#mongodb-expression-exp.-meta) expression. In the `{ <sort-key> }` document, set the [`{ $meta: "textScore" }`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/meta/#mongodb-expression-exp.-meta) expression to an arbitrary field name. The field name is ignored by the query system. For example:

```
db.users.aggregate(   [     { $match: { $text: { $search: "operating" } } },     { $sort: { score: { $meta: "textScore" }, posts: -1 } }   ])
```

This operation uses the [`$text`](https://www.mongodb.com/docs/manual/reference/operator/query/text/#mongodb-query-op.-text) operator to match the documents, and then sorts first by the `"textScore"` metadata in descending order, and then by the `posts` field in descending order. The `score` field name in the sort document is ignored by the query system. In this pipeline, the `"textScore"` metadata is not included in the projection and is not returned as part of the matching documents. See [`$meta`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/meta/#mongodb-expression-exp.-meta) for more information.


---
---
</details>

<details>
    <summary>
        <h2><a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind" target="_blank">$unwind</a></h2>
    <img src="https://www.mongodb.com/docs/manual/assets/link.svg" alt="Permalink to this definition">
    <p>Deconstructs an array field from the input documents to output a document for <code>_each_</code> element. Each output document is the input document with the value of the array field replaced by the element.</p>
    <h4> Not directly applicable in SQL; use JOIN to achieve similar functionality for one-to-many relationships.
    </h4>
    </summary>



## Syntax[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#syntax "Permalink to this heading")

You can pass a field path operand or a document operand to unwind an array field.

### Field Path Operand[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#field-path-operand "Permalink to this heading")

You can pass the array field path to [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind). When using this syntax, [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) does not output a document if the field value is null, missing, or an empty array.

```
{ $unwind: <field path> }
```

When you specify the field path, prefix the field name with a dollar sign `$` and enclose in quotes.

### Document Operand with Options[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#document-operand-with-options "Permalink to this heading")

You can pass a document to [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) to specify various behavior options.

```
{  $unwind:    {      path: <field path>,      includeArrayIndex: <string>,      preserveNullAndEmptyArrays: <boolean>    }}
```

|Field|Type|Description|
|---|---|---|
|[path](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-path)|string|Field path to an array field. To specify a field path, prefix the field name with a dollar sign `$` and enclose in quotes.|
|[includeArrayIndex](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-includeArrayIndex)|string|Optional. The name of a new field to hold the array index of the element. The name cannot start with a dollar sign `$`.|
|[preserveNullAndEmptyArrays](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-preserveNullAndEmptyArrays)|boolean|Optional.<br><br>- If `true`, if the `path` is null, missing, or an empty array, [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) outputs the document.<br>    <br>- If `false`, if `path` is null, missing, or an empty array, [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) does not output a document.<br>    <br><br>The default value is `false`.|

## Behaviors[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#behaviors "Permalink to this heading")

### Non-Array Field Path[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#non-array-field-path "Permalink to this heading")

- When the operand does not resolve to an array, but is not missing, `null`, or an empty array, `$unwind` treats the operand as a single element array.
    
- When the operand is `null`, missing, or an empty array `$unwind` follows the behavior set for the [preserveNullAndEmptyArrays](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-preserveNullAndEmptyArrays) option.
    

### Missing Field[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#missing-field "Permalink to this heading")

If you specify a path for a field that does not exist in an input document or the field is an empty array, [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind), by default, ignores the input document and will not output documents for that input document.

To output documents where the array field is missing, null or an empty array, use the [preserveNullAndEmptyArrays](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-preserveNullAndEmptyArrays) option.

## Examples[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#examples "Permalink to this heading")

### Unwind Array[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#unwind-array "Permalink to this heading")

In [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), create a sample collection named `inventory` with the following document:

```
db.inventory.insertOne({ "_id" : 1, "item" : "ABC1", sizes: [ "S", "M", "L"] })
```

The following aggregation uses the [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) stage to output a document for each element in the `sizes` array:

```
db.inventory.aggregate( [ { $unwind : "$sizes" } ] )
```

The operation returns the following results:

```
{ "_id" : 1, "item" : "ABC1", "sizes" : "S" }{ "_id" : 1, "item" : "ABC1", "sizes" : "M" }{ "_id" : 1, "item" : "ABC1", "sizes" : "L" }
```

Each document is identical to the input document except for the value of the `sizes` field which now holds a value from the original `sizes` array.

### Missing or Non-array Values[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#missing-or-non-array-values "Permalink to this heading")

Consider the `clothing` collection:

```
db.clothing.insertMany([  { "_id" : 1, "item" : "Shirt", "sizes": [ "S", "M", "L"] },  { "_id" : 2, "item" : "Shorts", "sizes" : [ ] },  { "_id" : 3, "item" : "Hat", "sizes": "M" },  { "_id" : 4, "item" : "Gloves" },  { "_id" : 5, "item" : "Scarf", "sizes" : null }])
```

[`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) treats the `sizes` field as a single element array if:

- the field is present,
    
- the value is not null, and
    
- the value is not an empty array.
    

Expand the `sizes` arrays with [`$unwind`:](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind)

```
db.clothing.aggregate( [ { $unwind: { path: "$sizes" } } ] )
```

The [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) operation returns:

```
{ _id: 1, item: 'Shirt', sizes: 'S' },{ _id: 1, item: 'Shirt', sizes: 'M' },{ _id: 1, item: 'Shirt', sizes: 'L' },{ _id: 3, item: 'Hat', sizes: 'M' }
```

- In document `"_id": 1`, `sizes` is a populated array. [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) returns a document for each element in the `sizes` field.
    
- In document `"_id": 3`, `sizes` resolves to a single element array.
    
- Documents `"_id": 2, "_id": 4`, and `"_id": 5` do not return anything because the `sizes` field cannot be reduced to a single element array.
    

## NOTE

The `{ path: <FIELD> }` syntax is optional. The following [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) operations are equivalent.

```
db.clothing.aggregate( [ { $unwind: "$sizes" } ] )db.clothing.aggregate( [ { $unwind: { path: "$sizes" } } ] )
```

### `preserveNullAndEmptyArrays` and `includeArrayIndex`[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#preservenullandemptyarrays-and-includearrayindex "Permalink to this heading")

The [`preserveNullAndEmptyArrays`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-ex-preservedNull) and [`includeArrayIndex`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-ex-includeArrayIndex) examples use the following collection:

```
db.inventory2.insertMany([   { "_id" : 1, "item" : "ABC", price: NumberDecimal("80"), "sizes": [ "S", "M", "L"] },   { "_id" : 2, "item" : "EFG", price: NumberDecimal("120"), "sizes" : [ ] },   { "_id" : 3, "item" : "IJK", price: NumberDecimal("160"), "sizes": "M" },   { "_id" : 4, "item" : "LMN" , price: NumberDecimal("10") },   { "_id" : 5, "item" : "XYZ", price: NumberDecimal("5.75"), "sizes" : null }])
```

#### `preserveNullAndEmptyArrays`[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#preservenullandemptyarrays "Permalink to this heading")

The following [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) operation uses the [preserveNullAndEmptyArrays](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-preserveNullAndEmptyArrays) option to include documents whose `sizes` field is null, missing, or an empty array.

```
db.inventory2.aggregate( [   { $unwind: { path: "$sizes", preserveNullAndEmptyArrays: true } }] )
```

The output includes those documents where the `sizes` field is null, missing, or an empty array:

```
{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "S" }{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "M" }{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "L" }{ "_id" : 2, "item" : "EFG", "price" : NumberDecimal("120") }{ "_id" : 3, "item" : "IJK", "price" : NumberDecimal("160"), "sizes" : "M" }{ "_id" : 4, "item" : "LMN", "price" : NumberDecimal("10") }{ "_id" : 5, "item" : "XYZ", "price" : NumberDecimal("5.75"), "sizes" : null }
```

#### `includeArrayIndex`[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#includearrayindex "Permalink to this heading")

The following [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) operation uses the [includeArrayIndex](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-includeArrayIndex) option to include the array index in the output.

```
db.inventory2.aggregate( [  {    $unwind:      {        path: "$sizes",        includeArrayIndex: "arrayIndex"      }   }])
```

The operation unwinds the `sizes` array and includes the array index in the new `arrayIndex` field. If the `sizes` field does not resolve to a populated array but is not missing, null, or an empty array, the `arrayIndex` field is `null`.

```
{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "S", "arrayIndex" : NumberLong(0) }{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "M", "arrayIndex" : NumberLong(1) }{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "L", "arrayIndex" : NumberLong(2) }{ "_id" : 3, "item" : "IJK", "price" : NumberDecimal("160"), "sizes" : "M", "arrayIndex" : null }
```

### Group by Unwound Values[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#group-by-unwound-values "Permalink to this heading")

In [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), create a sample collection named `inventory2` with the following documents:

```
db.inventory2.insertMany([  { "_id" : 1, "item" : "ABC", price: NumberDecimal("80"), "sizes": [ "S", "M", "L"] },  { "_id" : 2, "item" : "EFG", price: NumberDecimal("120"), "sizes" : [ ] },  { "_id" : 3, "item" : "IJK", price: NumberDecimal("160"), "sizes": "M" },  { "_id" : 4, "item" : "LMN" , price: NumberDecimal("10") },  { "_id" : 5, "item" : "XYZ", price: NumberDecimal("5.75"), "sizes" : null }])
```

The following pipeline unwinds the `sizes` array and groups the resulting documents by the unwound size values:

```
db.inventory2.aggregate( [ // First Stage { $unwind: { path: "$sizes", preserveNullAndEmptyArrays: true } }, // Second Stage { $group: { _id: "$sizes", averagePrice: { $avg: "$price" } } }, // Third Stage { $sort: { "averagePrice": -1 }   }] )
```

First Stage:

The [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) stage outputs a new document for each element in the `sizes` array. The stage uses the [preserveNullAndEmptyArrays](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#std-label-unwind-preserveNullAndEmptyArrays) option to include in the output those documents where `sizes` field is missing, null or an empty array. This stage passes the following documents to the next stage:

```
{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "S" }{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "M" }{ "_id" : 1, "item" : "ABC", "price" : NumberDecimal("80"), "sizes" : "L" }{ "_id" : 2, "item" : "EFG", "price" : NumberDecimal("120") }{ "_id" : 3, "item" : "IJK", "price" : NumberDecimal("160"), "sizes" : "M" }{ "_id" : 4, "item" : "LMN", "price" : NumberDecimal("10") }{ "_id" : 5, "item" : "XYZ", "price" : NumberDecimal("5.75"), "sizes" : null }
```

Second Stage:

The [`$group`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group) stage groups the documents by `sizes` and calculates the average price of each size. This stage passes the following documents to the next stage:

```
{ "_id" : "S", "averagePrice" : NumberDecimal("80") }{ "_id" : "L", "averagePrice" : NumberDecimal("80") }{ "_id" : "M", "averagePrice" : NumberDecimal("120") }{ "_id" : null, "averagePrice" : NumberDecimal("45.25") }
```

Third Stage:

The [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort) stage sorts the documents by `averagePrice` in descending order. The operation returns the following result:

```
{ "_id" : "M", "averagePrice" : NumberDecimal("120") }{ "_id" : "L", "averagePrice" : NumberDecimal("80") }{ "_id" : "S", "averagePrice" : NumberDecimal("80") }{ "_id" : null, "averagePrice" : NumberDecimal("45.25") }
```

## TIP

### See also:

- [`$group`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group)
    
- [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/sort/#mongodb-pipeline-pipe.-sort)
    

### Unwind Embedded Arrays[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#unwind-embedded-arrays "Permalink to this heading")

In [`mongosh`](https://www.mongodb.com/docs/mongodb-shell/#mongodb-binary-bin.mongosh), create a sample collection named `sales` with the following documents:

```
db.sales.insertMany([  { _id: "1", "items" : [ { "name" : "pens", "tags" : ["writing", "office", "school", "stationary" ],      "price" : NumberDecimal("12.00"),      "quantity" : NumberInt("5")     },     {      "name" : "envelopes",      "tags" : [ "stationary", "office" ],      "price" : NumberDecimal("19.95"),      "quantity" : NumberInt("8")     }    ]  },  {    _id: "2",    "items" : [     {      "name" : "laptop",      "tags" : [ "office", "electronics" ],      "price" : NumberDecimal("800.00"),      "quantity" : NumberInt("1")     },     {      "name" : "notepad",      "tags" : [ "stationary", "school" ],      "price" : NumberDecimal("14.95"),      "quantity" : NumberInt("3")     }    ]  }])
```

The following operation groups the items sold by their tags and calculates the total sales amount per each tag.

```
db.sales.aggregate([  // First Stage  { $unwind: "$items" },  // Second Stage  { $unwind: "$items.tags" },  // Third Stage  {    $group:      {        _id: "$items.tags",        totalSalesAmount:          {            $sum: { $multiply: [ "$items.price", "$items.quantity" ] }          }      }  }])
```

First Stage

The first [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) stage outputs a new document for each element in the `items` array:

```
{ "_id" : "1", "items" : { "name" : "pens", "tags" : [ "writing", "office", "school", "stationary" ], "price" : NumberDecimal("12.00"), "quantity" : 5 } }{ "_id" : "1", "items" : { "name" : "envelopes", "tags" : [ "stationary", "office" ], "price" : NumberDecimal("19.95"), "quantity" : 8 } }{ "_id" : "2", "items" : { "name" : "laptop", "tags" : [ "office", "electronics" ], "price" : NumberDecimal("800.00"), "quantity" : 1 } }{ "_id" : "2", "items" : { "name" : "notepad", "tags" : [ "stationary", "school" ], "price" : NumberDecimal("14.95"), "quantity" : 3 } }
```

Second Stage

The second [`$unwind`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/unwind/#mongodb-pipeline-pipe.-unwind) stage outputs a new document for each element in the `items.tags` arrays:

```
{ "_id" : "1", "items" : { "name" : "pens", "tags" : "writing", "price" : NumberDecimal("12.00"), "quantity" : 5 } }{ "_id" : "1", "items" : { "name" : "pens", "tags" : "office", "price" : NumberDecimal("12.00"), "quantity" : 5 } }{ "_id" : "1", "items" : { "name" : "pens", "tags" : "school", "price" : NumberDecimal("12.00"), "quantity" : 5 } }{ "_id" : "1", "items" : { "name" : "pens", "tags" : "stationary", "price" : NumberDecimal("12.00"), "quantity" : 5 } }{ "_id" : "1", "items" : { "name" : "envelopes", "tags" : "stationary", "price" : NumberDecimal("19.95"), "quantity" : 8 } }{ "_id" : "1", "items" : { "name" : "envelopes", "tags" : "office", "price" : NumberDecimal("19.95"), "quantity" : 8 } }{ "_id" : "2", "items" : { "name" : "laptop", "tags" : "office", "price" : NumberDecimal("800.00"), "quantity" : 1 } }{ "_id" : "2", "items" : { "name" : "laptop", "tags" : "electronics", "price" : NumberDecimal("800.00"), "quantity" : 1 } }{ "_id" : "2", "items" : { "name" : "notepad", "tags" : "stationary", "price" : NumberDecimal("14.95"), "quantity" : 3 } }{ "_id" : "2", "items" : { "name" : "notepad", "tags" : "school", "price" : NumberDecimal("14.95"), "quantity" : 3 } }
```

Third Stage

The [`$group`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/#mongodb-pipeline-pipe.-group) stage groups the documents by the tag and calculates the total sales amount of items with each tag:

```
{ "_id" : "writing", "totalSalesAmount" : NumberDecimal("60.00") }{ "_id" : "stationary", "totalSalesAmount" : NumberDecimal("264.45") }{ "_id" : "electronics", "totalSalesAmount" : NumberDecimal("800.00") }{ "_id" : "school", "totalSalesAmount" : NumberDecimal("104.85") }{ "_id" : "office", "totalSalesAmount" : NumberDecimal("1019.60") }
```


---
---
</details>

<details>
    <summary>
        <h2><a href="https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out" target="_blank">$out</a></h2>
    <img src="https://www.mongodb.com/docs/manual/assets/link.svg" alt="Permalink to this definition">
    <p>Takes the documents returned by the aggregation pipeline and writes them to a specified collection. Starting in MongoDB 4.4, you can specify the output database.</p>
    <h4> Not directly applicable in SQL; use INSERT INTO to copy the results to another table.</br>
    </h4>
    </summary>



The [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) stage must be _the last stage_ in the pipeline. The [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) operator lets the aggregation framework return result sets of any size.

## WARNING

[`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) replaces the specified collection if it exists. See [Replace Existing Collection](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#std-label-replace-existing-collection) for details.

## Syntax[![](https://www.mongodb.com/docs/manual/assets/link.svg)](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#syntax "Permalink to this heading")

The [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) stage has the following syntax:

- Starting in MongoDB 4.4, [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) can take a document to specify the output database as well as the output collection:
    
    ```
    { $out: { db: "<output-db>", coll: "<output-collection>" } }
    ```
    
    |Field|Description|
    |---|---|
    |[db](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#std-label-out-db)|The output database name.<br><br>- For a [replica set](https://www.mongodb.com/docs/manual/replication/#std-label-replica-set) or a standalone, if the output database does not exist, [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) also creates the database.<br>    <br>- For a [sharded cluster](https://www.mongodb.com/docs/manual/sharding/#std-label-sharded-cluster), the specified output database must already exist.|
    |[coll](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#std-label-out-collection)|The output collection name.|
    
- [`$out`](https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/#mongodb-pipeline-pipe.-out) can take a string to specify only the output collection (i.e. output to a collection in the same database):
    
    ```
    { $out: "<output-collection>" } // Output collection is in the same database
    ```

</details>
