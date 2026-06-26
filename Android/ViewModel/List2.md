
```kotlin
class ProductRepository(val db: MyDatabaseHelper) {  
  
    val list: SnapshotStateList<Product> = mutableStateListOf()  
  
    private fun load() {  
        db.readAll().let {  
            list.clear()  
            list.addAll(it)  
        }  
    }  
  
    init {  
        load()  
    }  
    fun add(name: String) {  
        db.add(CreateProductViewModel().apply { this.name = name })  
        load()  
    }  
}
```



```kotlin
LazyColumn {  
    items(repo.list.toMutableStateList()) {  
        Text(it.name)  
    }  
}
```


