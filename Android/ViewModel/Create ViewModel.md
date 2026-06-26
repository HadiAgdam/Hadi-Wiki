

### Using viewModel()


```kotlin
val viewmodel: MainViewModel = viewModel()
```

you need add this to dependency

```
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.8.7")

```



### Using ViewModelFactory

```kotlin
val viewmodel: MainViewModel by viewModels<MainViewModel> { MainViewModelFactory() }
```

```kotlin
class MainViewModelFactory : ViewModelProvider.Factory {  
  
  
    override fun <T : ViewModel> create(modelClass: Class<T>): T {  
        if (modelClass.isAssignableFrom(MainViewModel::class.java)) {  
            return MainViewModel() as T  
        }  
        throw IllegalArgumentException("Unknown ViewModel class")  
    }  
  
}
```
