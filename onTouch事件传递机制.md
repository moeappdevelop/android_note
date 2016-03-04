## onTouch事件传递机制

Android的触摸事件：onClick, onScroll, onFling等等，都是由许多个Touch组成的。

其中Touch的第一个状态肯定是ACTION_DOWN, 表示按下了屏幕。之后，touch将会有后续事件，可能是：
ACTION_MOVE  //表示为移动手势
ACTION_UP  //表示为离开屏幕
ACTION_CANCEL  //表示取消手势，不会由用户产生，而是由程序产生的

一个Action_DOWN, n个ACTION_MOVE, 1个ACTION_UP，就构成了Android中众多的事件。
## 嵌套控件
在Android中，有一类控件是中还可以包含其他的子控件，这类控件是继承于ViewGroup类，例如：ListView, Gallery, GridView，LinearLayout。
还有一类控件是不能再包含子控件，例如：TextView。
在触发OnTouch事件的时候Android的GroupView会调用如下三个函数：
public boolean dispatchTouchEvent(MotionEvent ev)      //用于事件的分发
public boolean onInterceptTouchEvent(MotionEvent ev)    //  用于事件的拦截
public boolean onTouchEvent(MotionEvent ev)     //处理事件

本文的主要讨论对象就是ViewGroup类的控件嵌套时事件触发情况。
### 对于ViewGroup类的控件，有一个很重要的方法，就是onInterceptTouchEvent()

用于处理事件并改变事件的传递方向，它的返回值是一个布尔值，决定了Touch事件是否要向它包含的子View继续传递，这个方法是从父View向子View传递。

###而方法onTouchEvent()

用于接收事件并处理，它的返回值也是一个布尔值，决定了事件及后续事件是否继续向上传递，这个方法是从子View向父View传递。


touch事件在onInterceptTouchEvent()和onTouchEvent以及各个childView间的传递机制完全取决于onInterceptTouchEvent()和onTouchEvent()的返回值。返回值为true表示事件被正确接收和处理了，返回值为false表示事件没有被处理，将继续传递下去。

ACTION_DOWN事件会传到某个ViewGroup类的onInterceptTouchEvent，如果返回false，则DOWN事件继续向子ViewGroup类的onInterceptTouchEvent传递，如果子View不是ViewGroup类的控件，则传递给它的onTouchEvent。

如果onInterceptTouchEvent返回了true,则DOWN事件传递给它的onTouchEvent，不再继续传递，并且之后的后续事件也都传递给它的onTouchEvent。

如果某View的onTouchEvent返回了false，则DOWN事件继续向其父ViewGroup类的onTouchEvent传递；如果返回了true，则后续事件会直接传递给其onTouchEvent继续处理。（后续事件只会传递给对于必要事件ACTION_DOWN返回了true的onTouchEvent。

onInterceptTouchEvent()用于处理事件并改变事件的传递方向。处理事件这个不用说了，你在函数内部编写代码处理就可以了。而决定传递方向的是返回值，返回为false时事件会传递给子控件的onInterceptTouchEvent()；

返回值为true时事件会传递给当前控件的onTouchEvent()，而不在传递给子控件，这就是所谓的Intercept(截断)。

onTouchEvent() 用于处理事件，返回值决定当前控件是否消费（consume）了这个事件。
可能你要问是否消费了又区别吗，反正我已经针对事件编写了处理代码？答案是有区别！比如ACTION_MOVE或者ACTION_UP发生的前提是一定曾经发生了ACTION_DOWN，如果你没有消费ACTION_DOWN，那么系统会认为ACTION_DOWN没有发生过，所以ACTION_MOVE或者ACTION_UP就不能被捕获。

在没有重写onInterceptTouchEvent()和onTouchEvent()的情况下(他们的返回值都是false)。
 
onTouch事件传递测试：
```
<?xml version="1.0" encoding="utf-8"?>  
<com.rpset.test.MyLinearLayout1 xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="fill_parent"  
    android:layout_height="fill_parent"  
    android:orientation="vertical" >  
    <com.rpset.test.MyLinearLayout2  
        android:layout_width="fill_parent"  
        android:layout_height="fill_parent"  
        android:gravity="center"  
        android:orientation="vertical" >  
        <com.rpset.test.MyTextView  
            android:id="@+id/tv"  
            android:layout_width="200px"  
            android:layout_height="200px"  
            android:background="#FFFFFF"  
            android:text="MyTextView"  
            android:textColor="#0000FF"  
            android:textSize="40sp"  
            android:textStyle="bold" />  
    </com.rpset.test.MyLinearLayout2>  
</com.rpset.test.MyLinearLayout1>  
```
注: 当点击MyTextView时，程序会先进入到LinearLayout1的dispatchTouchEvent中，这个类必须调用super.dispatchTouchEvent(ev);否则后面的两个方法无法触发，所以发现这个方法根本没有必要重写，因为框架是在 super.dispatchTouchEvent(ev)中来调用onInterceptTouchEvent和onTouchEvent方法的，所以手动的设置dispatchTouchEvent的返回值是无效的，除非你不想让框架触发这两个方法。
 
