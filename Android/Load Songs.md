

permission :
```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```


gradle :
```kotlin
implementation(libs.androidx.media3.exoplayer)  
implementation(libs.androidx.media3.exoplayer.dash)  
implementation(libs.androidx.media3.ui)
```

```toml
media3Exoplayer = "1.5.1"

androidx-media3-exoplayer = { module = "androidx.media3:media3-exoplayer", version.ref = "media3Exoplayer" }  
androidx-media3-exoplayer-dash = { module = "androidx.media3:media3-exoplayer-dash", version.ref = "media3Exoplayer" }  
androidx-media3-ui = { module = "androidx.media3:media3-ui", version.ref = "media3Exoplayer" }
```


MusicPlayer :
```kotlin
object MusicPlayer {  
  
    private const val UPDATE_DELAY = 500L  
  
  
    private val _currentTrack = MutableStateFlow<Track?>(null)  
    val currentTrack: StateFlow<Track?> = _currentTrack  
  
    private val _progress = MutableStateFlow(0f)  
    val progress: StateFlow<Float> = _progress  
  
    private val _playing = MutableStateFlow(false)  
    val playing: StateFlow<Boolean> = _playing  
  
    private lateinit var exoPlayer: ExoPlayer  
    private val handler = Handler(Looper.myLooper()!!)  
    private val updateProgressRunnable = object : Runnable {  
        override fun run() {  
            _progress.value = exoPlayer.currentPosition.toFloat() / exoPlayer.duration.toFloat()  
            handler.postDelayed(this, UPDATE_DELAY)  
            Log.e("progress", "${_progress.value * 100}%")  
        }  
    }  
  
  
    private val queue = ArrayList<Track>()  
    private var pointer = -1  
  
    fun initialize(context: Context) {  
        SongRepository.getSongs().forEach { queue.add(it) }  
        exoPlayer = ExoPlayer.Builder(context).build()  
        exoPlayer.addListener(object : Player.Listener {  
            override fun onPlaybackStateChanged(playbackState: Int) {  
                super.onPlaybackStateChanged(playbackState)  
                if (playbackState == Player.STATE_ENDED) {  
                    next()  
                }  
            }  
  
            override fun onIsPlayingChanged(isPlaying: Boolean) {  
                super.onIsPlayingChanged(isPlaying)  
                Handler(Looper.myLooper()!!).postDelayed({  
                    if (exoPlayer.isPlaying == isPlaying) _playing.value = isPlaying // I don't believe it fixed  
                }, 100)  
            }  
        })  
    }  
  
    fun play(track: Track) {  
        _currentTrack.value = track  
        exoPlayer.setMediaItem(MediaItem.fromUri(track.uri))  
        exoPlayer.prepare()  
        exoPlayer.play()  
        handler.post(updateProgressRunnable)  
        _progress.value = 0f  
        pointer = queue.indexOf(track)  
    }  
  
    fun resume() {  
        exoPlayer.play()  
        handler.post(updateProgressRunnable)  
    }  
  
    fun pause() {  
        exoPlayer.pause()  
        handler.removeCallbacks(updateProgressRunnable)  
    }  
  
    fun next() {  
        if (pointer == queue.size - 1)  
            pointer = 0  
        else  
            pointer++  
        play(queue[pointer])  
    }  
  
    fun prev() {  
        if (pointer == 0)  
            pointer = queue.size - 1  
        else  
            pointer--  
        play(queue[pointer])  
    }  
  
    fun progressChange(progress: Float) {  
        exoPlayer.seekTo((progress * exoPlayer.duration).toLong())  
    }  
  
  
}
```


load songs :
```kotlin
class SongLoader(private val context: Context) {  
  
    fun loadSongs(): List<Track> {  
        val trackList = mutableListOf<Track>()  
        val contentResolver = context.contentResolver  
  
        val projection = arrayOf(  
            MediaStore.Audio.Media._ID,  
            MediaStore.Audio.Media.TITLE,  
            MediaStore.Audio.Media.ARTIST,  
            MediaStore.Audio.Media.DATA,  
            MediaStore.Audio.Media.ALBUM_ID,  
            MediaStore.Audio.Media.DURATION  
        )  
  
        val cursor = contentResolver.query(MediaStore.Audio.Media.EXTERNAL_CONTENT_URI, projection, null, null, null)  
  
        cursor?.use {  
            val idColumn = it.getColumnIndexOrThrow(MediaStore.Audio.Media._ID)  
            val titleColumn = it.getColumnIndexOrThrow(MediaStore.Audio.Media.TITLE)  
            val artistColumn = it.getColumnIndexOrThrow(MediaStore.Audio.Media.ARTIST)  
            val uriColumn = it.getColumnIndexOrThrow(MediaStore.Audio.Media.DATA)  
            val albumIdColumn = it.getColumnIndexOrThrow(MediaStore.Audio.Media.ALBUM_ID)  
            val durationColumn = it.getColumnIndexOrThrow(MediaStore.Audio.Media.DURATION)  
  
            while (it.moveToNext()) {  
                val id = it.getString(idColumn)  
                val title = it.getString(titleColumn)  
                val artist = it.getString(artistColumn)  
                val uri = it.getString(uriColumn)  
                val albumId = it.getLong(albumIdColumn)  
                val duration = it.getLong(durationColumn)  
  
                val albumArtUri = getAlbumArtUri(albumId)  
  
                trackList.add(Track(id, title, artist, uri, albumArtUri, duration))  
            }  
        }  
  
        return trackList  
    }  
  
    private fun getAlbumArtUri(albumId: Long): String? {  
        val albumUri = Uri.parse("content://media/external/audio/albumart")  
        val uri = Uri.withAppendedPath(albumUri, albumId.toString())  
  
        return if (File(uri.path ?: "").exists()) uri.toString() else null  
    }  
  
}
```