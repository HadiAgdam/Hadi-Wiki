

it creates an isolated CSS boundary so styles inside it don't leak out to the rest of your site, and global site styles don't accidentally ruin the injected snippet.



### Example

```html
<html>

    <head>

        <title>This is DOM test</title>

    </head>

    <body>

        <div id="host">

            <h2>This one uses shadow DOM</h2>

        </div>

        <div>

            <h2>This one doesn't use shadow DOM</h2>

        </div>

  

        <script>

            const host = document.getElementById('host');

            const shadow = host.attachShadow({

                mode: 'open'

            });

  

            shadow.innerHTML = `

  

            <style>

                h2 {

                    color: red;

                    font-size: 30px;

                }

            </style>

                <h2> using shadow </h2>

            `;

        </script>

    </body>

</html>
```


### Implementation

You just need to create a `div` container. Then using JS going to put elements inside the `div`.
```html
<div id="host"></div>
```

```js

const host = document.getElementById('host');
const shadow = host.attachShadow({
	mode: 'open'
});

shadow.innerHTML = `

<style>

	h1 {
		color: red;
	}

</style>

<h1>H1 using DOM</h1>

`;
```