对于MyTextView进行测试:
测试一:当三个view的dispatchTouchEvent，onInterceptTouchEvent(MyTextView没有此方法)，onTouchEvent均返回false，也就是说事件始终没有被消费,那后续事件(ACTION_DOWN的ACTION_MOVE或者ACTION_UP)不会触发。Log信息如下:
```
04-09 12:01:55.019: D/MyLinearLayout(5435): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 12:01:55.027: D/MyLinearLayout(5435): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 12:01:55.043: D/MyLinearLayout(5435): MyLinearLayout2——dispatchTouchEvent action:ACTION_DOWN  
04-09 12:01:55.043: D/MyLinearLayout(5435): MyLinearLayout2——onInterceptTouchEvent action:ACTION_DOWN  
04-09 12:01:55.047: D/MyLinearLayout(5435): MyTextView——dispatchTouchEvent action:ACTION_DOWN  
04-09 12:01:55.051: D/MyLinearLayout(5435): MyTextView——-onTouchEvent action:ACTION_DOWN  
04-09 12:01:55.051: D/MyLinearLayout(5435): MyLinearLayout2——-onTouchEvent action:ACTION_DOWN  
04-09 12:01:55.054: D/MyLinearLayout(5435): MyLinearLayout1——-onTouchEvent action:ACTION_DOWN  
```
结论:MyLinearLayout1,MyLinearLayout2,MyTextView都只处理了ACTION_DOWN，其余的TouchEvent被外层的Activity处理了
 
传递示意图:
![](/imgs/onTouch1.png)

 测试二:当只有MyTextView的onTouchEvent返回true,即事件最终在这里消费，(action:ACTION_MOVE会重复出现多次，这里仅代表一下) Log信息如下:
```
04-09 11:58:21.992: D/MyLinearLayout(4621): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 11:58:21.992: D/MyLinearLayout(4621): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 11:58:21.992: D/MyLinearLayout(4621): MyLinearLayout2——dispatchTouchEvent action:ACTION_DOWN  
04-09 11:58:22.000: D/MyLinearLayout(4621): MyLinearLayout2——onInterceptTouchEvent action:ACTION_DOWN  
04-09 11:58:22.000: D/MyLinearLayout(4621): MyTextView——dispatchTouchEvent action:ACTION_DOWN  
04-09 11:58:22.000: D/MyLinearLayout(4621): MyTextView——-onTouchEvent action:ACTION_DOWN  
  
04-09 11:58:22.117: D/MyLinearLayout(4621): MyLinearLayout1——dispatchTouchEvent action:ACTION_MOVE  
04-09 11:58:22.117: D/MyLinearLayout(4621): MyLinearLayout1——onInterceptTouchEvent action:ACTION_MOVE  
04-09 11:58:22.117: D/MyLinearLayout(4621): MyLinearLayout2——dispatchTouchEvent action:ACTION_MOVE  
04-09 11:58:22.117: D/MyLinearLayout(4621): MyLinearLayout2——onInterceptTouchEvent action:ACTION_MOVE  
04-09 11:58:22.133: D/MyLinearLayout(4621): MyTextView——dispatchTouchEvent action:ACTION_MOVE  
04-09 11:58:22.133: D/MyLinearLayout(4621): MyTextView——-onTouchEvent action:ACTION_MOVE  
  
04-09 11:58:22.179: D/MyLinearLayout(4621): MyLinearLayout1——dispatchTouchEvent action:ACTION_UP  
04-09 11:58:22.179: D/MyLinearLayout(4621): MyLinearLayout1——onInterceptTouchEvent action:ACTION_UP  
04-09 11:58:22.179: D/MyLinearLayout(4621): MyLinearLayout2——dispatchTouchEvent action:ACTION_UP  
04-09 11:58:22.179: D/MyLinearLayout(4621): MyLinearLayout2——onInterceptTouchEvent action:ACTION_UP  
04-09 11:58:22.179: D/MyLinearLayout(4621): MyTextView——dispatchTouchEvent action:ACTION_UP  
04-09 11:58:22.179: D/MyLinearLayout(4621): MyTextView——-onTouchEvent action:ACTION_UP
```
结论:MyTextView处理了所有的TouchEvent。
 
传递示意图:
![](/imgs/onTouch2.png)

