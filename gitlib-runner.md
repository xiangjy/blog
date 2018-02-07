---
title: gitlib-runner
date: 2017-09-08 17:26:17
tags: 持续集成
---
### 1.使用docker运行gitlab-runner
```
gitlab-runner:
    container_name: gitlab-runner
    image: gitlab/gitlab-runner:latest
    restart: always
    volumes:
        - /etc/localtime:/etc/localtime
        - /home/data/docker/gitlab-runner/home/.pypirc:/root/.pypirc
        - /var/run/docker.sock:/var/run/docker.sock
        - /home/data/docker/gitlab-runner/config:/etc/gitlab-runner
    command: [run, --working-directory=/home/gitlab-runner]
    dns:
        - 192.168.0.100
```
### 2.进入docker
```
sudo docker exec -it gitlab-runner bash
```
### 3.执行gitlab-runner或gitlab-ci-multi-runner reigster
```
$ sudo gitlab-runner register

Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
## 输入gitlab地址
Please enter the gitlab-ci token for this runner
## gitlab的token(在gitlab的Admin Area中) 或者仓库的token(仓库->设置->Runner)
Please enter the gitlab-ci description for this runner
## Runner描述信息
Please enter the gitlab-ci tags for this runner (comma separated):
## Runner的标签 可以指定仓库，之后在.gitlab-ci.yml中配置相对应tags 
Please enter the executor: parallels, shell, ssh, virtualbox, docker+machine, docker-ssh+machine, docker, docker-ssh, kubernetes:
docker  ## 执行类型 
Please enter the Docker image (eg. ruby:2.1):
build-image  ## 编译环境的镜像
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```
注册runner执行类型有多种方式，具体参考：[Using Docker Build](https://docs.gitlab.com/ce/ci/docker/using_docker_build.html)
### 4.查看已经注册的runner
```
gitlab-runner list
Listing configured runners                          ConfigFile=/mypath/.gitlab-runner/config.toml
```
### 5.配置.gitlab-ci.yml
详情参考：[gitlab-ci说明文档](https://docs.gitlab.com/ce/ci/yaml/README.html)
