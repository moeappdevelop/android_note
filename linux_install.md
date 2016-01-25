``` shell
1.首先获得一个有帮助的androidstudio资料库
http://www.android-studio.org/
2.下载jdk
打印其值
 echo $JAVA_HOME
 sudo tar -xvf ./jdk-8u40-linux-x64.tar.gz -C /opt/java/
 
 设置环境变量
在~/.bashrc中添加
#set java environment
JAVA_HOME=/opt/java/jdk1.8.0_40/
CLASSPATH=$JAVA_HOME/lib/tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
3.下载android-studio

unzip android-studio-ide-143.2489090-linux.zip
sudo mv android-studio /opt
cd android-studio/bin/
 ./studio.sh
 
 4.再运行./studio.sh
会一直卡在“Fetching Android SDK component information”上
1）进入刚安装的Android Studio目录下的bin目录。找到idea.properties文件，用文本编辑器打开。
2）在idea.properties文件末尾添加一行： disable.android.first.run=true ，然后保存文件。
3）关闭Android Studio后重新启动，便可进入界面。

5.SDK
进入Android studio时有一个“start a new android studio project"选项，点击此选项时什么反应都没
这是因为SDK的路径没有设，
http://www.android-studio.org/
下载 SDK TOOLS FOR ANDROID STUDIO 
我下的是android-sdk_r24-linux.tgz
sudo tar -xvf android-sdk_r24-linux.tgz -C /opt/

6.Platform

下载 platform-tools_r14-linux.zip (这个网站只有这个版本的下载)
unzip platform-tools_r14-linux.zip
sudo mv platform-tools /opt/android-sdk-linux/
~/.bashrc中添加
PATH=/opt/android-sdk-linux/tools:/opt/android-sdk-linux/platform-tools:$PATH
export PATH
到Android studio下Configure --> Project Defaults --> Project Structure里把路径设成/opt/android-sdk-linux
这样就可以新建工程了

```
