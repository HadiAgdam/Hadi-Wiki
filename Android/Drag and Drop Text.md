

```kotlin
Box(Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {  
    Box(modifier=Modifier  
        .size(200.dp)  
        .dragAndDropSource(  
            transferData = {  
                DragAndDropTransferData(  
                    clipData = ClipData.newPlainText("label", "TEXT"),  
                    flags = View.DRAG_FLAG_GLOBAL  
                )  
            }  
        )  
          
        .dragAndDropTarget( // When receives content  
            shouldStartDragAndDrop = {  
                true  
            },  
            target = remember {  
                object : DragAndDropTarget {  
                    override fun onDrop(event: DragAndDropEvent): Boolean {  
                        event.toAndroidDragEvent().clipData  
                            .getItemAt(0)  
                            .text.let {  
                                Toast.makeText(this@MainActivity, it, Toast.LENGTH_SHORT).show()  
                            }  
                        return true  
                    }  
  
                }  
  
            }  
        )  
        .background(Color.Red)  
  
    )  
}
```