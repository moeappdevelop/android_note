一.代码
```java
  //设置没有动画
  intent.addFlags(Intent.FLAG_ACTIVITY_NO_ANIMATION);
```
二.anim动画
```xml
表二
XML节点	功能说明
alpha	渐变透明度动画效果
<alpha
android:fromAlpha=”0.1″
android:toAlpha=”1.0″
android:duration=”3000″ />
fromAlpha	
属性为动画起始时透明度
0.0表示完全透明
1.0表示完全不透明
以上值取0.0-1.0之间的float数据类型的数字
toAlpha	属性为动画结束时透明度
表三
scale	渐变尺寸伸缩动画效果
<scale
android:interpolator= “@android:anim/accelerate_decelerate_interpolator”
android:fromXScale=”0.0″
android:toXScale=”1.4″
android:fromYScale=”0.0″
android:toYScale=”1.4″
android:pivotX=”50%”
android:pivotY=”50%”
android:fillAfter=”false”
android:startOffset=“700”
android:duration=”700″
android:repeatCount=”10″ />
fromXScale[float] fromYScale[float]	为动画起始时，X、Y坐标上的伸缩尺寸	0.0表示收缩到没有
1.0表示正常无伸缩
值小于1.0表示收缩
值大于1.0表示放大
toXScale [float]
toYScale[float]	为动画结束时，X、Y坐标上的伸缩尺寸
pivotX[float]
pivotY[float]	为动画相对于物件的X、Y坐标的开始位置	属性值说明：从0%-100%中取值，50%为物件的X或Y方向坐标上的中点位置
 	 	 	 
表四
translate	画面转换位置移动动画效果
<translate
android:fromXDelta=”30″
android:toXDelta=”-80″
android:fromYDelta=”30″
android:toYDelta=”300″
android:duration=”2000″ />
fromXDelta
toXDelta	为动画、结束起始时 X坐标上的位置	 
fromYDelta
toYDelta	为动画、结束起始时 Y坐标上的位置	 
 	 	 	 
表五
rotate	画面转移旋转动画效果
<rotate
android:interpolator=”@android:anim/accelerate_decelerate_interpolator”
android:fromDegrees=”0″
android:toDegrees=”+350″
android:pivotX=”50%”
android:pivotY=”50%”
android:duration=”3000″ />
fromDegrees	为动画起始时物件的角度	说明
当角度为负数——表示逆时针旋转
当角度为正数——表示顺时针旋转
(负数from——to正数:顺时针旋转)
(负数from——to负数:逆时针旋转)
(正数from——to正数:顺时针旋转)
(正数from——to负数:逆时针旋转)
toDegrees	属性为动画结束时物件旋转的角度 可以大于360度
pivotX
pivotY	为动画相对于物件的X、Y坐标的开始位	说明：以上两个属性值 从0%-100%中取值
50%为物件的X或Y方向坐标上的中点位置
```

三.styles.xml
