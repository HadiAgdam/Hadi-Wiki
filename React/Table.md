


Basic `HTML` table:
```jsx
return(

	<div class="container">
	
		<h1>Tables</h1>
		
		<table>
			<thead>
				<tr>
					<th>Id</th>
					<th>Name</th>
					<th>Age</th>
				</tr>
			</thead>
				
			<tbody>
				<tr>
					<td>0</td>
					<td>Hadi</td>
					<td>21</td>
				</tr>
			</tbody>
		</table>
	</div>

)
```




loading dynamic data using `JSX`:
```jsx
const fruits = [
	{ id: 0, name: 'Banana', color: 'Yellow' },
	{ id: 1, name: 'Orange', color: 'Orange' },
	{ id: 2, name: 'Strawberry', color: 'Red' }
];
```

```jsx
<table>

	<thead>
	
		<tr>
			<th>Id</th>
			<th>Name</th>
			<th>Color</th>
		</tr>
	
	</thead>
	
	<tbody>
		{fruits.map(f => (
			<tr key={f.id}>
				<td>{f.id}</td>
				<td>{f.name}</td>
				<td>{f.color}</td>
			</tr>
		))}
	</tbody>

</table>
```