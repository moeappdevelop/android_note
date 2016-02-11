
## 活动管理类ActivityCollector

``` java


public class ActivityCollector{
  public static List<Activity> activitys=new ArrayList<Activity>();
  
  public static void addActivity(Activity activity){
    activitys.add(activity);
  }
  public static void removeActivity(Activity activity){
    activitys.remove(activity);
  }
  
  public static void finishAll(){
    for(Activity activity :activitys){
      if(!activity.isFinishing()){
        activity.finish();
      }
    }
  }
}
```
## 基类

``` java
public class MoeActivity extends Activity {
    @Override
    protected void onCreate(Bundle saveInstanceState){
        super.onCreate(saveInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        ActivityCollector.addActivity(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        ActivityCollector.removeActivity(this);
    }
}
```
