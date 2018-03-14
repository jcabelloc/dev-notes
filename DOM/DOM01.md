# DOM Notes

## Selectors

```javascript
var element = document.getElementById("firstName");
	
var elements = document.getElementByClassName("bolded");
	
var elements = document.getElementByTagName("li");

// Query Selector - obtains the first element that matches

var element = document.querySelector("#firstName");
	
var li = document.querySelector(".bolded");

var li = document.querySelector("li");


// Query Selector All
var elements = document.querySelectorAll("h1");

var elements = document.querySelectorAll(".bolded");

```

## Events

### Add a listener

```javascript
element.addEventListener(eventType, functionToCall);

button.addEventListener("click", function() {
	h1Element.textContent = "Title Updated";
});

var lis = document.querySelectorAll("li");

for (var i=0; i < lis.length; i++) {
	lis[i].addEventListener("click", function(){
		this.style.color = "pink"; // "this" inside a event listener refers to the item/element trigering it on
	});
};
```

### classList.toggle: Toggle between adding and removing a class name
```javascript
document.body.classList.toggle("red");
```

### Other Events
```javascript
var firstLi = querySelector("li");

firstLi.addEventListener("mouseover", function() {...});

firstLi.addEventListener("mouseout", function() {...});
```