## 广播消息机制

  1. 一.标准Broadcast 
  
    不能截断
  2. 二.有序广播Ordered broadcasts
  
    可以截断

## 注册广播
  1.在AndroidManifest.xml注册
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
