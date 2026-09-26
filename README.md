# LanDeploy

局域网 IPA 分发工具的构建产物。**这里只有编译好的安装包，源码不在此仓库。**

## 下载

在 [Releases](../../releases) 页面按平台取：

| 平台 | 文件 |
|---|---|
| macOS（Apple Silicon / Intel） | `.dmg` |
| Windows x64 | `.exe`（NSIS 安装程序） |
| Linux x64 | `.deb` / `.AppImage` |

## macOS 首次打开

构建产物没有做代码签名与公证，macOS 会拦一次。任选一种方式放行：

```bash
xattr -d com.apple.quarantine /Applications/LanDeploy.app
```

或者在 Finder 里右键 → 打开，再确认一次。

## 这个工具做什么

把用 Ad Hoc 描述文件签名的 iOS 安装包，通过局域网分发给同一 Wi-Fi 下的 iPhone / iPad。

桌面端负责导入安装包、开启本地 HTTPS 服务并出示二维码；测试设备用 Safari 扫码即可安装。首次使用需要在设备上安装并信任一次本地根证书（这一步由 iOS 强制，无法省略）。

安装包需要预先用包含测试设备 UDID 的描述文件签名——本工具不负责签名，也不注册设备。

## 更新方式

产物由 CI 从源码构建后发布到这里，不手动维护。
