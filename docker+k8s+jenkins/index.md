#### docker + k8s + jenkins 做自动化部署。

docker: 简单理解为简化的虚拟机。比如，在实际中，安装ngxin，需要下载，解压，配置等。使用docker则直接下载可用。 镜像相当于类，容器相当于对象。

k8s: 容器编排技术。当我们的容器过多，对于他的启动，启动多少个，当其中一个出问题了如何处理，就会变得复杂。利用k8s写yam配置文件。自动管理

jenkins: 相当于一个定时任务调度中心。

自动化部署项目实际流程是:
在jenkins配置任务如下 =>

配置git中pull代码，每当有更新时，自动pull  =>

maven打包成jar，打包成镜像  =>

push 镜像到docker的仓库 =>

装了k8s的机器根据yml，管理镜像。就是在仓库push指定的镜像，并且启动 =>

访问