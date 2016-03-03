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

# 大总结
```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- 
    shape drawable xml文件中定义的一个几何图形，定义在res/drawable/目录下，文件名filename称为访问的资源ID
    在代码中通过R.drawable.filename进行访问，在xml文件中通过@[package:]drawable/filename进行访问。
 -->
 <!-- 
   android:shape=["rectangle" | "oval" | "line" | "ring"]
   shape的形状，默认为矩形，可以设置为矩形（rectangle）、椭圆形(oval)、线性形状(line)、环形(ring)
      下面的属性只有在android:shape="ring时可用：
      android:innerRadius        尺寸，内环的半径。
      android:innerRadiusRatio    浮点型，以环的宽度比率来表示内环的半径，
      例如，如果android:innerRadiusRatio，表示内环半径等于环的宽度除以5，这个值是可以被覆盖的，默认为9.
      android:thickness           尺寸，环的厚度
      android:thicknessRatio      浮点型，以环的宽度比率来表示环的厚度，例如，如果android:thicknessRatio="2"，
      那么环的厚度就等于环的宽度除以2。这个值是可以被android:thickness覆盖的，默认值是3.
      android:useLevel            boolean值，如果当做是LevelListDrawable使用时值为true，否则为false.
  -->
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
     
    <!--
       圆角
       android:radius            整型 半径
        android:topLeftRadius         整型 左上角半径
        android:topRightRadius    整型 右上角半径
        android:bottomLeftRadius   整型 左下角半径
        android:bottomRightRadius  整型 右下角半径
     -->
     <corners  
        android:radius="8dp"
        android:topLeftRadius="5dp"
        android:topRightRadius="15dp"
        android:bottomLeftRadius="20dp"
        android:bottomRightRadius="25dp"  
        />
      
     <!--
       渐变色
       android:startColor  颜色值                          起始颜色
        android:endColor    颜色值                            结束颜色
        android:centerColor 整型                              渐变中间颜色，即开始颜色与结束颜色之间的颜色
        android:angle       整型                            渐变角度(PS：当angle=0时，渐变色是从左向右。 然后逆时针方向转，当angle=90时为从下往上。angle必须为45的整数倍)
        android:type        ["linear" | "radial" | "sweep"] 渐变类型(取值：linear、radial、sweep)
                            linear 线性渐变，这是默认设置
                            radial 放射性渐变，以开始色为中心。
                            sweep 扫描线式的渐变。
       android:useLevel       ["true" | "false"]               如果要使用LevelListDrawable对象，就要设置为true。设置为true无渐变。false有渐变色
       android:gradientRadius 整型                           渐变色半径.当 android:type="radial" 时才使用。单独使用 android:type="radial"会报错。
       android:centerX       整型                               渐变中心X点坐标的相对位置
       android:centerY    整型                               渐变中心Y点坐标的相对位置
    -->
    <gradient
        android:startColor="#FFFF0000"
        android:endColor="#80FF00FF"
        android:angle="45"
        /> 
         
    <!--
       内边距，即内容与边的距离 
       android:left      整型 左内边距
        android:top      整型 上内边距
        android:right     整型 右内边距
        android:bottom     整型 下内边距
      -->
     <padding 
         android:left="10dp"
         android:top="10dp"
         android:right="10dp"
         android:bottom="10dp"
         />
      
    <!-- 
        size 大小
        android:width  整型 宽度
        android:height     整型 高度
    -->
    <size
        android:width="600dp"
        />
     
    <!--
        内部填充
        android:color  颜色值 填充颜色
    -->
    <solid 
        android:color="#ffff9d77"
        />
     
     <!--
       描边
       android:width      整型     描边的宽度
        android:color      颜色值    描边的颜色
        android:dashWidth  整型     表示描边的样式是虚线的宽度， 值为0时，表示为实线。值大于0则为虚线。
        android:dashGap   整型     表示描边为虚线时，虚线之间的间隔 即“ - - - - ”
     -->
     <stroke 
        android:width="2dp"
        android:color="#dcdcdc"  
        /> 
</shape>
```
