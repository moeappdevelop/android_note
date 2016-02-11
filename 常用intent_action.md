#常用intent action
## 网络状态变化
``` java
IntentFilter intentFilter=new IntentFilter();
intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
networkChangeReceiver(newclass (extends BroadcastReceiver),intentFilter);

//@Override
onReceive(Context context,Intent intent)
```
