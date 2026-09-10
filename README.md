# MyIcon for LazyCat

轻量自托管图标库 — NAS 图标管理工具（Nuxt 4 + Element Plus）。

主页：https://github.com/ljw98/MyIcon

## 版本

首个懒猫包版本 `1.0.0` 对应 `ghcr.io/ljw98/myicon:sha-a89f4aa`，上游完整提交为 `a89f4aa91d81f9a8d527b1995035d5fb245c2cf2`。该镜像与首次检查的 `latest` 摘要一致。1.0.0 是本包版本，并非上游发行版本号。

使用现有预构建镜像，因此原 Compose 的 `build: .` 不再需要。每天跟踪 `latest` 的 amd64 镜像摘要：摘要改变时自动递增懒猫包的 patch 版本（1.0.0 → 1.0.1），更新 Manifest、构建 Release 并发布喵喵商店；摘要相同时不递增版本。运行镜像固定到已验证的摘要，防止同一包版本因 latest 漂移而运行不同镜像。

## 使用

要求懒猫微服 1.5.0 或更新版本，目标架构 amd64。默认管理员 `admin`，密码 `123456`。首次登录请在账号安全设置修改密码，再分享应用地址。

保留 MyIcon 自身鉴权和游客只读模式，游客开关由管理员控制。允许外部访问应用及图标链接，便于其他 NAS 面板使用图标。保留手动登录，不添加文件选择器。

- `/lzcapp/var/data` 对应 `/app/public/data`：图标、壁纸、站点设置和缩略图。
- `/lzcapp/var/auth` 对应 `/app/server/data`：账号哈希与登录令牌，不在 Web 公共目录中。

镜像入口自动为首次启动的空目录填充内置图标及认证配置；不会覆盖已有目录。备份与迁移请同时保留两个目录。内部端口 3000 由懒猫入口代理，不占用宿主 3000 端口。

## 构建与发布

```sh
mkdir -p dist
lzc-cli project release -o dist/application.lpk
lzc-cli lpk info dist/application.lpk
```

仅发布喵喵商店，官方商店关闭。镜像模式使用 `ghcr.1ms.run`，首次发布前已比对该提交 amd64 镜像与上游摘要。

工作流每日或手动触发，使用镜像摘要比较和 patch 递增策略。组织 Secrets 为 `APPSTORE_URL`、`APPSTORE_TOKEN` 和可选的 `PRIVATE_STORE_GROUP_CODES`。喵喵商店引用 `community.lazycat.app.myicon-v<version>.lpk` GitHub Release 文件及其 SHA256。商店发布更新不等于强制升级已安装的应用，微服上的自动升级取决于用户设置。

图标由用户提供。构建和发布检查不替代微服上的登录、上传、外链及持久化实测。
