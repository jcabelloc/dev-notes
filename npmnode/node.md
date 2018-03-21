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

