
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
