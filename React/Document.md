
# Project Structure

The two files to focus on first are `App.jsx` and `main.jsx`.

- `main.jsx` starts React and tells it where to render.
    
- `App.jsx` contains your main UI component.



`main.jsx`:
```jsx
import { createRoot } from 'react-dom/client'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
	<App />
)
```

`App.jsx`:
```jsx
import { useState } from 'react'
import './App.css'

  
function App() {

	const [count, setCount] = useState(0)

  

	return (
	
		<div class="container">
		
			<h1>I am Hadi Agdam</h1>
			<p>This is my first React site.</p>
			
			<h2>Count: {count}</h2>
			
			<button onClick={() => setCount(count + 1)}>
				Increase
			</button>
			
			<button onClick={() => {
				if (count > 0)
					setCount(count - 1)
			}}>
				Decrease
			</button>
			
			<button onClick={() => setCount(0)}>
				Reset
			</button>
		
		</div>
	
	)

}

  

export default App
```



# Creating Component

```jsx
function Greeting() {

	return (<h2>Hello Hadi!</h2>);

}


function App2() {

	return (
	
	<div class="container">
	
		<h1>App 2</h1>
	
		<Greeting/>
	
	</div>
	
	);

}
```

`<Greeting />` is a component, much like reusing a partial view or a reusable UI component in another framework.


# JSX

This:

```js
const name = 'Hadi';

return <h1>Hello, {name}!</h1>;
```

renders:

```jsx
<h1>Hello, Hadi!</h1>
```

JSX lets you describe UI using HTML-like syntax inside JavaScript. Use curly braces `{}` to insert JavaScript expressions.

A few differences from ordinary HTML:

| HTML                 | JSX                        |
| -------------------- | -------------------------- |
| `class="card"`       | `className="card"`         |
| `onclick="..."`      | `onClick={...}`            |
| `<img>`              | `<img />`                  |
| `style="color: red"` | `style={{ color: 'red' }}` |

JSX isn't HTML itself; Vite's tooling transforms it into JavaScript that React can use.


# State

```js
const [count, setCount] = useState(0);
```

This line creates a state variable.

- `count` is the current value.
- `setCount` updates that value.
- `0` is the initial value.


When you call:

```js
setCount(count + 1);
```

React updates the state and renders the component again with the new value.

You may be thinking, "Why not just use a regular JavaScript variable?"

Because changing a regular variable doesn't tell React to update the UI. State is how you make the UI respond to changing data.


These are same:
```js
const [count, setCount] = useState(0);


setCount(count + 1);
setCount(current => current + 1);
```




# Props — passing data between components

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

function App() {
  return (
    <div>
      <Greeting name="Hadi" />
      <Greeting name="HHH" />
    </div>
  );
}
```

Props are inputs to a component. State is data a component manages over time.


# Event

```jsx
<button onClick={() => alert('Hello!')}>
  Click me
</button>

<button onClick={() => {
	alert('Hello!');
	alert('Hello2!');
}}>
	Click to see two alerts
</button>	
```



# List

```js
const technologies = ['React', 'ASP.NET Core', 'SQL Server'];

function App() {
  return (
    <ul>
      {technologies.map(technology => (
        <li key={technology}>{technology}</li>
      ))}
    </ul>
  );
}
```



# Styling
```jsx
<p style={{ color: fruits[selected]['color'] }}>
```

*Important*: You should use `{{` instead of `{(`. Also you should not put `color` inside `"`.