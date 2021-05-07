# Docker的安装及使用

Docker 是一个开源的应用容器引擎，并且进行了进一步的封装，让用户不需要去关心容器的管理，使得操作更为简便。能够实现快速交付应用程序、快速构建和轻松管理



## Ubuntu下的Docker安装

+ 安装 Docker

  ```
  curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
  ```

+ 测试 Docker 是否安装成功

  ```
  sudo docker run hello-world
  ```

+ 使用 Docker 作为非 root 用户

  ```
  sudo usermod -aG docker your-user
  ```



## 镜像

镜像是用于创建容器的模板，比如 Ubuntu 系统

+ 列出镜像

  ```
  docker images
  ```

  > - **REPOSITORY：**镜像的仓库源
  > - **TAG：**镜像的标签
  > - **IMAGE ID：**镜像ID
  > - **CREATED：**镜像创建时间
  > - **SIZE：**镜像大小

+ 查找镜像

  ```
  docker search 镜像
  ```

+ 获取镜像

  ```
  docker pull 镜像
  ```

+ 创建镜像

  > 从已有镜像修改
  >
  > 1. 使用镜像创建容器
  >
  >    ```
  >    docker run -t -i httpd /bin/bash
  >    ```
  >
  > 2. 修改镜像
  >
  >    ```
  >    apt-get update
  >    ```
  >
  > 3. 提交更新后的镜像副本
  >
  >    ```
  >    docker commit -m="update" -a="clear" 87d2dba121df clear/httpd:v2
  >    docker images
  >    ```
  >
  >    ![image-20210424175958102](images\image-20210424175958102.png)
  >
  > 构建新的镜像
  >
  > 1. 创建 Dockerfile
  >
  > ```
  > vim Dockerfile
  > ```
  >
  > ![image-20210424201808838](images\image-20210424201808838.png)
  >
  > 2. 生成镜像
  >
  > ```
  > docker build [选项] PATH | URL | -
  > docker build -t clear/centos .
  > docker images
  > ```
  >
  > ![image-20210424201651548](images\image-20210424201651548.png)

+ 设置镜像标签

  ```
  docker tag 镜像:标签
  ```

+ 保存镜像

  ```
  docker save 镜像
  ```

+ 载入镜像

  ```
  docker load 镜像
  ```

+ 上传镜像

  ```
  docker push 镜像
  ```

+ 删除镜像

  ```
  docker rmi 镜像
  ```




## 容器

容器是独立运行的一个或一组应用，是镜像运行时的实体

+ 启动容器

  > 基于镜像新建一个容器并启动
  >
  > ```
  > docker run -t -i 镜像 /bin/bash
  > -d 后台运行，不会进入容器
  > ```
  >
  > 将终止状态的容器启动
  >
  > ```
  > docker start 容器
  > ```
  >
  > 重启容器
  >
  > ```
  > docker restart 容器
  > ```

+ 查看容器信息

  ```
  docker ps
  docker ps -a 查看全部容器信息
  ```

+ 进入容器

  > attach 命令
  >
  > ```
  > docker attach 容器
  > ```
  >
  > exec 命令，退出容器不会停止
  >
  > ```
  > docker exec -it 容器 /bin/bash
  > ```

+ 终止容器

  ```
  docker stop 容器
  ```

+ 导出和导入容器

  > 导出容器
  >
  > ```
  > docker export 容器
  > ```
  >
  > 导入容器快照
  >
  > ```
  > docker import 容器
  > ```

+ 删除容器

  ```
  docker rm 容器
  ```



## Volumes

数据卷是一个可供一个或多个容器使用的特殊目录

+ 直接创建一个数据卷

  ```
  docker run -d -P --name web -v /webapp training/webapp python app.py
  ```

+ 挂载一个主机目录作为数据卷

  ```
  docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py
  ```

+ 挂载一个本地主机文件作为数据卷

  ```
  docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
  ```

+ 数据卷容器

  > 创建数据卷容器
  >
  > ```
  > docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres
  > ```
  >
  > 挂载容器中的数据卷
  >
  > ```
  > docker run -d --volumes-from dbdata --name db1 training/postgres
  > ```

+ 利用数据卷容器来备份、恢复

  > 备份
  >
  > 创建一个加载容器卷的容器
  >
  > ```
  > sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
  > ```
  >
  > 恢复
  >
  > 创建一个带有数据卷的容器
  >
  > ```
  > sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
  > ```
  >
  > 创建另一个容器，挂载数据卷容器，并解压备份文件到挂载的容器卷中
  >
  > ```
  > sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf
  > ```



## 端口映射

+ 创建了一个 python 应用的容器

  ```
  docker run -d -P training/webapp python app.py
  docker ps
  ```

+ 指定容器端口绑定到主机端口

  ```
  docker run -d -p 5000:5000 training/webapp python app.py
  ```

+ 指定容器绑定的网络地址

  ```
  docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py
  ```

+ 查看端口的绑定情况

  ```
  docker port stupefied_spence 5000
  ```

  ![image-20210425152554852](images\image-20210425152554852.png)



## Odoo 的 Docker 安装

+ 创建 docker-compose.yml 文件

  ```
  mkdir odoo14
  cd odoo14
  vim docker-compose.yml
  ```

  ![image-20210425170327343](images\image-20210425170327343.png)

+ 拉取镜像生成容器并运行

  ```
  docker-compose up -d
  ```

+ 浏览器输入 ip:8069

  ![image-20210425171101946](images\image-20210425171101946.png)




