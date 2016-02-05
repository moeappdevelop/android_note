# ui控件xml属性

## 通用熟悉
android:id

长宽

android:layout_width

android:layout_height

长宽值
match_parent(=file_parent)
wrap_content,让当前控件的大小，刚好包住里面的内容

可见度
android:visibility

visible可见
invisible不可见但占据位置
gone无


## TextView 

文本对齐方向

android:gravity

center

## Button

## EditText

提示性文本

android:hint

最大行数

android:maxLines

## ImageView

图片地址

android:src

由于图片高宽未知，所以都设为 wrap_content

```
程序修改图片
imageView.setImageResource(R.drawable.jelly_bean);

```

## ProgressBar

style="?android:attr/progressBarStyleHorizontal"

配合程序代码更改进度
android:max="100"
```
progressBar.getProgress();
progressBar.setProgress();
```

## AlertDialog

