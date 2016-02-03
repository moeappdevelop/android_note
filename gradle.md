## 在线资源

[gradle](http://pkaq.github.io/gradledoc/docs/userguide/userguide.html)
```
timeloveboydeiMac:~ timeloveboy$ export GRADLE_HOME=/Volumes/MacData/gradle/gradle-2.10 
timeloveboydeiMac:~ timeloveboy$ echo $GRADLE_HOME
/Volumes/MacData/gradle/gradle-2.10
timeloveboydeiMac:~ timeloveboy$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin
timeloveboydeiMac:~ timeloveboy$ export PATH=$GRADLE_HOME/bin:$PATH
timeloveboydeiMac:~ timeloveboy$ echo $PATH
/Volumes/MacData/gradle/gradle-2.10/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin
timeloveboydeiMac:~ timeloveboy$ gradle
:help

Welcome to Gradle 2.10.

To run a build, run gradle <task> ...

To see a list of available tasks, run gradle tasks

To see a list of command-line options, run gradle --help

To see more detail about a task, run gradle help --task <task>

BUILD SUCCESSFUL

Total time: 3.003 secs

This build could be faster, please consider using the Gradle Daemon: https://docs.gradle.org/2.10/userguide/gradle_daemon.html

timeloveboydeiMac:~ timeloveboy$ gradle -v

------------------------------------------------------------
Gradle 2.10
------------------------------------------------------------

Build time:   2015-12-21 21:15:04 UTC
Build number: none
Revision:     276bdcded730f53aa8c11b479986aafa58e124a6

Groovy:       2.4.4
Ant:          Apache Ant(TM) version 1.9.3 compiled on December 23 2013
JVM:          1.7.0_79 (Oracle Corporation 24.79-b02)
OS:           Mac OS X 10.11.3 x86_64
```
