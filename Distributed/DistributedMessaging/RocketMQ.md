# RocketMQ
[TOC]
## 目录

### 一、个人总结
#### 1.安装配置
1. **RocketMQ下载和配置**
   - 下载地址：[rocketMQ下载](https://rocketmq.apache.org/download/)
   - 解压缩
   - **配置系统环境**
    ```java
    ROCKETMQ_HOME="E:\rocketMQ\rocketmq-all-5.1.3-bin-release"
    NAMESRV_ADDR="localhost:9876"
    ```
   - **runserver.cmd参数配置**
   ```java
    set "JAVA_OPT=%JAVA_OPT% -server -Xms512m -Xmx512m -Xmn512m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
   ``` 
   版本不匹配问题解决(直接覆盖)：
   ```java
    @echo off
    rem Licensed to the Apache Software Foundation (ASF) under one or more
    rem contributor license agreements.  See the NOTICE file distributed with
    rem this work for additional information regarding copyright ownership.
    rem The ASF licenses this file to You under the Apache License, Version 2.0
    rem (the "License"); you may not use this file except in compliance with
    rem the License.  You may obtain a copy of the License at
    rem
    rem     http://www.apache.org/licenses/LICENSE-2.0
    rem
    rem Unless required by applicable law or agreed to in writing, software
    rem distributed under the License is distributed on an "AS IS" BASIS,
    rem WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    rem See the License for the specific language governing permissions and
    rem limitations under the License.


    if not exist "%JAVA_HOME%\bin\java.exe" echo Please set the JAVA_HOME variable in your environment, We need java(x64)! & EXIT /B 1
    set "JAVA=%JAVA_HOME%\bin\java.exe"

    setlocal

    set BASE_DIR=%~dp0
    set BASE_DIR=%BASE_DIR:~0,-1%
    for %%d in (%BASE_DIR%) do set BASE_DIR=%%~dpd

    set CLASSPATH=.;%BASE_DIR%conf;%BASE_DIR%lib\*;%CLASSPATH%

    set "JAVA_OPT=%JAVA_OPT% -server -Xms4g -Xmx4g -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
    set "JAVA_OPT=%JAVA_OPT% -XX:+UseG1GC -XX:G1HeapRegionSize=16m -XX:G1ReservePercent=25 -XX:InitiatingHeapOccupancyPercent=30 -XX:SoftRefLRUPolicyMSPerMB=0"
    set "JAVA_OPT=%JAVA_OPT% -verbose:gc -Xloggc:"%USERPROFILE%\rmq_srv_gc.log" -XX:+PrintGCDetails"
    set "JAVA_OPT=%JAVA_OPT% -XX:-OmitStackTraceInFastThrow"
    set "JAVA_OPT=%JAVA_OPT% -XX:-UseLargePages"
    set "JAVA_OPT=%JAVA_OPT% %JAVA_OPT_EXT% -cp "%CLASSPATH%""

    echo %*

    "%JAVA%" %JAVA_OPT% %*

   ```
   - **runbroker.cmd参数配置**
    ```java
    set "JAVA_OPT=%JAVA_OPT% ‐server ‐Drocketmq.broker.diskSpaceWarningLevelRatio=0.98 ‐Xms512m ‐Xmx512m ‐Xmn512m"
    ```
    %CLASSPATH%要加个双引号，不然启动时会找不到jdk
    版本不匹配解决(直接覆盖)
    ```java
    @echo off
    rem Licensed to the Apache Software Foundation (ASF) under one or more
    rem contributor license agreements.  See the NOTICE file distributed with
    rem this work for additional information regarding copyright ownership.
    rem The ASF licenses this file to You under the Apache License, Version 2.0
    rem (the "License"); you may not use this file except in compliance with
    rem the License.  You may obtain a copy of the License at
    rem
    rem     http://www.apache.org/licenses/LICENSE-2.0
    rem
    rem Unless required by applicable law or agreed to in writing, software
    rem distributed under the License is distributed on an "AS IS" BASIS,
    rem WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    rem See the License for the specific language governing permissions and
    rem limitations under the License.

    if not exist "%JAVA_HOME%\bin\java.exe" echo Please set the JAVA_HOME variable in your environment, We need java(x64)! & EXIT /B 1
    set "JAVA=%JAVA_HOME%\bin\java.exe"

    setlocal

    set BASE_DIR=%~dp0
    set BASE_DIR=%BASE_DIR:~0,-1%
    for %%d in (%BASE_DIR%) do set BASE_DIR=%%~dpd

    set CLASSPATH=.;%BASE_DIR%conf;%BASE_DIR%lib\*;%CLASSPATH%

    rem ===========================================================================================
    rem  JVM Configuration
    rem ===========================================================================================
    set "JAVA_OPT=%JAVA_OPT% -server -Xms2g -Xmx2g"
    set "JAVA_OPT=%JAVA_OPT% -XX:+UseG1GC -XX:G1HeapRegionSize=16m -XX:G1ReservePercent=25 -XX:InitiatingHeapOccupancyPercent=30 -XX:SoftRefLRUPolicyMSPerMB=0 -XX:SurvivorRatio=8"
    set "JAVA_OPT=%JAVA_OPT% -verbose:gc -Xlog:gc*:file=%USERPROFILE%/mq_gc.log:time,tags:filecount=5,filesize=30M"
    set "JAVA_OPT=%JAVA_OPT% -XX:-OmitStackTraceInFastThrow"
    set "JAVA_OPT=%JAVA_OPT% -XX:+AlwaysPreTouch"
    set "JAVA_OPT=%JAVA_OPT% -XX:MaxDirectMemorySize=15g"
    set "JAVA_OPT=%JAVA_OPT% -XX:-UseLargePages -XX:-UseBiasedLocking"
    set "JAVA_OPT=%JAVA_OPT% -Drocketmq.client.logUseSlf4j=true"
    set "JAVA_OPT=%JAVA_OPT% %JAVA_OPT_EXT% -cp %CLASSPATH%"

    "%JAVA%" %JAVA_OPT% %*

    ```
   - **启动**
   在bin目录下执行`mqnamesrv.cmd`
   在bin目录下执行`mqbroker.cmd autoCreateTopicEnable=true`
   或者`mqbroker.cmd -n localhost:9876 autoCreateTopicEnable=true`
   > **-n localhost:9876**: 这指定了要连接的 NameServer 的地址和端口。在此例中，它连接到处于本地主机的 NameServer，并使用默认的 9876 端口。
    **autoCreateToopicEnable=true**: 这是 Broker 的一个配置参数，用于控制是否允许自动创建 Topic。当设置为 true 时，如果生产者发送消息到一个尚不存在的 Topic，Broker 将会自动创建这个 Topic。这个参数的设置可以方便地在没有显示创建 Topic 的情况下，通过发送消息自动创建所需的 Topic。
   - **测试**
    
    发送消息：
   ```java
    set NAMESRV_ADDR=127.0.0.1:9876
    cd G:\rocketmq\bin
    tools.cmd org.apache.rocketmq.example.quickstart.Producer
   ``` 
    接收消息：
    ```java
    set NAMESRV_ADDR=127.0.0.1:9876
    tools.cmd org.apache.rocketmq.example.quickstart.Consumer
    ```
2. **可视化工具RocketMQ DashBoard下载**
- 下载地址：https://dist.apache.org/repos/dist/release/rocketmq/rocketmq-dashboard/1.0.0/rocketmq-dashboard-1.0.0-source-release.zip
- application.properties配置文件`rocketmq.config.namesrvAddr=localhost:9876`
- 配置文件修改之后，就可以执行`mvn package`打包命令，生成对应的jar包程序。打包成功之后，就可以在`target`目录下面找到生成的jar包，这个jar就是可执行的rocketmq-dashboard程序。
### 二、八股问题整理