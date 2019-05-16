持续集成发展历程

1. 无构建服务器
2. 夜间构建
3. 夜间构建+自动化测试
4. 加入度量指标
5. 更认真的对待测试
6. 自动化验收测试和自动化部署
7. 持续部署

## Kubernetes

名称 **Kubernetes** 源于希腊语，意为 “舵手” 或 “飞行员”， 且是英文 “governor” 和 [“cybernetic”](http://www.etymonline.com/index.php?term=cybernetics)的词根。 **K8s** 是通过将 8 个字母 “ubernete” 替换为 8 而导出的缩写。另外，在中文里，k8s 的发音与 Kubernetes 的发音比较接近。

[K8S](https://kubernetes.io/zh/docs/concepts/overview/what-is-kubernetes/)



## Docker

### docker部署项目

docker network create

### Docker 介绍及安装

```
首先通过一张表来看Docker容器与传统虚拟机技术的特性对比：
```

| 特 性      | Docker容器         | 传统虚拟机 |
| ---------- | ------------------ | ---------- |
| 启动速度   | 秒级               | 分钟级     |
| 硬盘使用   | 一般为MB           | 一般为GB   |
| 性能       | 接近原生系统       | 相对弱于   |
| 系统支持量 | 单机支持上千个容器 | 一般几十个 |



### Docker核心概念

- 镜像

  Docker镜像（image）类似于虚拟机镜像，可以将它理解为一个面向Docker引擎的只读模板，包含了文件系统。

    	例如：一个镜像可以只包含一个完整的Centos操作系统环境，可以把它称为一个Centos镜像，镜像也可以安装了Nginx应用程序（或者用户需要的其他软件），可以把它称为一个Nginx镜像。
  ​		
    	镜像是创建Docker容器的基础，通过版本管理和增量的文件系统，Docker提供了一套十分简单的机制（仓库）创建和更新现有镜像，用户可以从网上下载一个已经做好的应用镜像，并通过命令直接创建Docker容器来使用。

- 容器

  Docker容器（Container）类似于一个箱子，可以把容器看做是一个简易的Linux系统环境（其中包括root用户权限，进程空间，用户空间和网路空间等），以及运行在其中的应用程序打包而成的一个箱子。Docker利用容器这个箱子来隔离和运行应用镜像。

    	容器是从镜像创建的应用运行实例，可以对他进行启动，停止，删除等常规操作。这些不同的容器之间都是相互隔离互不可见的。镜像自身是只读的，容器从镜像启动的时候，Docker会在镜像的最上层创建一个可写层，镜像本身将保持不变。

- 仓库

  Docker仓库（Repostory）类似于代码的仓库（与svn、git、maven等概念类似）是Docker用来集中存放镜像文件的场所。

  根据所存储的镜像是否公开分享，Docker仓库又分为：

  - 1.公开仓库  

  - 2.私有仓库

    ```
    顾名思义，公开仓库就是公共开放的镜像存储的地方，目前最大的公开仓库是Dokcer Hub （registry.hub.docker.com），存放了大量的镜像可供下载使用，国内的公开仓库有aliyun（acs-public-mirror.oss-cn-hangzhou.aliyuncs.com）。私有仓库是内部使用的私有不对外开放的仓库，用户可以内部自行搭建，内部分享镜像，方便快捷的分享专属环境的镜像文件。
    ```

### Docker安装

```
Docker支持在所有主流操作系统上运行（Ubuntu、CentOS、Windows以及MacOS）。按照[官网指示](https://docs.docker.com/)配置相应的环境。
```

### 在docker中配置预测算法环境

1. 在系统中安装docker

2. 创建一个VM来运行容器

   ```shell
   $ docker-machine create -d virtualbox --virtualbox-disk-size "50000" --virtualbox-cpu-count "4" --virtualbox-memory "8092" docker2
   ```

3. 运行第2步新建的VM

   ```shell
   $ docker-machine start docker2
   $ eval $(docker-machine env docker2)
   ```

4. 请求你所需要的镜像

   ```shell
   $ docker pull kaggle/python
   ```

   此时也可以配置一些快捷指令使工作更加方便，这些不是必须的，但是会使工作变得便捷：

   ```shell
   kpython(){
     docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python python "$@"
   }
   ikpython() {
     docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python ipython
   }
   kjupyter() {
     (sleep 3 && open "http://$(docker-machine ip docker2):8888")&
     docker run -v $PWD:/tmp/working -w=/tmp/working -p 8888:8888 --rm -it kaggle/python jupyter notebook --no-browser --ip="\*" --notebook-dir=/tmp/working
   }
   ```

   上面可以创建和kaggle上一致的环境，之后就可以在本地测试kaggle上对不同数据集进行各种处理。



#### Docker核心技术与实现原理

- 命名空间 (namespaces)

  运行在同一台机器上的不同服务能做到**完全隔离**，就像运行在多台不同的机器上一样。

  命名空间为新创建的进程隔离了文件系统、网络，并与宿主机器之间的进程相互隔离

  - 进程

  - 网络

    - Host

    - Container

    - None

    - Bridge

      当 Docker 服务器在主机上启动之后会创建新的虚拟网桥 docker0，随后在该主机上启动的全部服务在默认情况下都与该网桥相连

      - libnetwork

        提供了一个连接不同容器的实现，同时也为应用给出一个能够提供一致的编程接口和网络层抽象的容器网络模型

        - Sandbox

          存储着当前容器的网络栈配置，包括容器的接口、路由表和 DNS 设置，Linux 使用网络命名空间实现这个 Sandbox，每一个 Sandbox 中都可能会有一个或多个 Endpoint，在 Linux 上就是一个虚拟的网卡 veth，Sandbox 通过 Endpoint 加入到对应的网络中，这里的网络可能就是我们在上面提到的 Linux 网桥或者 VLAN

        - Endpoint

        - Network

  - 挂载点

- 控制组（CGroups）

  命名空间不能提供物理资源上的隔离，比如CPU或者内存

- UnionFS（存储驱动）

  后来改到AUFA，目前采用overlay2取代了aufs成为了推荐的存储驱动

  把多个文件系统『联合』到同一个挂载点的文件系统服务

  docker的镜像是如何组织的：每一层镜像都建立在另一层镜像之上，并且镜像都是只读的，每当基于一个镜像新建一个容器时，等于在镜像的最上层添加一个可以读写的层。

- reference

  https://draveness.me/docker





#### Docker入门与实践

> mac版的docker网络配置，mac版封装好了用xhyve创建的虚拟机，这台虚拟机才是docker的宿主机。

https://yq.aliyun.com/articles/40494?spm=a2c4e.11153959.teamhomeleft.95.44ab18b1OELKiS

[mac的例外之处](https://docs.docker.com/docker-for-mac/networking/#use-cases-and-workarounds)

宿主机无法直接和容器互ping，但是可以进行端口xport映射，从而通过localhost:xport访问容器内的服务

- docker run -d -it --rm ubuntu bash 

  - `-it`：这是两个参数，一个是 `-i`：交互式操作，一个是 `-t` 终端。我们这里打算进入 `bash` 执行一些命令并查看返回结果，因此我们需要交互式终端。
  - `--rm`：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 `docker rm`。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 `--rm` 可以避免浪费空间。
  - -d:此时容器会在后台运行并不会把输出的结果 (STDOUT) 打印到宿主机上面(输出结果可以用 `docker logs`查看)

- docker commit

- ```bash
  docker commit \
      --author "Changxin Cheng <chengcx1019@gmail.com>" \
      --message "添加了pyspark，可通过python接口调用spark服务" \
      nodemaster \
      sparkbase:v2
  ```

- docker build .  --no-cache -t {image_name}

  - `-t`:镜像的命名和可选的tag标签可以以 'name:tag' 这种形式
  - `.`:代表当前上下文环境，docker引擎会打包上下文内的所有文件进行镜像构建

- docker system df

  查看镜像、容器、数据卷所占用的空间

- docker exec

  在container内运行指令

  - docker exec -it {container-hash} bash 

- docker logs {container-hash}

##### compose

`docker-compose`

1707174376413

先用docker搭一个zookeeper集群吧

容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。









## docker and Kubernetes

https://hackernoon.com/the-best-architecture-with-docker-and-kubernetes-myth-or-reality-77b4f8f3804d

- 如何构建一个数据密集型应用
- 如何构建一个机器学习应用
- 微服务：微服务的实施
- 数据库索引



## opensource

[github](https://github.blog/2017-11-07-github-welcomes-all-ci-tools/)

[gitlab](https://docs.gitlab.com/ee/ci/quick_start/)



## 数据库备份

### mysql

https://www.linode.com/docs/databases/mysql/use-mysqldump-to-back-up-mysql-or-mariadb/

https://www.jotform.com/blog/how-to-backup-mysql-database/



## Docker + jenkins + github + django cicd 实践

[jenkins docker](https://github.com/jenkinsci/docker/blob/master/README.md)

3c5868f0fabe4691b91e35d5cfcf6111

先部署，再docker化，不然你不知道是引入docker产生的问题，还是本身的问题



ubuntu install mysql nginx:

mysql无法远程连接:

```sql
CREATE USER 'blogcx'@'localhost' IDENTIFIED BY '789012';

GRANT ALL PRIVILEGES ON *.* TO 'blogcx'@'localhost' WITH GRANT OPTION;

CREATE USER 'blogcx'@'%' IDENTIFIED BY '789012';

GRANT ALL PRIVILEGES ON *.* TO 'blogcx'@'%' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

自动化流程包括：

创建数据库blogcx；

导入最新备份的sql文件；（同时也引入了定时备份的任务及sql数据的存储）



**构建前端**

- 安装环境
  - apt-get update
  - apt-get install nodejs
  - apt-get install npm

npm install

npm run build

npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver

npm i -D webpack-cli

**收集静态文件**

每一次前端重新构建之后，都需要重新启动uwsgi，重新收集静态文件(一种快速的检验方式是vue构建的dist中index页面中引入的静态文件是否和stayic目录里的编码一致)

python manage.py collectstatic

 

**启动**

```sh
ps aux | grep uwsgi | awk '{print $2}' | xargs kill -9 
```

Restart nginx:`sudo nginx -s stop && sudo nginx`



建立log文件夹

修改server端口时，同时需要修改vue的store请求base_url

**sentry监控网络状态**



**bug**:

进入详情页后mathjax没有渲染

https

## redis

监控redis运行状态：redis-cli -h localhost -p ${port} monitor 

