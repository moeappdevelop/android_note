##  ContentProvider 内容提供器
  跨程序 数据共享
  
### 访问其它程序中的数据
``` java
  ContentResolver contentresolver=Context.getContextResolver();
  Uri uri=Uri.parse("content://com.example.app.provider/table1");
  
  
  1 查询操作
  Cursor cursor=getContentResolver().query(uri,projection,selection,selectionArgs,sortOrder);
  
  if(sursor!=null){
    while(sursor.moveToNext(){
      String column1=cursor.getString(sursor.getColumnIndex("column1"));
      int column2=cursor.getInt(cursor.getColumnIndex("column2"));
    })
    cursor.close();
  }
  
  2 增删改
  ContentValues values=new ContentValues();
  getContentResolver().insert(uri,values);
  
  getContentResolver().delete(uri,"column2 = ?",new String[]("1"));
```
### 创建内容提供器

``` java
public class Myprovider extends ContentProvider{
  @Override
  public boolean onCreate(){
    return false;
  }
  
  @
}
```
