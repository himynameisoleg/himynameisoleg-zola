+++
title = "Array Reduce Function"
date = 2020-01-19
+++

A higher-order function is a function that takes one or more functions as arguments, or returns a function as its result. Higher-order functions are an important concept in functional programming, as they allow you to abstract over actions rather than just values.

One common example of a higher-order function in JavaScript is the **Array.prototype.reduce()** function. This function takes an array and reduces it to a single value by applying a given function to each element in the array, starting from the left.

Here is an example of using **reduce()** to sum up the elements in an array:

```JavaScript
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((accumulator, currentValue) => accumulator + currentValue, 0);

console.log(sum); // 15
```

In this example, the **reduce()** function is called on the numbers array and passed a callback function that takes two arguments: an accumulator and a current value. The accumulator is the running total, and the current value is the current element being processed in the array. The callback function returns the sum of the accumulator and the current value, which is then assigned to the accumulator for the next iteration.

The **reduce()** function also takes an optional initial value as a second argument. In this case, we have passed 0 as the initial value, so the accumulator will start at 0 on the first iteration. If no initial value is provided, the first element in the array will be used as the accumulator and the callback function will start processing from the second element.

Here is another example of using **reduce()** to concatenate the elements of an array into a single string:

```JavaScript
const words = ['I', 'am', 'a', 'sentence'];
const sentence = words.reduce((accumulator, currentValue) => accumulator + ' ' + currentValue);

console.log(sentence); // "I am a sentence"
```

As you can see, **reduce()** is a very powerful and versatile higher-order function that can be used to perform a wide variety of operations on arrays. 
