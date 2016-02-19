#Android 编程下 shape 绘制图形

## 1. 使用 shape 绘制线条
``` xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="line" >

    <!-- 显示一条虚线，破折线的宽度为 dashWith，破折线之间的空隙的宽度为 dashGap，当 dashGap=0dp 时，为实线 -->
    <stroke
        android:dashGap="3dp"
        android:dashWidth="2dp"
        android:width="1dp"
        android:color="#777777" />

    <!-- 虚线的高度 -->
    <size android:height="2dp" />

</shape>
``` 
## 2. 使用 shape 绘制圆形
``` xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
   android:shape="oval" >

   <!-- 填充颜色 -->
   <solid android:color="#F0F0F0" ></solid>

   <!--线的宽度，颜色灰色-->
   <stroke android:width="2dp" android:color="#777777"></stroke>

   <!-- 矩形的圆角半径 -->
   <corners android:radius="5dp" />

</shape>
```
## 3. 使用 shape 绘制矩形

``` xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
   android:shape="rectangle" >

   <!-- 填充颜色 -->
   <solid android:color="#F0F0F0" ></solid>

   <!-- 显示一条实线，线的宽度为 width，颜色为 color -->
   <!-- <stroke android:width="2dp" android:color="#E3E0D5"></stroke> -->

   <!-- 显示一条虚线，破折线的宽度为 dashWith，破折线之间的空隙的宽度为 dashGap，当 dashGap=0dp 时，为实线 -->
   <stroke
       android:dashGap="2dp"
       android:dashWidth="5dp"
       android:width="2dp"
       android:color="#777777" />

   <!-- 虚线的高度 -->
   <size android:height="10dp" />

   <!-- 矩形的圆角半径 -->
   <corners android:radius="0dp" />

</shape>
```
## 4. 使用 shape 绘制半圆角矩形
``` xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle" >

    <!-- topLeftRadius、topRightRadius 为半圆角矩形上半部分的圆角半径，bottomLeftRadius、bottomRightRadius 为矩形下半部分的圆角半径，值为0表示直角 -->
    <corners
        android:bottomLeftRadius="0dp"
        android:bottomRightRadius="0dp"
        android:topLeftRadius="5dp"
        android:topRightRadius="5dp" />

    <gradient
        android:angle="270"
        android:endColor="#d3d3d3"
        android:startColor="#d3d3d3" />

    <stroke
        android:width="0.5dp"
        android:color="#d9d9d9" />

</shape>
```
