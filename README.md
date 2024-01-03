# 原项目介绍

原项目是 ** [TG-FileStreamBot](https://github.com/EverythingSuckz/TG-FileStreamBot) ** ，这个项目是用于生成telegram文件直连的项目，将你需要下载的telegram文件转发给机器人，你就可以得到一个链接，访问链接就可以直接下载文件。

<h1 align="center">Telegram File Stream Bot</h1>
<p align="center">
  <a href="https://github.com/EverythingSuckz/TG-FileStreamBot">
    <img src="https://socialify.git.ci/EverythingSuckz/TG-FileStreamBot/image?description=1&font=Source%20Code%20Pro&forks=1&issues=1&logo=https://telegra.ph/file/01385a9f4cf0419682b87.png&pattern=Circuit%20Board&pulls=1&stargazers=1&theme=Dark" alt="Cover Image" width="650">
  </a>
  <p align="center">
    A Telegram bot to <b>generate direct link</b> for your Telegram files.
    <br />
    <a href="https://telegram.dog/TG_FileStreamBot"><strong><s>Demo Bot »</s></strong></a>
    <br />
    <a href="https://github.com/EverythingSuckz/TG-FileStreamBot/issues">Report a Bug</a>
    |
    <a href="https://github.com/EverythingSuckz/TG-FileStreamBot/issues">Request Feature</a>
  </p>
</p>



# 本项目产生的原因

        本项目**[TGlinkToAria2](https://github.com/snakexgc/TGlinkToAria2)**是在**[TG-FileStreamBot](https://github.com/EverythingSuckz/TG-FileStreamBot)**的基础上，增加了一个自动将生成的直链加入到aria2中进行下载的这一步，他产生的原因是在我的使用过程中，我每次都需要复制他给我生成的直连，然后手动发送到aria2控制机器人中进行下载，如果文件比较多，就要高强度重复复制，粘贴，发送的操作，而且有时候容易搞错，导致重复下载或者漏下载。

        因此，我在想能不能直接让直链能够在生成后直接发送到aria2中下载，近几日在修复隔壁项目aria2bot的过程中，发现了一个很适合的python的库—aria2p，这个库非常的精简，很小的体积就能完成我需要的工作，于是我在查看了https://github.com/EverythingSuckz/TG-FileStreamBot的代码后，对代码做了适当修改，让他能够自动将连接加入到aria2中。

# 区别

1. 加入自动添加aria2下载任务的功能

本项目最大区别就是加入aria2的支持，所以新增了四个环境变量：

```jsx
ARIA2=True
RPC_URLS=http://1.1.1.1
RPC_PORTS=6800
RPC_TOKENS=youraria2tocken
```

四个环境变量的解释

- 第一个是控制是否启用aria2的，如果不用aria2，那我建议您用原项目…
- 第二个是你aria2的链接，请务必按格式更改
- 第三个是aria2的对外监听端口，一般是6800
- 第四个是aria2的秘钥
1. 优化输出
- 在没有启用aria2的时候，如果您坚持使用本项目，那么您得到的结果和官方返回基本一致，展示的链接是主链接，并且会提示没有启用aria2。
- 如果启用了aria2，为了使展示更加美观，返回结果会变成短连接，长连接用link的形式隐藏，也可以直接访问，并切会有相应提示！

# 部署

## 安装docker

```jsx
curl -fsSL get.docker.com -o get-docker.sh&&sh get-docker.sh &&systemctl enable docker&&systemctl start docker
```

## 拉取仓库

```jsx
cd /root

git clone https://github.com/snakexgc/TGlinkToAria2

cd TGlinkToAria2
```

## 构建docker镜像

```jsx
docker build . -t stream-bot
```

## 在`TGlinkToAria2` 文件下创建.env文件

文件内容如下：

```jsx
API_ID=452525
API_HASH=esx576f8738x883f3sfzx83
BOT_TOKEN=55838383:yourtbottokenhere
MULTI_TOKEN1=55838383:yourfirstmulticlientbottokenhere
ARIA2=True
RPC_URLS=http://1.1.1.1
RPC_PORTS=6800
RPC_TOKENS=youraria2tocken
BIN_CHANNEL=-100
PORT=8080
FQDN=yourserverip
HAS_SSL=False
```

对于这些环境变量的意义，我想大家应该都懂，不懂可以看项目的[readme](https://github.com/snakexgc/TGlinkToAria2/blob/main/README.md)

## 创建容器

```jsx
docker run -d --restart unless-stopped --name fsb \
-v /root/TGlinkToAria2/.env:/app/.env \
-p 8080:8080 \
stream-bot
```

        您的“PORT”变量必须与容器的公开端口一致，因为它用于生成URL。所以请记住，如果您更改了“PORT”变量，您的docker run命令也会更改。（例如：`PORT=9000`->`-p 9000:9000`）

        如果您需要在bot启动后更改“.env”文件中的变量，您所需要做的就是重新启动容器以更新bot设置：

```jsx
docker restart fsb
```

这里您需要注意，修改端口就需要一次修改两个地方的端口，而不是想之前的青龙之类的一样，只用修改启动命令，您需要修改

1. .env文件中的 `PORT` ，不是`RPC_PORTS` 请看清楚！
2. 修改部署容器的命令中的-p 而且是全部需要改！


例如您想使用1111端口，那么.**env修改成1111**，然后 **-p 1111:1111 \**

# 结束

        如果不出意外，您像机器人发送文件后，文件生成直链的同时就会同步将文件添加进入aria2中进行下载！

# 项目最终目的

        如果有时不时瞅一眼的朋友应该发现了，之前我修改了aria2bot这个项目，最近有修改了这个项目，我最终的目的是**将这两个项目合并，并且加入rclone的功能**，只不过由于水平问题，目前还不能实现这些功能，只能先这样使用了，耐心等待吧！
