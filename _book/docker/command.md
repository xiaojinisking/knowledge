# 常用命令

* 构建镜像
  docker build -t 镜像名称  .   --no-cache

* 启动容器
  docker run -itd --name 容器名称  镜像名称

* 进入容器
  docker exec -it 容器名称 bash

* 创建网络
  docker network create --subnet=192.168.1.0/24 swoftNetwork

  --subnet 子网地址

  swoftNetwork 网络名称

  IP计算器 https://tool.520101.com/wangluo/ipjisuan/

  根据子网掩码 24计算出 游多少个可用的IP地址

  ![](assets/markdown-img-paste-20190713195143543.png)


* -- privileged 参数
  使用该参数，可以使容器内的root拥有真正的root权限

 ![](assets/markdown-img-paste-20190725101043184.png)

 docker run -t -i --privileged centos:latest bash
