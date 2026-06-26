


Compose Ui test
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
        Thread.sleep(10000)  
        composeTestRule.onNodeWithText("b").assertExists()  
    }  
}
```

Ui Automator (mix with ui test):
```kotlin
class MyComposeTest {  
  
    @get:Rule  
    val ctr = createComposeRule()  
  
  
    @Test  
    fun testScroll() {  
        val list = mutableStateListOf("a", "b", "c", "d", "e", "f", "g", "h")  
  
        ctr.setContent {  
            MyScreen(list) { list.add("item ${LocalDateTime.now()}") }  
        }  
        ctr.onNodeWithText("Add item").let { node ->  
            repeat(1) {  
                node.performClick()  
            }  
        }  
        // ctr.onNodeWithTag("tag${list.size - 1}").performScrollTo()  
        val device = UiDevice.getInstance(InstrumentationRegistry.getInstrumentation())  
        device.pressHome()  
  
  
        Thread.sleep(10000)  
    }  
  
  
}  
  
  
@Composable  
fun MyScreen(list: List<String>, onAddClick: () -> Unit) {  
  
    Scaffold { padding ->  
        Column(Modifier  
            .fillMaxSize()  
            .padding(padding)  
            .padding(horizontal = 12.dp)) {  
            Button(onClick = onAddClick) { Text("Add item") }  
  
            LazyColumn {  
                for (i in 0 until list.size) {  
                    item {  
                        Image(  
                            painter = painterResource(R.drawable.ic_launcher_background),  
                            contentDescription = null,  
                            modifier = Modifier.testTag("tag$i")  
                        )  
                        Spacer(Modifier.height(48.dp))  
                    }  
                }  
            }  
  
        }    }}
```


```kotlin
device.findObject(By.text("OK")).click() device.findObject(By.desc("Settings")).click() device.findObject(By.res("android:id/button1")).click()

device.pressHome() // Home button
device.pressBack() // Back button 
device.openNotification() // Pull down notification
device.takeScreenshot(File(...))
```