# Node Notes

### Introduction

Enter to the Node Console
```bash
node
```
Run a file with node
```bash
node <filename.js>
```

## Mongoose for MongoDB Interaction

### Adding Mongoose for MongoDB Interaction

```bash
node install mongoose
```

```javascript
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost/collegeDB");

var studentSchema = new mongoose.Schema({
    name: String,
    age: Number,
    major: String
});

var Student = mongoose.model("Student", studentSchema);

```

### Adding and Reading data to/from MongoDB by using Mongoose

```javascript
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost/collegeDB");

var studentSchema = new mongoose.Schema({
    name: String,
    age: Number,
    major: String
});

var Student = mongoose.model("Student", studentSchema);

var csStudent = new Student({
    name: "Ale",
    age: 22,
    major: "MIS"
});

csStudent.save(function(err, student){
    if(err) {
        console.log("something went wrong!");
    }else{
        console.log("Saved in the DB")
        console.log(student);
    }
    
});
// Create and save at the same time
Student.create({
    name: "Alice",
    age: 25,
    major: "medicine"
}, function(err, student){
    if(err){
        console.log(err);
    } else {
        console.log(student);
    }
});

Student.find({}, function(err, students){
    if(err){
        console.log("ERROR!");
        console.log(err);
    }else {
        console.log("All the students")
        console.log(students);
    }
});
```

### findById method
```javascript
app.get("/students/:id", function(req, res){
    Campground.findById(req.params.id, function(err, foundStudent){
        if(err){
            console.log(err);
        } else {
            res.render("show", {student: foundStudent});
        }
    });
});
```

## Body-Parser functionalities
### Sending Object from the Form to the Server

```html ejs
<div class="ui main text container segment">
    <div class="ui huge header">New BLog</div>
    <form class="ui form" action="/blogs" method="POST">
        <div class="field">
            <label>Title</label>
            <input type="text" name="blog[title]" placeholder="title">
        </div>
        <div class="field">
            <label>Image</label>
            <input type="text" name="blog[image]" placeholder="image">
        </div>
        <div class="field">
            <label>Blog Content</label>
            <textarea name="blog[body]"></textarea>
        </div>
        <input class="ui violet big basic button" type="submit" value="Submit">
    </form>

</div>
```

### Capturing the object data in the server
```javascript
app.post("/blogs", function(req, res){
    Blog.create(req.body.blog, function(err, newBlog){
        if(err){
            res.render("new");
        } else {
            res.redirect("/blogs");
        }
    });
});
```
## Method-Override

### Install method override package
```bash
npm install method-override --save
```

### Transform a POST request to a PUT request by adding "?_method=PUT"
```html ejs
<div class="ui main text container segment">
    <div class="ui huge header">Edit <%= blog.title%></div>
    <form class="ui form" action="/blogs/<%= blog._id %>?_method=PUT" method="POST">
        <div class="field">
            <label>Title</label>
            <input type="text" name="blog[title]" value="<%= blog.title %>">
        </div>
        <div class="field">
            <label>Image</label>
            <input type="text" name="blog[image]" value="<%= blog.image %>">
        </div>
        <div class="field">
            <label>Blog Content</label>
            <textarea name="blog[body]"><%= blog.body%></textarea>
        </div>
        <input class="ui violet big basic button" type="submit" value="Submit">
    </form>
</div>

```

### Listen to that PUT request (+ Use of findByIdAndUpdate() method)
```javascript
app.put("/blogs/:id", function(req, res){
    Blog.findByIdAndUpdate(req.params.id, req.body.blog, function (err, updatedBlog){
        if(err){
            res.redirect("/blogs");
        } else {
            res.redirect("/blogs/" + req.params.id);
        }
    });
});
```

### Transform a POST request to a DELETE request by adding "?_method=DELETE"

```html ejs
    <form id="delete" action="/blogs/<%= blog._id %>?_method=DELETE" method="POST">
        <button class="ui red basic button">Delete</button>
    </form>
```

### Listen to that DELETE request (+Use of findByIdAndRemove() method)
```javascript
app.delete("/blogs/:id", function(req, res){
    Blog.findByIdAndRemove(req.params.id, function(err){
        if(err){
            res.redirect("/blogs");
        } else {
            res.redirect("/blogs");
        }
    })
})
```


