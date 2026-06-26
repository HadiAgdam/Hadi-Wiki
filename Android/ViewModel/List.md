

### Using StateFlow

#### ViewModel
```kotlin
private val _listStateFlow = MutableStateFlow<List<String>>(listOf())  
val listStateFlow = _listStateFlow.asStateFlow()
```

set value
```kotlin
_listStateFlow.value = listOf("item1", "item2", "item3")
```

add
```kotlin
_listStateFlow.value += "an item"
_listStateFlow.value += listof("many", "items")
```

#### Compose
```kotlin
val listSateFlow = viewmodel.listStateFlow.collectAsState()
```

```kotlin
for (item in listSateFlow.value)   
    Text(item)
```


This is **less efficient** because it replaces the entire list, causing unnecessary recompositions.

---
### StateList

#### ViewModel
```kotlin
private val _conversations = mutableStateListOf<ConversationModel>()  
val conversations: SnapshotStateList<ConversationModel> = _conversations
```

This one allows add, remove, clear.