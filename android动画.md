# android动画

Android 平台提供了两类动画。 一类是Tween动画，就是对场景里的对象不断的进行图像变化来产生动画效果（旋转、平移、放缩和渐变）。
第二类就是 Frame动画，即顺序的播放事先做好的图像，与gif图片原理类似。

## Tweene Animations。
 
主要类：
 
+ Animation  			动画

+ AlphaAnimation		渐变透明度

+ RotateAnimation		画面旋转

+ ScaleAnimation			渐变尺寸缩放

+ TranslateAnimation		位置移动

+ AnimationSet 			动画集


如何让多个动画同时生效呢？

答案是 AnimationSet
