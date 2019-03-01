## Tips and tricks:

* add `debugger` text in the source code, so that chrome developer takes to that point when debugging. Remove them, once done debugging

## lint errors and fixes Fixes:

* no-undef lint error fix: can be added global section of webpack.config
https://github.com/chaijs/type-detect/issues/98
* lint error while using single return statement in arrow function:
unexpected block statement surrounding arrow body

## Code Snippets

* Filter an array based on type
```javascript
const requiredOrders = orders.filter(order => order.required);
```

* Converting array to JSON Data
```javascript
function convertToJson(items) {
	return JSON.parse(JSON.stringify(items));
}
```

* Get the data from event object
```javascript
saveUserData(event) {
	var field = event.target.name;
	var value = event.target.value;
	this.state.author[field] = value;

	return this.setState({ author: this.state.author});
}
```

* Passing Parameters to function from render:
```javascript
onClick={() => this.deleteRow(index)}
```

* Unique Array: Pass concatenated array as input
```javascript
uniqueArray (arrArg) {
 return arrArg.filter((elem, pos, arr) => {
	 return arr.indexOf(elem) == pos;
	});
}
// ES-5:
array1 = array1.filter(function(val) {
	return array2.indexOf(val) == -1;
});
// ES6:
array1 = array1.filter(val => !array2.includes(val));
```

* Sorting array based on Parameters:
```javascript
array.sort((last, first) => last.displayValue > first.displayValue);
```

* Debouncing the search to happen after certain amount time use lodash.debounce
```javascript
doSearch = debounce(() => {}, 300);
handleSearch = (event) => {
	this.setState({ searchTerm: event.target.value}, () => {
		this.doSearch();
	});
}
```

* Perform search on object using lodash.pickby
```javascript
render() {
	let { articles, searchTerm } = this.state;

	if (searchTerm) {
		articles = pickBy(articles, (value) => {
			return value.title.match(searchTerm) || value.body.match(searchTerm);
		});
	}
}
```

* Get Cheapest items first:
```javascript
function newItemsCheapestFirst(items) {
  return items
    .filter(item => item.isNew) // filters only new items
    .sort((a, b) => {
      if(a.price < b.price) {
        return -1;
      } else if(a.price > b.price) {
        return 1;
      } else {
        return 0;
      }
    });
}
```