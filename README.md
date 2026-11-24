<div align="center">

<img src="docs/images/hero.svg" alt="TrackFerry" width="100%">

</div>

<br>

> **这是衍生版，在上游 [madrays/trackferry](https://github.com/madrays/trackferry) 的基础上加了 LX Music Server 音源。**
>
> - 上游版权归 madrays 所有，按 MIT 许可发布；本仓库沿用同一许可，`LICENSE` 里的原始版权声明保留不动。
> - 镜像由 GitHub Actions 自动构建，发布在 Docker Hub：**[geelonn/trackferry-lx](https://hub.docker.com/r/geelonn/trackferry-lx)**。**拉 `madrays/trackferry` 的官方镜像拿到的是原版，不含这里的改动。**
> - 与上游的差别见 [CHANGELOG](CHANGELOG.md)。

<br>

TrackFerry 让小爱音箱播放你 NAS 上的歌。说一句“播放潮汐来信”，它先查飞牛音乐，再查酷我、酷狗、咪咕。都没有时，小爱照常回答。

音箱不用刷机，NAS 上跑一个 Docker 容器就行。

<img src="docs/images/route.svg" alt="点歌流程" width="100%">

## 功能

<table>
<tr>
<td width="33%" valign="top"><h3>🎵 飞牛曲库</h3>登录后同步歌曲、封面、歌词和收藏。也可以只读挂载音乐文件夹。</td>
<td width="33%" valign="top"><h3>🌊 在线补歌</h3>曲库没有的歌，从酷我、酷狗、咪咕找免费完整版。每个来源都能单独关闭。</td>
<td width="33%" valign="top"><h3>🗣️ 连续点歌</h3>“换成周杰伦的版本”“换个版本”“收藏这首歌”都能接上。说“播放我的收藏”“播放某某歌单”，就随机一首接一首地放。听错一个字也能按曲库纠正。</td>
</tr>
<tr>
<td valign="top"><h3>⏯️ 断点续播</h3>听歌时和小爱说话，说完从刚才的位置继续。说了暂停就停下。</td>
<td valign="top"><h3>🐟 点歌流转</h3>实时查看一句话经过了哪里、为什么选这首，也能回放以前的记录。</td>
<td valign="top"><h3>🔊 多台音箱</h3>每台音箱单独设为不接入、观察或接管。观察模式只显示会放什么，不出声。</td>
</tr>
</table>

## 界面

<img src="docs/images/home.webp" alt="此刻" width="100%">

### 🐟 流转

| 曲库命中 | 在线查找 |
| :---: | :---: |
| ![曲库命中](docs/images/flow.webp) | ![在线查找](docs/images/flow-online.webp) |

### 💿 曲库与音箱

| 曲库 | 音箱 |
| :---: | :---: |
| ![曲库](docs/images/library.webp) | ![音箱](docs/images/speakers.webp) |

### ⚙️ 怎么说与设置

| 怎么说 | 设置 |
| :---: | :---: |
| ![怎么说](docs/images/say.webp) | ![设置](docs/images/settings.webp) |

### 📱 手机
<p align="center">
  <img src="docs/images/m-home.webp" alt="手机上的此刻" width="24%">
  <img src="docs/images/m-flow.webp" alt="手机上的流转" width="24%">
  <img src="docs/images/m-library.webp" alt="手机上的曲库" width="24%">
  <img src="docs/images/m-settings.webp" alt="手机上的设置" width="24%">
</p>

<sub>截图使用演示数据。</sub>

## 快速开始

准备一台装有 Docker 的 NAS（amd64 或 arm64），以及同一局域网内的小爱音箱。

**1. 下载部署文件**

```sh
mkdir -p trackferry && cd trackferry && curl -fLO https://raw.githubusercontent.com/madrays/trackferry/main/compose.yaml
```

**2. 修改地址**

打开 `compose.yaml`，把 `TRACKFERRY_URL` 改成 NAS 的局域网地址：

```yaml
TRACKFERRY_URL: "http://192.168.1.10:18561"
```

**3. 启动**

```sh
docker compose up -d
```

**4. 查看初始化码**

```sh
docker exec trackferry /trackferry setup-code
```

**5. 完成设置**

打开 `http://<NAS 地址>:18561`，设置管理密码，登录小米账号和飞牛音乐。先把音箱设为“观察”，试过后再改成“接管”。

> [!IMPORTANT]
> 接管前，先让 xiaomusic 等程序退出对这台音箱的控制。

> [!TIP]
> 管理页打不开或音箱不出声，多半是防火墙没有放行 TCP 18561。见[部署教程](docs/deployment.md#第四步放行端口)。

<details>
<summary><b>更新</b></summary>

“设置 → 关于”会提示新版本，点“更新”即可，起不来会自动退回。

也可以用命令：先备份 `data`，再在 `compose.yaml` 所在目录执行：

```sh
docker compose pull && docker compose up -d
```

</details>

<details>
<summary><b>从源码构建</b></summary>

```sh
git clone https://github.com/madrays/trackferry.git && cd trackferry
```

修改 `compose.yaml` 后执行：

```sh
docker compose -f compose.yaml -f compose.build.yaml up -d --build
```

</details>

## 文档

| 使用 | 了解与参与 |
| --- | --- |
| 🚀 [部署教程](docs/deployment.md) | 🧭 [架构](docs/architecture.md) |
| 🗣️ [使用说明](docs/usage.md) | 🔌 [接口](docs/api.md) |
| 🏠 [接入 Home Assistant](docs/homeassistant.md) | 🎧 [音箱的三种模式](docs/usage.md#音箱的三种模式) |
| ⚙️ [配置](docs/configuration.md) | 🤝 [参与贡献](CONTRIBUTING.md) |
| 🩺 [问题排查](docs/troubleshooting.md) | 📝 [更新记录](CHANGELOG.md) |
| 🔐 [隐私与安全](docs/security.md) | 💛 [致谢](docs/credits.md) |

## 参与

问题和建议请提 [Issue](https://github.com/madrays/trackferry/issues)，改代码前看一下[贡献说明](CONTRIBUTING.md)。

<div align="center">

喜欢 TrackFerry 的话，点个 ⭐ 吧。

<a href="https://star-history.com/#madrays/trackferry&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=madrays/trackferry&type=Date&theme=dark">
    <img src="https://api.star-history.com/svg?repos=madrays/trackferry&type=Date" alt="星标趋势" width="720">
  </picture>
</a>

</div>

## 致谢与许可

感谢 [xiaomusic](https://github.com/hanxi/xiaomusic)、[fnmusic-miot](https://github.com/le5inioo/fnmusic-miot)、[songloft-plugin-miot](https://github.com/songloft-org/songloft-plugin-miot)、[fnos_music_ext](https://github.com/javycoder/fnos_music_ext) 和 [musicdl](https://github.com/CharlesPikachu/musicdl)。

TrackFerry 以 [MIT](LICENSE) 许可开源。
