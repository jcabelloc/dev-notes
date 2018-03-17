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

### Use of render and pass of data objects
```javascript
// In the app.js file
// ...
app.get("/love/:thing", function(req, res){
    var thing = req.params.thing;
    res.render("love.ejs", {thingVar: thing});
})
// ...
```
```ejs
<!-- In the views/love.ejs file -->
<h1>Love with: <%= thingVar.toUpperCase %></h1>
```
### if and loop statements

```ejs
<% if (thingVar === "dog") { %>
    <p>Good Choice</p>
<% }else { %>
    <p>Sorry... but it is a bad choice!!</p>
<% } %>
```

```ejs
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
```ejb
<!-- Add this to the EJB file -->
<link rel="stylesheet" href="app.css">

```

Update the app.js file, including the following instruction
```javascript
// ...
app.use(express.static("public"));
// ...
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
```html
<!DOCTYPE html>
<html>
    <head>
        <title>DEMO APP</title>
        <link rel="stylesheet" href="/app.css">
    </head>
    <body>
```
footer.ejs
```html
<!DOCTYPE html>
    <p>Tramemark 2018</p>
    </body>
</html>
```

Add these partials to an ejs file
```ejs
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
