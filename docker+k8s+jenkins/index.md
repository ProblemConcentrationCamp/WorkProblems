#### docker + k8s + jenkins 做自动化部署。

#### docker ####

* 什么是容器？

一种虚拟化技术。一个容器实质就是一个独立的进程。只不过在其中这个进程时，进行了一些特殊处理。让这个进程进入了
一个全新的虚拟环境，与宿主机环境分开。所以这个进程以及其子进程都 认为 自己运行在一个独立的环境中。
所以简单的理解来说，容器 是一个简化版的的虚拟机。

* 什么是docker？

docker 是一个容器引擎。提供了一套完整的容器解决方案。

* 容器的优势

创建容器速度快、删除容器速度快、占用的额外开销小。因为轻、快的特点，容器又叫做“轻量虚拟化”技术。

* 为什么学习docker？

docker 很可能改变传统软件的“交付”和“运行”方式。

* CentOS 下 安装docker

https://www.cnblogs.com/lylsr/p/11173003.html

* docker 镜像

可以将docker镜像简单的理解成一个目录A。在docker server 启动时，复制这个镜像目录成为B。然后在容器进程启动后，
将这个进程chroot到这个目录B。这样目录B就成为了这个容器进程的根文件系统（rootfs）。

docker images 查看本地镜像



#### k8s ####

容器编排技术。当我们的容器过多，对于他的启动，启动多少个，当其中一个出问题了如何处理，就会变得复杂。利用k8s写yam配置文件。自动管理



#### jenkins ####
可配置job，并远程执行的可视化操作。

Jenkins服务器 -> 部署服务器 —> git服务器
前提：部署服務器已安裝java，maven，git环境。
1. 部署服务器连接git服务器需要提前配置ssh。确保部署服务器可以通过git免密直接clone项目
2. Jenkins中添加节点。实际是远程部署服务器的账号密码
3. Jenlins配置Job，指定生效节点，指定项目的git地址等。 
4. enkins配置部署脚本。






自动化部署项目实际流程是:
在jenkins配置任务如下 =>

配置git中pull代码，每当有更新时，自动pull  =>

maven打包成jar，打包成镜像  =>

push 镜像到docker的仓库 =>

装了k8s的机器根据yml，管理镜像。就是在仓库push指定的镜像，并且启动 =>

访问
