+++
title = "Remove duplicates from an Array of Objects based on property"
date = 2020-02-06
+++

Here is a useful code snippet that allows you to pass in an array of objects, and then filter out the objects that have duplicate key values. 
Imagine you have a list of articles that you have merged from multiple sources and want to remove the duplicates based on id.

```javascript
const articlesList = [
	{
		id: 3,
		title: "Interesting Stuff",
		content: "..."
	},
	{
		id: 45,
		title: "Even more interesting",
		content: "..."
	},
	{
		id: 3, 
		title: "Interesting Stuff",
		content: "..."
	}
]
```
The snippet looks something like this:

```javascript
function removeDuplicates(myArr, prop) {
    return myArr.filter((obj, pos, arr) => {
        return arr.map(mapObj => mapObj[prop]).indexOf(obj[prop]) === pos;
    });
}

removeDuplicates(articlesList, 'id');
```