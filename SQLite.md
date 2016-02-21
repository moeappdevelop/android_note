## SQLite辅助类
SQLiteOpenHelper 是抽象类

## sqlite 基础
一、存储种类和数据类型：

    SQLite将数据值的存储划分为以下几种存储类型：
     NULL: 表示该值为NULL值。
     INTEGER: 无符号整型值。
     REAL: 浮点值。
     TEXT: 文本字符串，存储使用的编码方式为UTF-8、UTF-16BE、UTF-16LE。
     BLOB: 存储Blob数据，该类型数据和输入数据完全相同。
    由于SQLite采用的是动态数据类型，而其他传统的关系型数据库使用的是静态数据类型，即字段可以存储的数据类型是在表声明时即以确定的，因此它们之间在数据存储方面还是存在着很大的差异。在SQLite中，存储分类和数据类型也有一定的差别，如INTEGER存储类别可以包含6种不同长度的Integer数据类型，然而这些INTEGER数据一旦被读入到内存后，SQLite会将其全部视为占用8个字节无符号整型。因此对于SQLite而言，即使在表声明中明确了字段类型，我们仍然可以在该字段中存储其它类型的数据。然而需要特别说明的是，尽管SQLite为我们提供了这种方便，但是一旦考虑到数据库平台的可移植性问题，我们在实际的开发中还是应该尽可能的保证数据类型的存储和声明的一致性。除非你有极为充分的理由，同时又不再考虑数据库平台的移植问题，在此种情况下确实可以使用SQLite提供的此种特征。
   1. 布尔数据类型：
    SQLite并没有提供专门的布尔存储类型，取而代之的是存储整型1表示true，0表示false。

   2. 日期和时间数据类型：
    和布尔类型一样，SQLite也同样没有提供专门的日期时间存储类型，而是以TEXT、REAL和INTEGER类型分别不同的格式表示该类型，如：
    TEXT: "YYYY-MM-DD HH:MM:SS.SSS"
    REAL: 以Julian日期格式存储
    INTEGER: 以Unix时间形式保存数据值，即从1970-01-01 00:00:00到当前时间所流经的秒数。

## sql 联合查询
|| *Year* || *Temperature (low)* || *Temperature (high)* ||
|| 1900 || -10 || 25 ||
|| 1910 || -15 || 30 ||
|| 1920 || -10 || 32 ||
表1

|id|stuid|stuname|
|1|2340|李鹏|

表2

|id|stuid|projectname|score|
|1|2340|语文|88|
### 内联inner join

### 左联left outer join

### 右联right outer join

### 全联full outer join
