

## Notification with actions

permissions
```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MEDIA_PLAYBACK" />  
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>  
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
```

inside application
```xml
<receiver  
    android:name=".broadcast.MyReceiver"  
    android:enabled="true"  
    android:exported="true"/>  
  
<service  
    android:name=".service.MyService"  
    android:enabled="true"  
    android:exported="true"  
    android:foregroundServiceType="mediaPlayback"/>
```

service
```kotlin
class MyService : Service() {  
  
    private val notificationId = 1  
    private val channelId = "channel"  
    private val scope = CoroutineScope(Dispatchers.Main)  
    private lateinit var notificationManager: NotificationManager  
  
    @RequiresApi(Build.VERSION_CODES.O)  
    private fun createNotificationChannel() {  
        val channel = NotificationChannel(channelId, "Default Channel", NotificationManager.IMPORTANCE_LOW) // IMPORTANT  
        notificationManager = getSystemService(NotificationManager::class.java)  
        notificationManager.createNotificationChannel(channel)  
    }  
  
    private fun buildNotification(content: String): Notification {  
        val remoteViews = RemoteViews(packageName, R.layout.notification_layout)  
  
        val playPauseIntent = Intent(this, MyReceiver::class.java).apply {  
            action = "a1"  
        }  
  
        remoteViews.setOnClickPendingIntent(  
            R.id.button,  
            PendingIntent.getBroadcast(this, 0, playPauseIntent, PendingIntent.FLAG_IMMUTABLE)  
        )  
  
        remoteViews.setTextViewText(R.id.text, content)  
  
  
        return NotificationCompat.Builder(this, channelId)  
            .setSmallIcon(R.drawable.ic_launcher_foreground)  
            .setCustomContentView(remoteViews)  
            .setPriority(NotificationCompat.PRIORITY_LOW) // IMPORTANT  
            .setOngoing(true) // IMPORTANT  
            .build()  
    }  
  
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {  
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {  
            createNotificationChannel()  
        }  
  
        // Start collecting StateFlow  
        scope.launch {  
            Content.content.collect { content ->  
                val notification = buildNotification(content)  
                notificationManager.notify(notificationId, notification)  
            }  
        }  
        val notification = buildNotification("Initial Text")  
        startForeground(notificationId, notification)  
  
        return START_STICKY  
    }  
  
    override fun onDestroy() {  
        scope.cancel()  
        super.onDestroy()  
    }  
  
    override fun onBind(intent: Intent): IBinder? = null  
}
```


broadcast receiver
```kotlin
class MyReceiver : BroadcastReceiver() {  
  
    override fun onReceive(context: Context, intent: Intent) {  
        when(intent.action) {  
            "a1" -> {  
                Toast.makeText(context, "Click", Toast.LENGTH_SHORT).show()}  
        }  
    }  
}
```


---

## Sounds with notification

```kotlin
channel.setSound(RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION), null)  
channel.enableVibration(true)
```


---

## Ask for permission on android >= 33

```kotlin
ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.POST_NOTIFICATIONS), 0)
```