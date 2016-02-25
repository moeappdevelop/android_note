## dp
密度无关像素，也成dip

## px
像素

## sp
和sp一样，主要用于字体

## 获取手机屏幕宽高

``` java
 public void initViews(){
        Display display=getWindowManager().getDefaultDisplay();
        DisplayMetrics dm = new DisplayMetrics();
        display.getRealMetrics(dm);
        int width=dm.widthPixels;
        int height=dm.heightPixels;

        Log.v("moe","width(dp):"+px2dip(MainActivity.this,width));
        Log.v("moe","height(dp):"+px2dip(MainActivity.this,height));
    }
    public static int dip2px(Context context, float dipValue){
        final float scale = context.getResources().getDisplayMetrics().density;
        return (int)(dipValue * scale + 0.5f);
    }
    public static int px2dip(Context context, float pxValue){
        final float scale = context.getResources().getDisplayMetrics().density;
        return (int)(pxValue / scale + 0.5f);
    }

```
