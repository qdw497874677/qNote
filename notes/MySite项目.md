# MySite项目





项目原地址

[WinterChenS/my-site: springboot2.0开发的个人网站，集成了：个人首页，个人博客，个人作品 (github.com)](https://github.com/WinterChenS/my-site)



项目地址

[qdw497874677/my-site-qdw: springboot2.0开发的个人网站，集成了：个人首页，个人博客，个人作品 (github.com)](https://github.com/qdw497874677/my-site-qdw)





## 启动







### 打docker镜像

```bash
mvn package docker:build -DSkipTests=true
```



### 启动镜像

后台启动

```bash
docker run -d -p 8080:8080 qdw/my-site
```

docker ps找到启动的容器，进入容器命令行

```bash
docker exec -it 01e01fd4c1db /bin/bash
```







