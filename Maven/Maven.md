###一. Maven的一些特性

####1.maven的dependencies与dependencyManagement的区别
父pom中，<dependencies>的依赖，子pom都会被动继承。且是依赖的引入。
父pom中，<dependencyManagement> 只是做依赖的声明，并不会做真正的引入。
当子pom中的<dependencies>引入了父pom的<dependencyManagement>中有的依赖，
则会真正的引入依赖。如果子pom中的<dependencies>没有填写版本号，
则会使用父pom的<dependencyManagement>规定的版本号，有则会覆盖。
所以父pom的<dependencyManagement>只是用来管理子pom中依赖的版本号。
至于子类则可以主动选择是否继承，并且使用规定的版本号。

####2.maven的仓库
本地仓库、中央仓库、远程仓库。
maven寻找顺序：本地仓库 =》中央仓库 =》远程仓库。
有时，应用需要用到比较新的依赖，而这些依赖并没有正式发布，还是处于milestone或者是snapshot阶段，
并不能从中央仓库下载。这时需要从远程仓库下载。

####3.maven 有做避免依赖冲突的策略
短路优先:
两个直接引用的jar包，都引用了共同jar包。
A-B-Z
C-Z
这种情况下，短路优先，会引用C引用的Z。

顺序优先：
当路径相同，则考虑在pom声明的顺序。

虽然maven有做避免冲突，为什么还会发生冲突？因为maven避免的是同一个依赖的同一版本冲突，
如果同一个依赖，有两个版本存在，maven就不知道如何进行选择，就会报依赖冲突。

####4.依赖冲突时，可以进行依赖排除。


###二 场景1：
在一些特殊情况下（例如去给客户演示项目，客户那边只有内网），maven不能从远程仓库下载依赖的jar包，也访问不了镜像仓库，
这个时候就需要在无网络的情况下运行项目。

如果项目要迁移到一个不联网的电脑，可直接复制maven本地仓库中的所有依赖包至另一台电脑上maven的本地仓库中，修改下文中的内容即可。

需要做的很简单，但是网上相关的文档相当少，你只需修改maven目录下conf/setting.xml文件中的两个地址便能实现在无网络的情况下运行项目。

第一步：设置maven的本地仓库地址。

第二部：把镜像地址也指向本地的maven仓库地址。

修改好之后的setting.xml文件如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
	
      <!-- 本地仓库地址 -->
	  <localRepository>E:\wokesoftware\maven-repository</localRepository>
      <mirrors>
         <mirror>
                <id>central</id>
                <name>central</name>
                <!-- 将镜像地址设置为本地maven地址 -->
                <url>file://E:\wokesoftware\maven-repository</url>
                <mirrorOf>*</mirrorOf>
            </mirror>
      </mirrors>
</settings>
```

要想在完成以上两步就能运行项目的前提是你已经将项目中需要依赖的jar包都下载到本地的maven仓库中，
并在Eclipse，idea等打开发工具中正确配置了本地maven。
