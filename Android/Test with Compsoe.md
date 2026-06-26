

```kotlin
class MyComposeTest {  
  
    @get:Rule  
    val composeTestRule = createComposeRule()  
  
  
    @Test  
    fun test1() {  
        composeTestRule.setContent {  
            MainScreen()  
        }  
  
        composeTestRule.onNodeWithText("a").performClick()  
  
        composeTestRule.onNodeWithText("b").assertExists()  
    }  
}
```

