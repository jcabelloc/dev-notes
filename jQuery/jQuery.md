# JQuery Notes

### Introduction

jQuery is a DOM manipulation library


### Selectors and css method examples
```javascript

var elements = $("#preferable");
var element = $("#preferable")[0];

$("h1").css("color", "blue");

var styles = {
	color: "green",
	background: "pink",
	border: "2px solid black"
}

$("h1").css(styles);

$("li").css("color", "white");
$("li").css("font-size", "100px");
$("li").css({
	fontSize: "10px",
	border: "3px dashed purple",
	background: "rgba(85,45,20,05)"
});

$("div").css("background", "purple");

$(".highlight").css("width", "200px");
// or $("div.highlight").css("width", "200px");

$("#third").css({
	border: "4px solid orange"
});

$("div").first().css("color", "pink")
// or $("div:first-of-type").css("color", "pink")

```

### Text and HTML

```javascript
$("h1").text();
$("body").text();
$("ul").text();
// set all the <li> text to "Choices"
$("li").text("Choices");

$("ul").html();
// set 2 <li> for the <ul>s"
$("ul").html("<li> One </li> <li> Two</li>");
// set all the <li> html to Google links
$("li").html("<a href='http://www.google.com'> GO TO GOOGLE</a>")
```

### att() method
```javascript
// just show for the first
$("img").attr("src");
$("input").attr("type");
// set 
$("img").attr("src", "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/Macaca_sinica_-_01.jpg/220px-Macaca_sinica_-_01.jpg");

$("input").attr("type", "color");
$("input").attr("type", "checkbox");
$("input").attr("type", "text");

$("img:first-of-type").attr("src", "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/Macaca_sinica_-_01.jpg/220px-Macaca_sinica_-_01.jpg");
$("img").last().attr("src", "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/Macaca_sinica_-_01.jpg/220px-Macaca_sinica_-_01.jpg");
```

### val() method
```javascript
// Get the input text
$("input").val();
// Set the input text
$("input").val("Here the input");
// Clean the input text
$("input").val("");

$("select").val();
```


### classes methods

```javascript
$("h1").addClass("correct");
$("h1").removeClass("correct");
$("li").addClass("wrong");
$("li").removeClass("wrong")
$("li").addClass("correct");
$("li").toggleClass("correct");
$("li").first().toggleClass("done");

```

## Events

### click() Event
```javascript
$("h1").click(function(){
	alert("h1 click");
});

$("button").click(function(){
	$(this).css("background", "pink");
});

$("button").click(function(){
	text = $(this).text();
	console.log("You clicked: " + text);
});
```

### keypress() Event
```javascript
$("input").keypress(function(){
	console.log("You pressed a key!");
});

$("input").keypress(function(event){
	console.log(event);
});
// evaluate is key pressed is [ENTER]
$("input").keypress(function(event){
	if (event.which === 13 ) {
		alert("you hit Enter");
	};
});
```
### on() Event
* The most used jQuery method
```javascript
$("h1").on("click", function(){
	$(this).css("color", "purple");
});

$("input").on("keypress", function(){
	console.log("key pressed");
});

$("button").on("mouseenter", function(){
	console.log("Mouse Enter!");
});

$("button").on("mouseenter", function(){
	$(this).css("font-weight", "bold");
});

$("button").on("mouseleave", function(){
	$(this).css("font-weight", "normal");
});


```

### Effects 
```javascript
$("button ").on("click", function(){
    $("div").fadeOut(2000, function(){
        $(this).remove();
    });
});

$("button ").on("click", function(){
    $("div").fadeIn(2000, function(){
        
    });
});

$("button ").on("click", function(){
    $("div").fadeToggle(1000, function(){
       
    });
});

$("button ").on("click", function(){
    $("div").slideDown();
});

$("button ").on("click", function(){
    $("div").slideUp();
});

$("button ").on("click", function(){
    $("div").slideToggle();
});

$("button ").on("click", function(){
    $("div").slideToggle(1000, function(){
        console.log("SLIDE IS DONE!");
    });
});
```
