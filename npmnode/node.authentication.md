# NodeJS Authentication

### Install packages

```bash
npm install express mongoose --save
npm install passport passport-local --save
npm install passport-local-mongoose --save
npm install body-parser express-session --save
npm install ejs --save
```

### Create the user model (./models/user.js)
```javascript
var mongoose = require("mongoose");
var passportLocalMongoose = require("passport-local-mongoose");

var UserSchema = new mongoose.Schema({
    username: String,
    password: String
});

UserSchema.plugin(passportLocalMongoose);

module.exports = mongoose.model("User", UserSchema);
```

### Set up packages and initial configuration (app.js)
```javascript
var express                 = require("express"),
    mongoose                = require("mongoose"),
    passport                = require("passport"),
    bodyParser              = require("body-parser"),
    User                    = require("./models/user"),
    LocalStrategy           = require("passport-local"),
    passportLocalMongoose   = require("passport-local-mongoose");

mongoose.connect("mongodb://localhost/auth_demo_app");

var app = express();
app.set("view engine", "ejs");

//Enabling session support is optional, though it is recommended for most applications
app.use(require("express-session")({
    secret: "Neque porro quisquam est qui dolorem ipsum quia dolor sit amet",
    resave: false,
    saveUninitialized: false
}));

// In a Express-based app, passport.initialize() middleware is required to initialize Passport
app.use(passport.initialize());
// If the app uses persistent login sessions, passport.session() middleware must also be used
app.use(passport.session());

//  To support login sessions, Passport will serialize and deserialize user instances to and from the session.
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());

app.get("/", function(req, res){
    res.render("home");
});

app.get("/secret", function(req, res){
    res.render("secret");
});

app.listen(3000, function(){
    console.log("server started...");
});

```

## Adding Regiter routes and Register form

### Register Routes (app.js)
```javascript
//...
app.use(bodyParser.urlencoded({extended: true}));
//...
// Auth Routes
// show sign up form
app.get("/register", function(req, res){
    res.render("register");
});

// handling user sign up
app.post("/register", function(req, res){

    User.register(new User({username: req.body.username}), req.body.password, function(err, user){
        if(err){
            console.log(err);
            return res.redirect("/register");
        }
        passport.authenticate("local")(req, res, function(){
            res.redirect("/secret");
        });
    });
});
//...
```
### Register form (register.ejs)
```html ejs
<form action="/register" method="POST">
    <input type="text" name="username" placeholder="username">
    <input type="password" name="password" placeholder="password">
    <button>Submit</button>
</form>
```

## Adding Login Routes and Login Form

### Login Routes (app.js)
```javascript
//...
passport.use(new LocalStrategy(User.authenticate()));
//...
// LOGIN ROUTES
// render login form
app.get("/login", function(req, res){
    res.render("login");
})
// Login Logic
// Middleware
app.post("/login", passport.authenticate("local", {
    successRedirect: "/secret",
    failureRedirect: "/login"
}), function(req, res){
});
//...
```

### Login Form
```html ejs
<form action="/login" method="POST">
    <input type="text" name="username" placeholder="username">
    <input type="password" name="password" placeholder="password">
    <button>Submit</button>
</form>
```

## Adding Logout and isLoggedIn Middleware

### Adding Logout(app.js)
```javascript
//...
app.get("/logout", function(req, res){
    req.logout();
    res.redirect("/")
});
//...
```

### isLoggedIn Middleware (app.js)
```javascript
//...
app.get("/secret", isLoggedIn, function(req, res){
    res.render("secret");
});
//...
function isLoggedIn(req, res, next){
    if(req.isAuthenticated()){
        return next();
    }
    res.redirect("/login");
};
//...
```

### Middleware to pass the current user to all templates (app.js)
```javascript
app.use(function(req, res, next){
    res.locals.currentUser = req.user;
    next();
});
```