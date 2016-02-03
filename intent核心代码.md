``` java
//向下一个activity传递数据

Intent intent=new Intent();
String data="Hello SecondActivity";
intent.putExtra("extra_data",data);

//下一个activity接收数据
Activity:onCreate函数中
Intent intent=getIntent();
String data=intent.getStringExtra("extra_data");
Log.d("moe",data);

//接受整型
Int i=intent.getIntExtra("extra_data");

//接受（下一个Activity）返回数据
Intent intent=new Intent(firstActivity.this,SecondActivity.class);
startActivityForResult(intent,1);

//接受（下一个Activity）返回数据 ;回调方法或
onBackPress()
onActivityResult(int requestCode,int resultCode,Intent data){
  switch(requestCode){
  case :1
    if(resultCode==RESULT_OK){
      String returndata=data.getStrintExtra("data_return");
    }
  }
}
//返回数据给（上一个Activity）
Intent intent=new Intent();
intent.purExtra("data_return","hello ");
setResult(RESULT_OK,intent);
finish();
```
