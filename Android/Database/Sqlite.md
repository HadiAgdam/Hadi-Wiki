
```kotlin
private const val SQL_CREATE_ENTRIES =
"CREATE TABLE ${FeedEntry.TABLE_NAME} (" +                "${BaseColumns._ID} INTEGER PRIMARY KEY," +                "${FeedEntry.COLUMN_NAME_TITLE} TEXT," +                "${FeedEntry.COLUMN_NAME_SUBTITLE} TEXT)"  
  
private const val SQL_DELETE_ENTRIES =
"DROP TABLE IF EXISTS ${FeedEntry.TABLE_NAME}"
```

```kotlin

class ContactsDatabaseHelper(private val context: Context) :  
    SQLiteOpenHelper(context, Database.NAME, null, Database.VERSION) {  
  
    private val createTableQuery =  
        "CREATE TABLE ${Database.Contacts.NAME} (" +  
                "${Columns.ID} INTEGER PRIMARY KEY autoincrement," +  
                "${Columns.NAME} TEXT," +  
                "${Columns.PICTURE} TEXT," +  
                "${Columns.TEL} TEXT," +  
                "${Columns.BIO} TEXT)"  
    private val deleteTableQuery =  
        "DROP TABLE IF exists ${Database.Contacts.NAME}"  
  
    override fun onCreate(db: SQLiteDatabase) {  
        db.execSQL(createTableQuery)  
    }  
  
    override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) {  
        db.execSQL(deleteTableQuery)  
        onCreate(db)  
    }

```

### Insert
---
```kotlin
fun insert(contact: Contact): Int {  
    val values = ContentValues().apply {  
        put(Columns.NAME, contact.name.toString())  
        put(Columns.TEL, contact.tel.toString())  
        put(Columns.PICTURE, contact.picture.toString())  
        put(Columns.BIO, contact.bio.toString())  
    }  
  
    return writableDatabase.insert(Database.Contacts.NAME, null, values).toInt()  
  
}
```

### Select
---
```kotlin
val db = dbHelper.readableDatabase  
  
// Define a projection that specifies which columns from the database  
// you will actually use after this query.  
val projection = arrayOf(BaseColumns._ID, FeedEntry.COLUMN_NAME_TITLE, FeedEntry.COLUMN_NAME_SUBTITLE)  
  
// Filter results WHERE "title" = 'My Title'  
val selection = "${FeedEntry.COLUMN_NAME_TITLE} = ?"  
val selectionArgs = arrayOf("My Title")  
  
// How you want the results sorted in the resulting Cursor  
val sortOrder = "${FeedEntry.COLUMN_NAME_SUBTITLE} DESC"  
  
val cursor = db.query(
FeedEntry.TABLE_NAME,
projection,
selection,
selectionArgs,
null,
null,
sortOrder
)

```
