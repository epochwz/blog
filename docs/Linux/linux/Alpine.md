# Alpine

## 自定义镜像

**Initial**

```shell script
cat>$HOME/.dockerignore<<EOF
/*
!Dockerfile
EOF
```

**Alpine**

```shell script
cat>$HOME/Dockerfile<<EOF
FROM alpine:3.12

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && apk update
EOF

docker build -t alpine:aliyun $HOME
# docker run -d --name alpine:aliyun alpine top
# docker exec -it alpine sh
```

**Nodejs**

```shell script
cat>$HOME/Dockerfile<<EOF
FROM alpine:aliyun

RUN apk add nodejs npm \
    && npm config set registry https://registry.npm.taobao.org -g \
    && npm config set disturl https://npm.taobao.org/dist -g
EOF

docker build -t node $HOME
# docker run -d --name node node top
# docker exec -it node sh
```

**docsify**

```shell script
cat>$HOME/Dockerfile<<EOF
FROM node:latest

RUN npm install -g docsify-cli

WORKDIR /docs

EXPOSE 3000

CMD ["docsify", "serve", "."]
EOF

docker build -t docsify $HOME
# docker pull registry.cn-shenzhen.aliyuncs.com/epochwz/dev:docsify
# docker tag registry.cn-shenzhen.aliyuncs.com/epochwz/dev:docsify docsify
# docker rmi registry.cn-shenzhen.aliyuncs.com/epochwz/dev:docsify
# docker run -d -p 3000:3000 -v ~/Documents/Projects/GitHub/epochwz.github.io/docs:/docs --name docsify-blog docsify	
```

## 其他操作

**查看依赖此镜像的其他镜像**

```bash
docker image inspect --format='{{.RepoTags}} {{.Id}} {{.Parent}}' $(docker image ls -q --filter since=<image-id>)
```

**查看镜像的构建历史**

```bash
docker history <image-id/name>
```