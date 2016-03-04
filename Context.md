这里大致可以分为两种：一是传递Context参数，二是调用全局的Context.

其实我们应用启动的时候会启动Application这个类，这个类是在AndroidManifest.xml文件里其实是默认的

```
<application  
       android:icon="@drawable/ic_launcher"  
       android:label="@string/app_name"  
       >  
       <activity  
           android:name=".ApplicationDemoActivity"  
           android:label="@string/app_name" >  
           <intent-filter>  
               <action android:name="android.intent.action.MAIN" />  
               <category android:name="android.intent.category.LAUNCHER" />  
           </intent-filter>  
       </activity>  
   </application>  
```
这个Application类是单例的,也就是说我们可以自己写个Application(比如名为：MainApplication)类，来代替默认的Applicaiton,这个类可以保存应用的全局变量，我们可以定义一个全局的Context.供外部调用.用法如下：
```
package com.tutor.application;  
  
import android.app.Application;  
import android.content.Context;  
  
public class MainApplication extends Application {  
  
    /** 
     * 全局的上下文. 
     */  
    private static Context mContext;  
      
    @Override  
    public void onCreate() {  
        super.onCreate();  
          
        mContext = getApplicationContext();  
          
    }     
      
    /**获取Context. 
     * @return 
     */  
    public static Context getContext(){  
        return mContext;  
    }  
      
      
    @Override  
    public void onLowMemory() {  
        super.onLowMemory();  
    }  
      
      
}  
```
我们需要在AndroidMainifest.xml把MainApplication注册进去(第10行代码)：
```
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
    package="com.tutor.application"  
    android:versionCode="1"  
    android:versionName="1.0" >  
      
    <application  
        android:icon="@drawable/ic_launcher"  
        android:label="@string/app_name"  
        android:name=".MainApplication" >  
        <activity  
            android:name=".ApplicationDemoActivity"  
            android:label="@string/app_name" >  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN" />  
                <category android:name="android.intent.category.LAUNCHER" />  
            </intent-filter>  
        </activity>  
    </application>  
      
</manifest>  
```
为了让大家更容易理解，写了一个简单的Demo.步骤如下：

第一步：新建一个Android工程ApplicationDemo,目录结构如下：



第二步：新建MainApplication.java，代码和上面一样我就不贴了.

第三步：新建一个工具类ToolsUtil.java，代码如下

```
package com.tutor.application;  
  
import android.content.Context;  
import android.widget.Toast;  
  
/** 
 * @author frankiewei. 
 * 应用的一些工具类. 
 */  
public class ToolUtils {  
      
    /** 
     * 参数带Context. 
     * @param context 
     * @param msg 
     */  
    public static void showToast(Context context,String msg){  
        Toast.makeText(context, msg, Toast.LENGTH_SHORT).show();  
    }  
      
    /** 
     * 调用全局的Context. 
     * @param msg 
     */  
    public static void showToast(String msg){  
        Toast.makeText(MainApplication.getContext(), msg, Toast.LENGTH_SHORT).show();  
    }  
}  
```
