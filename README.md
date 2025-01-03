# ImmortalWrt Docker Image Builder

[![Build Status](https://github.com/xugd2019/openwrt-docker-pi5/actions/workflows/build.yml/badge.svg)](https://github.com/xugd2019/openwrt-docker-pi5/actions)

本项目通过 GitHub Actions 实现了自动化构建和推送自定义适用于树莓派 4～5的 [ImmortalWrt](https://github.com/immortalwrt/immortalwrt) Docker 镜像。工作流会在代码更新时或每天凌晨 1 点（UTC）自动触发。

## 功能特点

- 自动化编译、构建 ImmortalWrt 镜像。
- 上游为ImmortalWrt 23.05，主题使用argon。
- 支持通过 `packages.config` 文件自定义包列表。
- 自动生成并推送 Docker 镜像至 Docker Hub。

## 自动构建使用方法

### 先决条件

1. 注册一个 [Docker Hub](https://hub.docker.com/) 账号。
2. 在 Docker Hub 中生成一个具有“读/写”权限的访问令牌。
3. 将以下密钥添加到 GitHub 仓库的 `Settings > Secrets and variables > Actions` 中：
    - `DOCKER_USERNAME`：您的 Docker Hub 用户名。
    - `DOCKER_PASSWORD`：您的 Docker Hub 访问令牌。

### 部署构建步骤

1. Fork 本仓库。
2. 编辑 `packages.config` 文件，将您需要的包名称按行添加到文件中。
2. 编辑 `network` 文件，修改为您的网段。
3. 推送代码更改至仓库，即可触发自动化构建。

GitHub Actions 工作流将执行以下操作：
- 下载 ImmortalWrt ImageBuilder。
- 修改 `.config` 文件，编译出rootfs.tar.gz。
- 提取生成的 `rootfs.tar.gz` 文件至 Docker 构建上下文。
- 构建并推送 Docker 镜像至 Docker Hub。

### 定时构建

工作流已配置为每天凌晨 1 点（UTC）自动运行。您可以通过修改 `.github/workflows/build.yml` 中的 `schedule` 部分调整运行时间。

### 输出

生成的 Docker 镜像将推送至：
```
docker.io/xugd/openwrt-pi5:latest
```

### 示例配置

以下是一个 `packages.config` 文件的示例：

```
luci-app-firewall
luci-app-opkg
luci-app-smartdns
luci-app-passwall
zsh
tmux
wget-ssl
```

以下是一个 `network` 文件的示例：

```
config interface 'lan'
        option proto 'static'
        option netmask '255.255.255.0'
        option ipaddr '192.168.1.105'
        option gateway '192.168.1.103'
        option dns '192.168.1.103'
        option device 'br-lan'
```
### 获取生成的 Docker 镜像

拉取镜像：
```bash
docker pull xugd/openwrt-pi5:latest
```

## 贡献指南

欢迎贡献！您可以 Fork 本仓库，进行修改后提交 Pull Request。

## 许可证

本项目使用 GPL 许可证，详情请参阅 `LICENSE` 文件。

## 鸣谢

- [ImmortalWrt](https://github.com/immortalwrt/immortalwrt)
- [Docker](https://www.docker.com/)
- [SuLingGG](https://github.com/SuLingGG)