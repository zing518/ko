# 某容器云部署Xray高性能代理服务

在某容器云部署Xray高性能代理服务，通过ws传输的(vmess、vless、trojan、shadowsocks、socks)等协议

Kxxxb：https://owo.misaka.rest/koyeb-xray/

Flyio（已被检测，需要付费）：https://owo.misaka.rest/flyio-xray/

# 请勿使用常用的账号部署此项目，以免封号！！

关于本脚本加密sh文件的说明：由于某容器云已识别本脚本，故不得不加密项目的sh文件代码

## 部署步骤

<details>
    <summary>Fly.io 容器云 （已被检测，需要付费）</summary>
1. 使用以下命令安装Flyctl工具

Windows：`iwr https://fly.io/install.ps1 -useb | iex` 

（请使用PowerShell或Windows终端的管理员模式安装）

Mac OS / Linux：`curl -L https://fly.io/install.sh | sh`

2. 下载[KOXray项目文件](https://github.com/Misaka-blog/KOXray)，并解压
    
3. 修改`Dockerfile`内第3-5行修改自定义设置，说明如下：

`AUUID`：用来部署节点的UUID，如有需要可在[uuidgenerator](https://www.uuidgenerator.net/)生成

`CADDYIndexPage`：伪装站首页文件

`ParameterSSENCYPT`：ShadowSocks加密协议

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423024827.png)

4. 右键当前目录，点击“从终端中打开”

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423024905.png)

5. 输入`flyctl auth login`，在CLI页面登陆自己的Fly.io账号
    
6. 输入`flyctl launch`，然后按照下图设置

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423025437.png)

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423025618.png)

7. 修改CLI生成的`fly.toml`文件，将`internal_port = 8080`改为`internal_port = 80`

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423025720.png)

8. 回到命令行，输入`flyctl deploy`进行推送至Fly.io

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423025825.png)

9. CLI推送成功之后，在Fly.io的控制面板会提示刚刚部署的应用

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423030020.png)

10. 在这里你可以看到应用信息，复制Hostname备用

![](https://cdn.jsdelivr.net/gh/Misaka-blog/tuchuang@master/20220423030108.png)

11. 客户端配置如下
    
V2ray

```
地址：appname.fly.dev
端口：443
默认UUID：24b4b1e1-7a89-45f6-858c-242cf53b5bdb
vmess额外id：0
加密：none
传输协议：ws
伪装类型：none
伪装域名：appname.fly.dev
路径：/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-vless
vless使用(/自定义UUID码-vless)，vmess使用(/自定义UUID码-vmess)
底层传输安全：tls
跳过证书验证：false
```

Trojan-go

```bash
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "appname.fly.dev",
    "remote_port": 443,
    "password": [
        "24b4b1e1-7a89-45f6-858c-242cf53b5bdb"
    ],
    "websocket": {
        "enabled": true,
        "path": "/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-trojan",
        "host": "appname.fly.dev"
    }
}
```

ShadowSocks

```bash
服务器地址: appname.fly.dev
端口: 443
密码：24b4b1e1-7a89-45f6-858c-242cf53b5bdb
加密：chacha20-ietf-poly1305
插件程序：xray-plugin_windows_amd64.exe
说明：需将插件 https://github.com/shadowsocks/xray-plugin/releases 下载解压后放至shadowsocks同目录
插件选项: tls;host=appname.fly.dev;path=/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-ss
```
    
</details>

<details>
    <summary>Kxxxb 容器云</summary>
1. Fork本仓库并改名
    
2. 在`Dockerfile`内第3-5行修改自定义设置，说明如下：

`AUUID`：用来部署节点的UUID，如有需要可在[uuidgenerator](https://www.uuidgenerator.net/)生成

`CADDYIndexPage`：伪装站首页文件

`ParameterSSENCYPT`：ShadowSocks加密协议

3. 去[Docker Hub](https://hub.docker.com/)注册一个账号，如有账号可跳过
    
4. 编辑Actions文件`docker-image.yml`，按照“name: Docker Hub ID/自定义镜像名称”格式修改第13行
    
5. 添加Actions的Secrets变量，变量说明如下

`DOCKER_USERNAME`：Docker Hub ID

`DOCKER_PASSWORD`：Docker Hub 登录密码

6. 打开某容器云主页，新建一个应用
    
7. 应用配置如下所示

`Docker Image`：Docker Hub镜像地址，格式为“docker.io/Docker Hub ID/自定义镜像名称”

`Container size`：部署配置，一般默认即可

`Port`：80

Environment variables：`Key`：PORT，`Value`：80
`Name`：自己定义

8. 客户端配置如下所示

V2ray

```
地址：xxx-xxx.koyeb.app 或 CF优选IP
端口：443
默认UUID：24b4b1e1-7a89-45f6-858c-242cf53b5bdb
vmess额外id：0
加密：none
传输协议：ws
伪装类型：none
伪装域名：xxx-xxx.koyeb.app
路径：/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-vless
vless使用(/自定义UUID码-vless)，vmess使用(/自定义UUID码-vmess)
底层传输安全：tls
跳过证书验证：false
```

Trojan-go

```bash
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "xxx-xxx.koyeb.app",
    "remote_port": 443,
    "password": [
        "24b4b1e1-7a89-45f6-858c-242cf53b5bdb"
    ],
    "websocket": {
        "enabled": true,
        "path": "/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-trojan",
        "host": "xxx-xxx.koyeb.app"
    }
}
```

ShadowSocks

```bash
服务器地址: xxx-xxx.koyeb.app
端口: 443
密码：24b4b1e1-7a89-45f6-858c-242cf53b5bdb
加密：chacha20-ietf-poly1305
插件程序：xray-plugin_windows_amd64.exe
说明：需将插件 https://github.com/shadowsocks/xray-plugin/releases 下载解压后放至shadowsocks同目录
插件选项: tls;host=xxx-xxx.koyeb.app;path=/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-ss
```
    
</details>

## 注意

请勿滥用本仓库

## 赞助我们

![afdian-MisakaNo.jpg](https://s2.loli.net/2021/12/25/SimocqwhVg89NQJ.jpg)

## 交流群
[Telegram](https://t.me/misakanetcn)

## Stars 增长记录

[![Stargazers over time](https://starchart.cc/Misaka-blog/KOXray.svg)](https://starchart.cc/Misaka-blog/KOXray)
