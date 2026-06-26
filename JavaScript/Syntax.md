

```js
const name = "Hadi"; // immutable
let age = 19; // mutable
```

```js
const numbers = ["one", "two", "three"];
```

```js
let user = {name: "Hadi", age: 19};
```

```js
function greet(name) {
	return "Hello, " + name;
}

const greet2 = (name) => `Hello, ${name}`;
```

```js

const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

async function fetchData() {
	console.log("Waiting...");
	await delay(2000);
	console.log("Waited!");
}

```

```js
5 == "5" // true
5 === "5" // false

null == undefined // true
null === undefined // false
```