---
layout:     post
title:      "树莓派配置科学上网"
subtitle:   "v2rayA 安装与全局代理设置"
date:       2025-07-20 12:00:00
author:     "Hxin"
header-img: "img/post-bg-rwd.jpg"
catalog:    true
tags:
    - 树莓派
    - 技术
    - 教程
---

## 安装 v2rayA

#### 1. 添加公钥
```bash
wget -qO - https://apt.v2raya.org/key/public-key.asc | sudo tee /etc/apt/keyrings/v2raya.asc
```

#### 2. 添加V2RayA软件源
```bash
echo "deb [signed-by=/etc/apt/keyrings/v2raya.asc] https://apt.v2raya.org/ v2raya main" | sudo tee /etc/apt/sources.list.d/v2raya.list
sudo apt update
```

#### 3. 安装V2RayA
```bash
sudo apt install v2raya v2ray
```

#### 4. 启动v2rayA
```bash
sudo systemctl start v2raya
```

#### 5. 设置开机自启
```bash
sudo systemctl enable v2raya
```

#### 6. 查看服务状态
```bash
systemctl status v2raya
```

---

## 配置 v2rayA 客户端

#### 1. 获取树莓派IP
在终端输入 `hostname -I` 获取你的树莓派IP地址。

#### 2. 访问Web界面
在任何位于同一局域网的设备（电脑、手机）的浏览器中，访问 `http://<你的树莓派IP地址>:2017`。

#### 3. 初始化设置
* 首次访问需要创建管理员用户名和密码。
* 登录后，点击左侧“导入(Import)”，粘贴你的 VPN订阅链接 并更新。
* 回到主页，从列表中选择一个节点。
* 点击右侧的选择，当它变为“取消”并显示延迟时，表示连接成功。

---

## 配置系统全局代理
这是让树莓派上所有应用都通过VPN上网的关键一步。

#### 1. 编辑全局环境配置文件
```bash
sudo nano /etc/environment
```

#### 2. 添加代理配置
在文件末尾添加以下内容 (v2rayA 默认的HTTP代理端口为 `20171`)：
> **注意**：此处为纯文本，请勿添加其他格式。

```
http_proxy="http://127.0.0.1:20171"
https_proxy="http://127.0.0.1:20171"
no_proxy="localhost,127.0.0.1"
```

#### 3. 保存文件
按 `Ctrl+X` -> `Y` -> `Enter` 保存并退出。

#### 4. 重启树莓派
此配置**必须重启**才能完全生效。
```bash
sudo reboot
```

---

## 验证配置
重启后，打开终端，执行以下命令来检查你的公网出口IP。

```bash
curl ip.sb
```
或者
```bash
curl myip.ipip.net
```

如果返回的 IP 地址是你所选 VPN 节点的 IP，那么恭喜你，所有配置均已成功！
