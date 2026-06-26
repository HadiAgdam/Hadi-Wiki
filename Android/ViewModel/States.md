

### Using mutableStateOf

```kotlin
val string: String by mutableStateOf("")
	private set
```

```kotlin
Text(viewmodel.string)
```


### Using StateFlow

```kotlin
private val _string = MutableStateFlow("")
val string: StateFlow<String> = _string
```

```kotlin
fun updateText() {
	_string.value = "new text"
}
```


View
```kotlin
val string by viewmodel.string.collectAsState()
```


- MutableState is designed only for compose. You cannot use it in data layer.
- Can use this inside coroutines
- Can be updated from a Thread