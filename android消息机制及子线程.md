##  线程方法

android不允许 在 子线程中更新ui

``` java

@Override
public void onClick(View v) {
    //模拟子线程耗时操作
    new Thread(new Runnable(){
        @Override
        public void run()
        {
            //逻辑方法
            
            //如果在这里更新ui，会报错：android不允许 在 子线程中更新ui

        }
    )}.start();
}
```

## 匿名线程方法

``` java

@Override
public void onClick(View v) {
    //模拟子线程耗时操作
    new Thread()
    {
        @Override
        public void run()
        {
            try
            {
                Thread.sleep(2000);
            } catch (InterruptedException e)
            {
                e.printStackTrace();
            }
            //模拟登录成功
            if ("zhy".equals(name) && (123+"").equals(password))
            {
                userinfo user = new userinfo();
                user.setName(name);
                user.setPassword(Integer.parseInt(password));
                listener.Success(user);
            } else if ( 123==Integer.parseInt(password))
            {
                listener.PasswordError();
            }
        }
    }.start();
}
```
## 异步消息处理机制
![](/imgs/android_message.png)

## AsyncTask异步任务
