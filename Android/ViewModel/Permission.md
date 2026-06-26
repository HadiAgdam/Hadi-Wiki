

compose :
```kotlin
  
  
val context = LocalContext.current  
val lifecycleOwner = LocalLifecycleOwner.current  
  
val permissionLauncher = rememberLauncherForActivityResult(  
    contract = ActivityResultContracts.RequestPermission(),  
    onResult = viewModel::onPermissionResult  
)  
  
val progress = viewModel.progress.collectAsState()  
val playingSong = viewModel.playingSong.collectAsState()  
val playing = viewModel.playing.collectAsState()  
val hasPermission = viewModel.hasStoragePermission.collectAsState()  
  
  
DisposableEffect(lifecycleOwner) {  
    val observer = LifecycleEventObserver { _, event ->  
        if (event == Lifecycle.Event.ON_RESUME) { // back from settings  
            viewModel.checkPermission()  
        }  
    }  
    lifecycleOwner.lifecycle.addObserver(observer)  
    onDispose { lifecycleOwner.lifecycle.removeObserver(observer) }  
}  
  
if (!hasPermission.value) {  
    LaunchedEffect(Unit) {  
        permissionLauncher.launch(Manifest.permission.READ_EXTERNAL_STORAGE)  
    }  
    PermissionDialog {  
        val intent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS).apply {  
            data = Uri.fromParts("package", context.packageName, null)  
        }  
        context.startActivity(intent)  
    }  
} else { // content
```


permission dialog :
```kotlin
@Composable  
fun PermissionDialog(onConfirmClick: () -> Unit) {  
    AlertDialog(  
        icon = {},  
        title = { Text(stringResource(R.string.permission_dialog_title)) },  
        text = { Text(stringResource(R.string.permission_dialog_content)) },  
        onDismissRequest = {},  
        confirmButton = { Button(onConfirmClick) { Text("open settings") } })  
}  
```


viewMode. :
```kotlin
fun checkPermission() {  
    getApplication<Application>().applicationContext.let { context ->  
        (context.checkSelfPermission(Manifest.permission.READ_EXTERNAL_STORAGE) == PackageManager.PERMISSION_GRANTED).let { granted ->  
            if (granted && !_hasStoragePermission.value) { // re loading musics  
                SongRepository.initialize(context)  
                MusicPlayer.initialize(context)  
                _tracks.value =  
                    SongRepository.getSongs().onEach { track -> Log.e("track", track.title) }  
            }  
            _hasStoragePermission.value = granted  
        }  
  
    }}  
  
fun onPermissionResult(granted: Boolean) {  
    _hasStoragePermission.value = granted  
  
}
```