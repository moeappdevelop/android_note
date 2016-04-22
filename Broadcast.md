## 广播消息机制

  1. 一.标准Broadcast 
  
    不能截断
  2. 二.有序广播Ordered broadcasts
  
    可以截断

## 注册广播
  1.在AndroidManifest.xml注册
  ``` xml
  <receiver android:name=".networkchangereceiver">
    <intent-filter>
      <action android:name="com.example.broadcasttest.Mybroadcast"/>
    </intent-filter>
  </receiver>
  
  ```
  2. 在代码中注册
## 创建 广播接收器
``` java
class networkchangereceiver extends BroadcastReveicer{
    @Override
    public void onReceive(Context context,Intent intent){
      ///
    }
}

```
## 发送标准广播
``` java
  Intent intent=new Intent("com.example.broadcasttest.Mybroadcast");
  sendBroadcast(intent);
```
## 发送有序广播
  设置优先级
  ``` xml
  <receiver android:name=".networkchangereceiver">
    <intent-filter
      android:priortity="100">
      <action android:name="com.example.broadcasttest.Mybroadcast"/>
    </intent-filter>
  </receiver>
  
  ```
``` java
  Intent intent=new Intent("com.example.broadcasttest.Mybroadcast");
  sendOrderedBroadcast(intent,null);
```
``` java
class networkchangereceiver extends BroadcastReveicer{
    @Override
    public void onReceive(Context context,Intent intent){
      ///
      abortBroadcast();//截断广播
    }
}
```
## 本地广播
> 相对BroadcastReceiver，它只能用于应用内通信，安全性更好，同时拥有更高的运行效率。也是需要发送应用内广播时的官方推荐。
LocalBroadcastManager,仅支持代码注册
``` java
public class MainActivity extends Activity{
  IntentFilter intentFilter;
  LocalReceiver localReceiver;
  LocalBroadcastManager localBroadcastManager;
  
  @Override
  protected void onCreate(Bundle saveInstanceState){
    super.onCreate(saveInstanceState);
    setContentView(R.layout.activity_main);
    localBroadcastManager = LocalBroadcastManager.getInstance(this);
    
    intentFilter =new IntentFilter();
    intentFilter.addAction("xyz.moechat.broadcasttest.LOCAL_BROADCAST")
    
    localReceiver=new LocalReceiver();
    localBroadcastManager.registarReceiver(localReceiver,intentFilter);
  }
  @Override
  protected void onDestroy(){
    super.onDestory();
    localBroadcastManager.ungistarReceiver(localReceiver);
  }
  class LocalReceiver extends BroadcastReceiver{
    @Override
    public void onReceiver(Context context,Intent intent){
      //
    }
  }
}
```

```java
    //发送广播代码片段
    Intent intent=new Intent("xyz.moechat.broadcasttest.LOCAL_BROADCAST");
    localBroadcastManager.sendBroadcast(intent);
    //
```
