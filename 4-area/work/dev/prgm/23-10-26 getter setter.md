--- 

creation date: 23-10-26 
modification date: 23-10-26 
tag: 

---



```js
const obj = { 
	_value: 0, 
	get value() { return this._value; }, 
	set value(newValue) { this._value = newValue; }
}; 
obj.value = 5;
```