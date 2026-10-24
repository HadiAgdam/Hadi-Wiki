
```jsx
function ChessBoardApp() {

  

return(

	<div>

		<h1>Chess board</h1>
		
		  
		
		{Array.from({ length: 8}, (_, i) => (
		
			<div class="row">
			
				{Array.from({ length: 8}, (_, j) => (
				
					<div className={(i + j) % 2 == 0? 'white' : 'black'}></div>
				
				))}
			
			</div>
		
		))}
		
	</div>

)

}
```