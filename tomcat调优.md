1. tomcat内存优化
linux修改TOMCAT_HOME/bin/catalina.sh，在前面加入
```
JAVA_OPTS="-XX:PermSize=64M -XX:MaxPermSize=128m -Xms512m -Xmx1024m -Duser.timezone=Asia/Shanghai"
```
windows修改TOMCAT_HOME/bin/catalina.bat，在前面加入  

``` nix
set JAVA_OPTS=-XX:PermSize=64M -XX:MaxPermSize=128m -Xms512m -Xmx1024m
```


最大堆内存是1024m，对于现在的硬件还是偏低，实施时，还是按照机器具体硬件配置优化。
2. tomcat 线程优化
```
<Connector port="80" protocol="HTTP/1.1" maxThreads="600" minSpareThreads="100" maxSpareThreads="500" acceptCount="700"
connectionTimeout="20000" redirectPort="8443" />
maxThreads="600"       ///最大线程数
minSpareThreads="100"///初始化时创建的线程数
maxSpareThreads="500"///一旦创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程。
acceptCount="700"//指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理
```  
3. 压缩
Tomcat有一个通过在server.xml配置文件中设置压缩的选项。压缩可以在connector像如下设置中完成，

```
<Connector port="8080" protocol="HTTP/1.1"
connectionTimeout="20000"
redirectPort="8181" compression="500"
compressableMimeType="text/html,text/xml,text/plain,application/octet-stream" />
```

在前面的配置中，当文件的大小大于等于500bytes时才会压缩。如果当文件达到了大小但是却没有被压缩，那么设置属性compression="on"。否则Tomcat默认设置是“off”。接下来我们将看看如何调优数据库。
