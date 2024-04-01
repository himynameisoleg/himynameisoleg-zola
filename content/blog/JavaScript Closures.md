+++
title = "JavaScript Closures"
date = 2020-02-13
+++

A Closure in JavaScript gives you access to an outer funtion's scope from an inner function. This is useful for a number of things, but first lets take a look at how we declare a local variable. 

For example: 

```javascript
function getDinner() {
  var dinner = "steak";

  function whatsForDinner() {
    console.log(dinner);
  }

  return whatsForDinner();
}

var todaysDinner = getDinner();
todaysDinner(); // steak
```

Notice how the inner function **whatsForDinner** was able to access dinner directly without having to pass it in as a parameter.

A possible use case for a closure is to "emulate" private methods and "instance varaiables" similar to what we would see in a traditional OOP language. To do this lets create a self invoking function - a function that calls itself:

```javascript
var shoppingCart = (function (){
  // these will be the private attributes
  var totalItems = 0;

  function addItems(num) {
    totalItems += num;
  }

  // and we return the publicly accessible functions
  return {
    addToCart: function() {
      addItems(1);
    },
    removeFromCart: function() {
      if (totalItems > 0) {
        addItems(-1);
      }
    },
    checkItems: function() {
      return totalItems;
    }
  }
})(); // and self-invoke at the end

console.log(shoppingCart.checkItems()) // 0
console.log(shoppingCart.addToCart())
console.log(shoppingCart.checkItems()) // 1

// if we try to access totalItems
console.log(shoppingCart.totalItems) // undefined
```

Notice that by creating a closure using a self-invoking function, we are able to have private variable and methods. This pattern is also referred to as a Module and is useful for managing scope of global variables in JavaScript.

