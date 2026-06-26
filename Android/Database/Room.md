

module gradle
```kotlin
plugins {  
    alias(libs.plugins.android.application)  
    alias(libs.plugins.kotlin.android)  
    alias(libs.plugins.kotlin.compose)  
    id("kotlin-kapt")  
  
}
```


```kotlin
implementation("androidx.room:room-runtime:2.6.1")  
kapt("androidx.room:room-compiler:2.6.1")  
implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.8.7")  
implementation("androidx.room:room-ktx:2.6.1")
```

---

Dao :
```kotlin
@Dao  
interface NotiDao {  
  
    @Insert  
    suspend fun insertItems(items: List<NotiEntity>)  
  
    @Insert  
    suspend fun insertItem(noti: NotiEntity)  
  
  
    @Query("SELECT * from notifications")  
    fun getItems(): Flow<List<NotiEntity>>  
  
  
    @Query("SELECT * from notifications where packageName = :packageName")  
    fun getItemsByPackageName(packageName: String): Flow<List<NotiEntity>>  
  
  
    @Query("UPDATE notifications SET seen = 1 where notificationId = :id")  
    suspend fun updateToSeen(id: Int)  
  
  
    @Query("SELECT * from notifications where notificationId = :id")  
    fun getItemById(id: Int): Flow<NotiEntity?>  
}
```

Database :
```kotlin
@Database(entities = [NotiEntity::class], version = 1, exportSchema = false)  
abstract class AppDatabase : RoomDatabase() {  
  
    abstract fun notiDao(): NotiDao  
  
    companion object {  
        @Volatile  
        private var INSTANCE: AppDatabase? = null  
  
        fun getDatabase(context: Context): AppDatabase {  
            return INSTANCE ?: synchronized(this) {  
                val instance = Room.databaseBuilder(  
                    context.applicationContext,  
                    AppDatabase::class.java,  
                    "app_database"  
                ).build()  
                INSTANCE = instance  
                instance  
            }  
        }  
    }  
  
}
```


Entity :
```kotlin
@Entity(tableName = "notifications")  
data class NotiEntity(  
    @PrimaryKey(autoGenerate = true) val notificationId: Int = 0,  
    val packageName: String,  
    val title: String?,  
    val text: String?,  
    val subText: String?,  
    val bigText: String?,  
    val postTime: Long,  
    val isOngoing: Boolean,  
    val extraMessages: String?, // like telegram notification chat  
    val seen: Boolean = false,  
)
```

---

Usage in ViewModel :
```kotlin
val notifications: Flow<List<NotiEntity>> = repository.getItemsByPackageName(packageName)
```