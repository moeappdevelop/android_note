## question

Android “Only the original thread that created a view hierarchy can touch its views.”

## solution

```java
runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            dbloadingInfo.setVisibility(View.VISIBLE);
                            bar.setVisibility(View.INVISIBLE);
                            loadingText.setVisibility(View.INVISIBLE);
                        }
                    });


```
