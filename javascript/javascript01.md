# Javascript Notes

## Functions

### function declaration
```javascript
function sum(num1, num2) {
	return num1 + num2;
};
```
### function expression
```javascript
var sumNumbers = function (num1, num2) {
	return num1 + num2;
};
```
### Pass functions to other functions
```javascript
function sayHello() {
	console.log("hello");
};
setInterval(sayHello, 1000);
```
### Pass functions using anonymous functions
```javascript
setInterval(function(){
	console.log("Awesome");
}, 1000);
```

### Arrow Functions
```javascript
var multiply = (a, b) => { return a * b };
```


## Arrays
### ForEach
```javascript
var names = ["Jose", "Miguel", "Ivan", "Santiago"];

names.forEach (function(name) {
	console.log(name);
});

names.forEach (function(name, index) {
	console.log(index + ".- " + name);
});
```
### Own ForEach
```javascript
var names = ["Jose", "Miguel", "Ivan", "Santiago"];

function myForEach(theArray, theFunction) {
	for (var i = 0; i < theArray.length; i++) {
		theFunction(theArray[i]);
	}
};

myForEach(names, console.log);

myForEach(names, function(name) {
	console.log(name);
});

// Another way
var names = ["Jose", "Miguel", "Ivan", "Santiago"];

Array.prototype.myForEach = function(theFunction) {
	for (var i = 0; i < this.length; i++) {
		theFunction(this[i]);
	} 
};

names.myForEach(console.log); 

names.myForEach(function(name){
	console.log("Hey!: " + name);
});
```

## Objects
### Basics
```javascript
// declaring and initializing
var student = {};
// or
var student = new Object();

student.name = "Pedro";
student.age = 21;

var student = {
	name: "Jose",
	age: "23",
	city: "Lima"
};

student["name"];
student.name;
```

