---
layout: post
title: Docker常用命令
date: 2020-09-30 10:30:00 +0900
category: Docker
---

#### 一、镜像管理命令
1、拉取镜像
`docker pull {image_name}`
`docker pull {image_name}:2.3`拉取指定版本镜像
2、推送镜像
`docker push {image_name}`
3、查看当前机器的所有镜像
`docker images`
4、删除当前机器的镜像
`docker rmi {image_name}`
5、强制删除镜像
`docker rm -f {image_name}`
6、为镜像打tag
`docker tag {source_image_name:tag your_image_name:tag}`
7、获取容器/镜像的元数据
`docker inspect {image_id}|{image_name}|{container_name}|{container_id}`

#### 二、容器管理命令
1、运行容器
`docker run --name {your_name} --d {image_name}`
`docker run -d --name {your_name} -p 8080:8080 {image_name} #端口`
2、查看当前所有容器
`docker ps -s -a`
3、停止容器
`docker stop {container_name}`
4、杀死容器
`docker kill {container_name}`
5、删除容器
`docker rm -f {container_name}`
6、启动容器
`docker start {container_name}`
7、重启容器
`docker restart {container_name}`

#### 三、查看相关信息log
1、查看容器日志
`docker logs -f {image_name}`
2、查看docker服务的信息
`docker info`
3、查看容器的元数据
`docker inspect`

#### 四、与容器交互的命令
1、进入容器shell交互
`docker exec -it {image_name} bash #进入容器的shell `
指定容器来执行命令
`docker exec {image_name} {命令}`
`docker exec -d {image_name} {命令} #后台运行`
`docker exec jenkins echo "hello word"`

2、把宿主机的文件copy到容器中
`docker cp {host_path} {container:name}:{container_path}`
`docker cp 'pwd'/start.sh jenkins:/home`

#### 五、容器运行命令的参数
```
1、 —name 指定容器名称
2、 -d 后台运行
3、 -port 指定端口映射规则
4、 -network 指定容器运行的网络模式
5、 -v 指定需要挂载的数据卷
6、 -env 指定需要传递给容器的环境变量
```

#### 六、网络模式的指定
```
1、四种网络模式：Container、briage、Host、none
2、docker run -itd --name {container_name} --net {网络模式}
```

#### 七、搭建Jenkins
```
1、 docker pull jenkins
2、 docker run --name myjenkins -itd -p 8001:8080 -v /Users/tmp/jenkins:/var/jenkins_home --env JAVA_OPTS="-Xmx8192m" jenkins
**修改Java应用程序的内存,-Xmx8192mw为8G**
3、 docker logs -f myjenkins
```
备注： 需要修改下⽬录权限, 因为当映射本地数据卷时，/Users/tmp/jenkins⽬录的拥有者为root用户，⽽容器中jenkins user的uid为1000
`sudo chown -R 1000 /Users/tmp/jenkins `

#### 八、搭建MYSQL并连接服务
```
1、docker run -p 13306:3306 --name my-mysql -v $PWD/docker/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql
2、docker run -d --name test_sleep_infinity --link some-mysql centos sleep infinity
```

