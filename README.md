## Describing how to manipulate object or array in an immutable way 

### What is immutability?
All updates return new values, but internally structures are shared to drastically reduce memory usage (and GC thrashing). This means that if you append to a vector with 1000 elements, it does not actually create a new vector 1001-elements long. Most likely, internally only a few small objects are allocated.

ref: [Using immutable data structures](http://jlongster.com/Using-Immutable-Data-Structures-in-JavaScript#Immutable.js)

### Description
This comes handy when writing reducers for applications using Redux or other state manager frameworks which suggest you to use immutable functions. The following examples are written using ES6-7 syntax.


##### Copy an array
```js
const copyAnArray = (items) => {
	return [...items];
}

copyAnArray([1, 2, 3]);
//[1,2,3]
```

##### Splice from Array
```js
const splice = (items, start, deleteCount, ...newItems) => {
	return [
		...items.slice(0, start), 
		...newItems, 
		...items.slice(start + deleteCount)
	];
}

splice([1, 12, 13, 14, 5], 1, 3, 2, 3, 4)
//[1,2,3,4,5]
```

##### Reverse an Array
```js
const reverse = (items) => {
	return [ ...items ].reverse();
}

reverse([1, 2, 4, 3, 5])
//[5,3,4,2,1] 
```

##### Sort
```js
const sort = (items, compareFunction) => {
	return [ ...items ].sort(compareFunction);
}

sort([1, 2, 4, 3, 5], (a, b) => {
	return a - b;
});
//[1,2,3,4,5]
```

##### Remove the first element from an Array (Shift)
```js
const shift = (items) => {
	return items.slice(1);
}

shift([1, 2, 4, 3, 5])
//[2,4,3,5]
```

##### Add en element to the beginning of an Array (Unshift)
```js
const unshift = (items, newItem) => {
	return [newItem, ...items];
}

unshift([2, 4, 3, 5], 1)
//[1,2,4,3,5]
```

##### Removing the last element from an Array (Pop)
```js
const pop = (items) => {
	return items.slice(0, -1)     
}

pop([1, 2, 4, 3, 5, 6])
//[1,2,4,3,5]
```

##### Adding one element to an Array (Push)
  ```js
const push = (list, element) => {
	return [...list, element];
}

push([1, 2, 3], 4);
//[1,2,3,4] 
```

##### Remove item from an Array according to an index
  ```js
const removeItemFromAnArray = (list, index) => {
	return [
		...list.slice(0, index),
		...list.slice(index + 1)
	];
}

removeItemFromAnArray([1, 2, 3], 1);
//[1,3]
```

##### Increment an item in an Array containing numbers
 ```js
const incrementAnItemInAnArray = (list, index) => {
	return [
		...list.slice(0, index),
		list[index] + 1,
		...list.slice(index + 1)
	];
}

incrementAnItemInAnArray([1, 2, 3], 1);
//[1,3,3]
```

##### Replace an item in an array 
  ```js
const replaceAnItemInAnArray = (list, index, newItem) => {
	return [
		...list.slice(0, index),
		newItem,
		...list.slice(index + 1)
	];
}

replaceAnItemInAnArray([1, 3, 3], 1, 2);
//[1,2,3] 
```

##### Change a boolean in an object
 ```js
const changeBooleanInAnObject = (item) => {
	return Object.assign({}, item, {
		isActive: !item.isActive
	});
}

changeBooleanInAnObject({isActive: false});
//{"isActive":true} 
```

##### Change a boolean in an object (with Babel Stage 2 preset)
 ```js
const changeBooleanInAnObject = (item) => {
	return {
		...item,
		isActive: !item.isActive
	};
}

changeBooleanInAnObject({isActive: false});
//{"isActive":true} 
```

#####  Toggle every isActive property in an array of objects
 ```js
const toggleAllIsActiveProperty = (list) => {
	return list.map(item => {
		return {
			...item,
			isActive: !item.isActive
		}
	});
}

toggleAllIsActiveProperty([{isActive: false}, {isActive: false}, {isActive: false}]);
//[{"isActive":true},{"isActive":true},{"isActive":true}]
```

Comments or issues are welcome