对MyLinearLayout2进行测试:
测试一:当MyLinearLayout2的onInterceptTouchEvent方法返回true时，发送截断，事件不再向下传递而是直接给当前MyLinearLayout2的onTouchEvent处理，那后续事件(ACTION_DOWN的ACTION_MOVE或者ACTION_UP)不会触发。log信息如下:
```
04-09 12:54:44.398: D/MyLinearLayout(9177): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 12:54:44.398: D/MyLinearLayout(9177): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 12:54:44.398: D/MyLinearLayout(9177): MyLinearLayout2——dispatchTouchEvent action:ACTION_DOWN  
04-09 12:54:44.398: D/MyLinearLayout(9177): MyLinearLayout2——onInterceptTouchEvent action:ACTION_DOWN  
04-09 12:54:44.398: D/MyLinearLayout(9177): MyLinearLayout2——-onTouchEvent action:ACTION_DOWN  
04-09 12:54:44.398: D/MyLinearLayout(9177): MyLinearLayout1——-onTouchEvent action:ACTION_DOWN  
```
结论:MyLinearLayout2,MyLinearLayout1都处理了ACTION_DOWN，其余的由最外层的Activity处理了。
 
传递示意图:
![](/imgs/onTouch3.png)

测试二:当MyLinearLayout2的onTouchEvent方法返回true时，后续的MOVE，Up事件会经过LayoutView1的onInterceptTouchEvent函数，然后到LayoutView2的onTouchEvent函数中去。不再经过MyLinearLayout2的onInterceptTouchEvent函数。 Log信息如下:
```
04-09 18:17:26.410: D/MyLinearLayout(21504): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 18:17:26.410: D/MyLinearLayout(21504): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 18:17:26.414: D/MyLinearLayout(21504): MyLinearLayout2——dispatchTouchEvent action:ACTION_DOWN  
04-09 18:17:26.414: D/MyLinearLayout(21504): MyLinearLayout2——onInterceptTouchEvent action:ACTION_DOWN  
04-09 18:17:26.418: D/MyLinearLayout(21504): MyTextView——dispatchTouchEvent action:ACTION_DOWN  
04-09 18:17:26.418: D/MyLinearLayout(21504): MyTextView——-onTouchEvent action:ACTION_DOWN  
04-09 18:17:26.418: D/MyLinearLayout(21504): MyLinearLayout2——-onTouchEvent action:ACTION_DOWN  
  
04-09 18:17:26.437: D/MyLinearLayout(21504): MyLinearLayout1——dispatchTouchEvent action:ACTION_UP  
04-09 18:17:26.437: D/MyLinearLayout(21504): MyLinearLayout1——onInterceptTouchEvent action:ACTION_UP  
04-09 18:17:26.441: D/MyLinearLayout(21504): MyLinearLayout2——dispatchTouchEvent action:ACTION_UP  
04-09 18:17:26.445: D/MyLinearLayout(21504): MyLinearLayout2——-onTouchEvent action:ACTION_UP  
```
结论:MyTextView只处理了ACTION_DOWN事件，MyLinearLayout2处理了所有的TouchEvent事件。

传递示意图:
![](/imgs/onTouch4.png)

 
测试三:当MyLinearLayout2的onInterceptTouchEvent和onTouchEvent 方法都返回true时，后续的MOVE，Up事件依然会经过LayoutView1的onInterceptTouchEvent函数，然后到LayoutView2的onTouchEvent函数中去。不再经过MyLinearLayout2的onInterceptTouchEvent函数。 Log信息如下:
```
04-09 13:17:30.515: D/MyLinearLayout(9935): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 13:17:30.515: D/MyLinearLayout(9935): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 13:17:30.515: D/MyLinearLayout(9935): MyLinearLayout2——dispatchTouchEvent action:ACTION_DOWN  
04-09 13:17:30.515: D/MyLinearLayout(9935): MyLinearLayout2——onInterceptTouchEvent action:ACTION_DOWN  
04-09 13:17:30.515: D/MyLinearLayout(9935): MyLinearLayout2——-onTouchEvent action:ACTION_DOWN  
  
04-09 13:17:30.547: D/MyLinearLayout(9935): MyLinearLayout1——dispatchTouchEvent action:ACTION_MOVE  
04-09 13:17:30.547: D/MyLinearLayout(9935): MyLinearLayout1——onInterceptTouchEvent action:ACTION_MOVE  
04-09 13:17:30.547: D/MyLinearLayout(9935): MyLinearLayout2——dispatchTouchEvent action:ACTION_MOVE  
04-09 13:17:30.551: D/MyLinearLayout(9935): MyLinearLayout2——-onTouchEvent action:ACTION_MOVE  
  
04-09 13:17:30.644: D/MyLinearLayout(9935): MyLinearLayout1——dispatchTouchEvent action:ACTION_UP  
04-09 13:17:30.644: D/MyLinearLayout(9935): MyLinearLayout1——onInterceptTouchEvent action:ACTION_UP  
04-09 13:17:30.644: D/MyLinearLayout(9935): MyLinearLayout2——dispatchTouchEvent action:ACTION_UP  
04-09 13:17:30.648: D/MyLinearLayout(9935): MyLinearLayout2——-onTouchEvent action:ACTION_UP  
```
结论:MyLinearLayout2处理了所有的TouchEvent。
 传递示意图:
