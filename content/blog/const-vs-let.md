+++
title = "Understanding const vs let"
date = 2020-02-02
+++

In JavaScript, **const** and **let** are both ways to declare variables, but they have some important differences.

**const** stands for "constant." When you declare a variable using **const**, you are telling the compiler that the value of the variable cannot be reassigned. For example:

```JavaScript
const pi = 3.14;
pi = 3.14159; // This would cause an error
```

On the other hand, **let** allows you to declare a variable whose value can be reassigned. For example:

```JavaScript
let x = 5;
x = 10; // This is allowed
```

One important thing to note is that **const** does not mean that the value of the variable is immutable (i.e., cannot be changed). For example, if you have a **const** variable that is an array or an object, you can still change the elements or properties of that array or object.

```JavaScript
const arr = [1, 2, 3];
arr.push(4); // This is allowed
arr = [1, 2, 3, 4, 5]; // This would cause an error
```

In general, it is a good practice to use **const** whenever you know that a variable will not need to be reassigned, as it can help to prevent accidental reassignments and make your code easier to understand. **let** should be used when you need to reassign a variable.
