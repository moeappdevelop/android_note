## 一、DownloadManager简单介绍

DownloadManager是系统开放给第三方应用使用的类，包含两个静态内部类DownloadManager.Query和DownloadManager.Request。DownloadManager.Request用来请求一个下载，DownloadManager.Query用来查询下载信息，这两个类的具体功能会在后面穿插介绍。DownloadManager的源码可见DownloadManager@Grepcode。
 
DownloadManager主要提供了下面几个接口：
public long enqueue(Request request)执行下载，返回downloadId，downloadId可用于后面查询下载信息。若网络不满足条件、Sdcard挂载中、超过最大并发数等异常会等待下载，正常则直接下载。
public int remove(long… ids)删除下载，若下载中取消下载。会同时删除下载文件和记录。
public Cursor query(Query query)查询下载信息。
 
public static Long getRecommendedMaxBytesOverMobile(Context context通过移动网络下载的最大字节数
public String getMimeTypeForDownloadedFile(long id)得到下载的mimeType，如何设置后面会进行介绍
 
其它：通过查看代码我们可以发现还有个CursorTranslator私有静态内部类。这个类主要对Query做了一层代理。将DownloadProvider和DownloadManager之间做个映射。将DownloadProvider中的十几种状态对应到了DownloadManager中的五种状态，DownloadProvider中的失败、暂停原因转换为了DownloadManager的原因。

## 二、下载管理示例
下面具体介绍利用DownloadManager进行下载。
1、AndroidManifest中添加权限
Java

<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
1
2
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
网络访问权限是必须的，下载地址为sdcard的话需要添加sdcard写权限。
 
2、调用DownloadManager.Request开始下载
Java

DownloadManager downloadManager = (DownloadManager)getSystemService(DOWNLOAD_SERVICE);
String apkUrl = "http://img.meilishuo.net/css/images/AndroidShare/Meilishuo_3.6.1_10006.apk";
DownloadManager.Request request = new DownloadManager.Request(Uri.parse(apkUrl));
request.setDestinationInExternalPublicDir("Trinea", "MeiLiShuo.apk");
// request.setTitle("MeiLiShuo");
// request.setDescription("MeiLiShuo desc");
// request.setNotificationVisibility(DownloadManager.Request.VISIBILITY_VISIBLE_NOTIFY_COMPLETED);
// request.setAllowedNetworkTypes(DownloadManager.Request.NETWORK_WIFI);
// request.setNotificationVisibility(DownloadManager.Request.VISIBILITY_HIDDEN);
// request.setMimeType("application/cn.trinea.download.file");
long downloadId = downloadManager.enqueue(request);

DownloadManager downloadManager = (DownloadManager)getSystemService(DOWNLOAD_SERVICE);
String apkUrl = "http://img.meilishuo.net/css/images/AndroidShare/Meilishuo_3.6.1_10006.apk";
DownloadManager.Request request = new DownloadManager.Request(Uri.parse(apkUrl));
request.setDestinationInExternalPublicDir("Trinea", "MeiLiShuo.apk");
// request.setTitle("MeiLiShuo");
// request.setDescription("MeiLiShuo desc");
// request.setNotificationVisibility(DownloadManager.Request.VISIBILITY_VISIBLE_NOTIFY_COMPLETED);
// request.setAllowedNetworkTypes(DownloadManager.Request.NETWORK_WIFI);
// request.setNotificationVisibility(DownloadManager.Request.VISIBILITY_HIDDEN);
// request.setMimeType("application/cn.trinea.download.file");
long downloadId = downloadManager.enqueue(request);
上面调用downloadManager的enqueue接口进行下载，返回唯一的downloadId。
 
DownloadManager.Request除了构造函数的Uri必须外，其他设置都为可选设置。下面逐个介绍下：
request.setDestinationInExternalPublicDir(“Trinea”, “MeiLiShuo.apk”);表示设置下载地址为sd卡的Trinea文件夹，文件名为MeiLiShuo.apk。
setDestinationInExternalPublicDir源码
从源码中我们可以看出下载完整目录为Environment.getExternalStoragePublicDirectory(dirType)。不过file是通过file.mkdir()创建的，这样如果上级目录不存在就会新建文件夹异常。所以下载前我们最好自己调用File的mkdirs方法递归创建子目录，如下：
Java

File folder = new File(folderName);
return (folder.exists() && folder.isDirectory()) ? true : folder.mkdirs();
 
File folder = new File(folderName);
return (folder.exists() && folder.isDirectory()) ? true : folder.mkdirs();
否则，会报异常

java.lang.IllegalStateException: Unable to create directory: /storage/sdcard0/Trinea/aa
at android.app.DownloadManager$Request.setDestinationInExternalPublicDir(DownloadManager.java)
 
java.lang.IllegalStateException: Unable to create directory: /storage/sdcard0/Trinea/aa
at android.app.DownloadManager$Request.setDestinationInExternalPublicDir(DownloadManager.java)
其他设置下载路径接口为setDestinationUri，setDestinationInExternalFilesDir，setDestinationToSystemCache。其中setDestinationToSystemCache仅限系统app使用。
 
request.allowScanningByMediaScanner();表示允许MediaScanner扫描到这个文件，默认不允许。
request.setTitle(“MeiLiShuo”);设置下载中通知栏提示的标题
request.setDescription(“MeiLiShuo desc”);设置下载中通知栏提示的介绍
request.setNotificationVisibility(DownloadManager.Request.VISIBILITY_VISIBLE_NOTIFY_COMPLETED);
表示下载进行中和下载完成的通知栏是否显示。默认只显示下载中通知。VISIBILITY_VISIBLE_NOTIFY_COMPLETED表示下载完成后显示通知栏提示。VISIBILITY_HIDDEN表示不显示任何通知栏提示，这个需要在AndroidMainfest中添加权限android.permission.DOWNLOAD_WITHOUT_NOTIFICATION.
 
request.setAllowedNetworkTypes(DownloadManager.Request.NETWORK_WIFI);
表示下载允许的网络类型，默认在任何网络下都允许下载。有NETWORK_MOBILE、NETWORK_WIFI、NETWORK_BLUETOOTH三种及其组合可供选择。如果只允许wifi下载，而当前网络为3g，则下载会等待。
request.setAllowedOverRoaming(boolean allow)移动网络情况下是否允许漫游。
 
request.setMimeType(“application/cn.trinea.download.file”);
设置下载文件的mineType。因为下载管理Ui中点击某个已下载完成文件及下载完成点击通知栏提示都会根据mimeType去打开文件，所以我们可以利用这个属性。比如上面设置了mimeType为application/cn.trinea.download.file，我们可以同时设置某个Activity的intent-filter为application/cn.trinea.download.file，用于响应点击的打开文件。
Java

<intent-filter>
	<action android:name="android.intent.action.VIEW" />

	<category android:name="android.intent.category.DEFAULT" />

	<data android:mimeType="application/cn.trinea.download.file" />
</intent-filter>
<intent-filter>
	<action android:name="android.intent.action.VIEW" />
 
	<category android:name="android.intent.category.DEFAULT" />
 
	<data android:mimeType="application/cn.trinea.download.file" />
</intent-filter>
request.addRequestHeader(String header, String value)
添加请求下载的网络链接的http头，比如User-Agent，gzip压缩等
 
3 下载进度状态监听及查询
下载进度状态监听代码
其中我们会监听Uri.parse(“content://downloads/my_downloads”)。然后查询下载状态和进度，发送handler进行更新，handler中处理就是设置进度条和状态等。
其中DownloadManagerPro.getBytesAndStatus的主要代码如下，可直接引入TrineaAndroidCommon@Github(欢迎star和fork^_^)作为你项目的library(如何拉取代码及添加公共库)：
下载进度状态查询代码
从上面代码可以看出我们主要调用DownloadManager.Query()进行查询。DownloadManager.Query为下载管理对外开放的信息查询类，主要包括以下接口：
setFilterById(long… ids)根据下载id进行过滤
setFilterByStatus(int flags)根据下载状态进行过滤
setOnlyIncludeVisibleInDownloadsUi(boolean value)根据是否在download ui中可见进行过滤。
 
orderBy(String column, int direction)根据列进行排序，不过目前仅支持DownloadManager.COLUMN_LAST_MODIFIED_TIMESTAMP和DownloadManager.COLUMN_TOTAL_SIZE_BYTES排序。
 
4 下载成功监听
下载完成后，下载管理会发出DownloadManager.ACTION_DOWNLOAD_COMPLETE这个广播，并传递downloadId作为参数。通过接受广播我们可以打开对下载完成的内容进行操作。代码如下：
下载成功监听
 
5、响应通知栏点击
(1) 响应下载中通知栏点击
点击下载中通知栏提示，系统会对下载的应用单独发送Action为DownloadManager.ACTION_NOTIFICATION_CLICKED广播。intent.getData为content://downloads/all_downloads/29669，最后一位为downloadId。
如果同时下载多个应用，intent会包含DownloadManager.EXTRA_NOTIFICATION_CLICK_DOWNLOAD_IDS这个key，表示下载的的downloadId数组。这里设计到下载管理通知栏的显示机制，会在下一篇具体介绍。
 
(2) 响应下载完成通知栏点击
下载完后会调用下面代码进行处理，从中我们可以发现系统会调用View action根据mimeType去查询。所以可以利用我们在介绍的DownloadManager.Request的setMimeType函数。
openDownload源码
 
如果界面上过多元素需要更新，且网速较快不断的执行onChange会对页面性能有一定影响。推荐ScheduledExecutorService定期查询，如下:
Java

public static ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(3);
Runnable command = new Runnable() {

		@Override
		public void run() {
			updateView();
		}
	};
scheduledExecutorService.scheduleAtFixedRate(command, 0, 3, TimeUnit.SECONDS);
 
public static ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(3);
Runnable command = new Runnable() {
 
		@Override
		public void run() {
			updateView();
		}
	};
scheduledExecutorService.scheduleAtFixedRate(command, 0, 3, TimeUnit.SECONDS);
表示3秒定时刷新