![](/imgs/onTouch5.png)

对于MyLinearLayout1进行测试：
测试一:当MyLinearLayout1的onInterceptTouchEvent方法返回true时，发送截断，事件不再向下传递而是直接给当前MyLinearLayout1的onTouchEvent处理，那后续事件(ACTION_DOWN的ACTION_MOVE或者ACTION_UP)不会触发。log信息如下:
```
04-09 13:52:06.789: D/MyLinearLayout(12363): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 13:52:06.789: D/MyLinearLayout(12363): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 13:52:06.789: D/MyLinearLayout(12363): MyLinearLayout1——-onTouchEvent action:ACTION_DOWN  
```
结论:MyLinearLayout1只处理了ACTION_DOWN，其余的被外层的Avtivity处理了。
传递示意图:
![](/imgs/onTouch6.png)

测试二:当MyLinearLayout1的onTouchEvent 方法返回true时，后续的MOVE，Up事件会直接传给MyLinearLayout1的onTouchEvent()。不再经过MyLinearLayout1的onInterceptTouchEvent函数。Log信息如下:
```
04-09 18:26:58.125: D/MyLinearLayout(22294): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 18:26:58.125: D/MyLinearLayout(22294): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 18:26:58.125: D/MyLinearLayout(22294): MyLinearLayout2——dispatchTouchEvent action:ACTION_DOWN  
04-09 18:26:58.125: D/MyLinearLayout(22294): MyLinearLayout2——onInterceptTouchEvent action:ACTION_DOWN  
04-09 18:26:58.125: D/MyLinearLayout(22294): MyTextView——dispatchTouchEvent action:ACTION_DOWN  
04-09 18:26:58.125: D/MyLinearLayout(22294): MyTextView——-onTouchEvent action:ACTION_DOWN  
04-09 18:26:58.125: D/MyLinearLayout(22294): MyLinearLayout2——-onTouchEvent action:ACTION_DOWN  
04-09 18:26:58.125: D/MyLinearLayout(22294): MyLinearLayout1——-onTouchEvent action:ACTION_DOWN  
  
04-09 18:26:58.215: D/MyLinearLayout(22294): MyLinearLayout1——dispatchTouchEvent action:ACTION_MOVE  
04-09 18:26:58.215: D/MyLinearLayout(22294): MyLinearLayout1——-onTouchEvent action:ACTION_MOVE  
  
04-09 18:26:58.281: D/MyLinearLayout(22294): MyLinearLayout1——dispatchTouchEvent action:ACTION_UP  
04-09 18:26:58.285: D/MyLinearLayout(22294): MyLinearLayout1——-onTouchEvent action:ACTION_UP  
```
结论:MyTextView和MyLinearLayout2只处理了ACTION_DOWN事件，MyLinearLayout1处理了所有的TouchEvent。
传递示意图:
![](/imgs/onTouch7.png)

测试三:当MyLinearLayout1的onInterceptTouchEvent和onTouchEvent 方法都返回true时，后续的MOVE，Up事件会直接传给MyLinearLayout1的onTouchEvent(),不传给其他任何控件的任何函数。不再经过MyLinearLayout1的onInterceptTouchEvent函数。Log信息如下:
```
04-09 13:58:04.199: D/MyLinearLayout(13116): MyLinearLayout1——dispatchTouchEvent action:ACTION_DOWN  
04-09 13:58:04.199: D/MyLinearLayout(13116): MyLinearLayout1——onInterceptTouchEvent action:ACTION_DOWN  
04-09 13:58:04.203: D/MyLinearLayout(13116): MyLinearLayout1——-onTouchEvent action:ACTION_DOWN  
  
04-09 13:58:04.324: D/MyLinearLayout(13116): MyLinearLayout1——dispatchTouchEvent action:ACTION_MOVE  
04-09 13:58:04.328: D/MyLinearLayout(13116): MyLinearLayout1——-onTouchEvent action:ACTION_MOVE  
  
04-09 13:58:04.367: D/MyLinearLayout(13116): MyLinearLayout1——dispatchTouchEvent action:ACTION_UP  
04-09 13:58:04.367: D/MyLinearLayout(13116): MyLinearLayout1——-onTouchEvent action:ACTION_UP  
```
结论:MyLinearLayout1处理了所有的TouchEvent。

 传递示意图:
![](/imgs/onTouch8.png)
