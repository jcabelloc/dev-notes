# Express Notes

### Basic Express Configuration

```javascript
// In the app.js file
var express = require("express");
var app=express();

app.get("/", function(req, res){
    res.send("Hi there!");
});

app.get("/bye", function(req, res){
    res.send("See you later");
})

app.listen(3000, function(){
    console.log("Started");
});
```

### Routes and Route Parameters
```javascript
// In the app.js file
// ...
app.get("/r/:subTopicName", function(req, res){
    var subTopic = req.params.subTopicName;
    res.send("Welcome to the " + subTopic + " Sub Topic");
});

app.get("/r/:subTopicName/comments/:id/:title", function(req, res){
    console.log(req.params);
    res.send("Welcome to the comments section");
});
// Default route
app.get("*", function(req, res) {
    res.send("Nothing to SHOW!!!!");
});

// ...
```
### Install EJS package
EJS: Embedded JavaScript

```bash
# To use render
npm install ejs --save
```

### how to use render and pass data objects
```javascript
// In the app.js file
// ...
app.get("/love/:thing", function(req, res){
    var thing = req.params.thing;
    res.render("love.ejs", {thingVar: thing});
})
// ...
```
```html -ejs
<!-- In the views/love.ejs file -->
<h1>Love with: <%= thingVar.toUpperCase %></h1>
```
### if and loop statements

```html -ejs
<% if (thingVar === "dog") { %>
    <p>Good Choice</p>
<% }else { %>
    <p>Sorry... but it is a bad choice!!</p>
<% } %>
```

```html -ejs
<h1>The posts page</h1>
<% for(var i = 0; i < posts.length; i++) { %>
    <li> <%= posts[i].title %> - <strong><%= posts[i].author %></strong></li>
<% } %>

<% posts.forEach(function(post) { %>
    <li> 
        <%= post.title %> - <strong><%= post.author %></strong>
    </li>
<% }) %>
```

### Add an asset such as css file

Create a (i.e) app.css file in the public directory
```html -ejs
<!-- Add this to the EJB file -->
<link rel="stylesheet" href="app.css">

```

Update the app.js file, including the following instruction
```javascript
// ...
app.use(express.static("public"));
// ...
```

In case we add bootstrap, we just need to add the link reference
```html -ejs
    <head>
            <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    </head>
```

### Refers to views without "ejs" extension
Update the app.js file, including the following instruction
```javascript
// ...
app.set("view engine", "ejs");
// ...
```

### Adding Partials

Create a header.ejs and footer.ejs inside views/partials
header.ejs
```html -ejs
<!DOCTYPE html>
<html>
    <head>
        <title>DEMO APP</title>
        <link rel="stylesheet" href="/app.css">
    </head>
    <body>
```
footer.ejs
```html -ejs
<!DOCTYPE html>
    <p>Tramemark 2018</p>
    </body>
</html>
```

Add these partials to an ejs file
```html -ejs
<% include partials/header %>

<h1>Love with: <%= thingVar%></h1>
<p>P.S. this is the love.ejs file</p>
<% if (thingVar === "dog") { %>
    <p>Good Choice</p>
<% }else { %>
    <p>Sorry... but it is a bad choice!!</p>
<% } %>

<% include partials/footer %>
```



### Adding Body Parser for reading Body in Post Requests

```bash
# Install body parser
npm install body-parser --save
```

```javascript
// Add Body Parser to our app.js file
//...
var bodyParser = require("body-parser");

app.use(bodyParser.urlencoded({extended: true}));
//...
```

```html -ejs
<!-- Add a Form with POST request-->
<form action="/addFriend" method="POST">
    <input type="text" name="newFriend" placeholder="name">
    <button>New Friend</button>
</form>
```

```javascript
// create a route in our app.js file for listening to POST requests
//...
app.post("/addFriend", function(req, res){
    var newFriend = req.body.newFriend;
    friends.push(newFriend);
    res.redirect("/friends");
});
//...
```

### Use of Redirect to Navigate to other Router
```javascript
// Use redirect to navigate other route (app.js)
//...
app.post("/addFriend", function(req, res){
    var newFriend = req.body.newFriend;
    friends.push(newFriend);
    res.redirect("/friends");
});
//...
```


### WEB requests

Install the Request Package
```bash
npm install request
``` 
Make a request by editing the js file (i.e app.js)
```javascript
var request = require('request');
request('http://www.google.com', function (error, response, body) {
    if (!error && response.statusCode == 200 ){
        console.log(body);
    }
});
```

### API WEB requests and JSON parsing

```javascript
var express = require("express");
var app = express();
var request = require("request");

var url = "https://query.yahooapis.com/v1/public/yql?q=select%20astronomy.sunset%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22maui%2C%20hi%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
request(url, function (error, response, body) {
    if (!error && response.statusCode == 200 ){
        var parsedData = JSON.parse(body);
        console.log(parsedData.query.results.channel.astronomy.sunset);
    }
});
```

```javascript
//other example
var express = require("express");
var app = express();
var request = require("request");

app.get("/results", function(req, res){
    var url = "http://www.omdbapi.com/?s=happiness&apikey=thewdb";
    request(url, function (error, response, body) {
        if (!error && response.statusCode == 200 ){
            var parsedData = JSON.parse(body);
            res.send(parsedData.Search[0].Title);
        }
    });
});
//...
```

### API Dynamic Search - Use of query

```html
<!-- Request Form-->
<h1> Search for Movies</h1>
<form action="/results" method="GET">
    <input type="text" name="search" placeholder="search name movie"> 
    <input type="submit">

</form>
```
```javascript
// app.js file
var express = require("express");
var app = express();
var request = require("request");

app.set("view engine", "ejs");

app.get("/", function(req, res){
    res.render("search")
})

app.get("/results", function(req, res){
    var query = req.query.search;
    var url = "http://www.omdbapi.com/?s=" + query + "&apikey=thewdb";
    request(url, function (error, response, body) {
        if (!error && response.statusCode == 200 ){
            var moviesData = JSON.parse(body);
            res.render("results", {data: moviesData});
        }
    });
});
app.listen(3000, function(){
    console.log("Server Movie App Started");
});

```
```html -ejs
<!-- HTML Results-->
<h1> Movie Results</h1>
<ul>
    <% data.Search.forEach(function(movie){ %>
        <li> <%= movie.Title %> - <%= movie.Year%></li>
    <% }) %>
</ul>
<a href="/">Search Again</a>
```

### Restful routes

name    url             verb        desc
=====================================================
INDEX   /students           GET     Display a list of all students
NEW     /students/new       GET     Display Form to make a new student
CREATE  /students           POST    Add a new student
SHOW    /students/:id       GET     Shows info about one student
EDIT    /students/:id/edit  GET     Show Edit Form for one Student
UPDATE  /students/:id       PUT     Update a Student, then redirect somewhere
DESTROY /students/:id       DELETE  Delete a student, then redirect somewhere