## Saninitizing HTML/Text Input

### Intall the express-sanitizer pacakge
```bash
npm install express-sanitizer --save
```

### Use Express sanitizer (app.js)

```javascript
//...
var expressSanitizer= require("express-sanitizer");

app.use(expressSanitizer()); // must go after bodyParser

// use inside create and update routes
app.post("/blogs", function(req, res){
    console.log(req.body);
    req.body.blog.body = req.sanitize(req.body.blog.body);
    console.log("===============");
    console.log(req.body);
    //..
}
//...
```

### show freely html content (see notation)

```html
                <p><%- blog.body %></p>
```


## Associations by using Mogoose

### Embedding Data
```javascript
var mongoose = require("mongoose");

mongoose.connect("mongodb://localhost/blog_demo");

// POST - title, content
var postSchema = new mongoose.Schema({
    title: String,
    content: String
});
var Post = mongoose.model("Post", postSchema);

// USER - email, name
var userSchema = new mongoose.Schema({
    email: String,
    name: String,
    posts: [postSchema]
});
var User = mongoose.model("User", userSchema);

var newUser = new User({
    email: "hellen@gmail.com",
    name: "Hellen Granger"
});

newUser.posts.push({
    title: "Reflections on Oranges",
    content: "They are more than delicious"
});

newUser.save(function(err, user){
    if(err){
        console.log(err);
    } else {
        console.log(user);
    }
});

/*
var newPost = new Post({
    title: "Reflections on Apples",
    content: "They are delicious"
});

newPost.save(function(err, post){
    if(err){
        console.log(err);
    } else {
        console.log(post);
    }
});
*/

User.findOne({name: "Hellen Granger"}, function(err, user){
    if(err){
        console.log(err);
    } else{
        user.posts.push({
            title: "3 things I really hate",
            content: "Lie, Lie, Lie"
        });
        user.save(function(err, user){
            if(err){
                console.log(err);
            } else {
                console.log(user);
            }
        })
    }
})
```

### Referencing Data
```javascript
var mongoose = require("mongoose");

mongoose.connect("mongodb://localhost/blog_ref");

// POST - title, content
var postSchema = new mongoose.Schema({
    title: String,
    content: String
});
var Post = mongoose.model("Post", postSchema);

// USER - email, name
var userSchema = new mongoose.Schema({
    email: String,
    name: String,
    posts: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: "Post"
        }
    ]
});

var User = mongoose.model("User", userSchema);

Post.create({
    title: "How to cook make hot hamburgues ",
    content: "rrwqlorwrwhlorwiorweiorweuio"
}, function(err, post){
    User.findOne({email:"bob@gmail.com"}, function(err, foundUser){
        if(err){
            console.log(err);
        } else {
            foundUser.posts.push(post);
            foundUser.save(function(err, data){
                if(err){
                    console.log(err);
                } else {
                    console.log(data);
                }
            })
        }
    });
});

/*
User.create({
    email: "bob@gmail.com",
    name: "Bob Bob"
});
*/

User.findOne({email: "bob@gmail.com"}).populate("posts").exec(function(err, user){
    if(err){
        console.log(err);
    } else {
        console.log(user);
    }
});

```


## Module Exports
### Create user.js and post.js files in ./models/

```javascript
var mongoose = require("mongoose");
// POST - title, content
var postSchema = new mongoose.Schema({
    title: String,
    content: String
});
module.exports = mongoose.model("Post", postSchema);
```
```javascript
var mongoose = require("mongoose");
// USER - email, name
var userSchema = new mongoose.Schema({
    email: String,
    name: String,
    posts: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: "Post"
        }
    ]
});
module.exports = mongoose.model("User", userSchema);
```

### Then require(import) those models in your app.js program
```javascript
var mongoose = require("mongoose");
mongoose.connect("mongodb://localhost/blog_ref");

// The magic is here!
var Post = require("./models/post");
var User = require("./models/user");


Post.create({
    title: "How to cook make hot wings ",
    content: "KLGSAGLASGFLASLAKLSGAFJKLS"
    //...
});
//...
```