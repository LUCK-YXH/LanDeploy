# LanDeploy

局域网 IPA 分发工具的构建产物。**这里只有编译好的安装包，源码不在此仓库。**

官网：<https://luck-yxh.github.io/LanDeploy/>

## 下载

在 [Releases](../../releases) 页面按平台取：

| 平台 | 文件 |
|---|---|
| macOS（Apple Silicon / Intel） | `.dmg` |
| Windows x64 | `.exe`（NSIS 安装程序） |
| Linux x64 | `.deb` / `.AppImage` |

## macOS 首次打开

产物是 **ad-hoc 签名**（没有 Apple 开发者证书链），macOS 会拦一次。拦住之后弹哪句话，取决于**签名本身是否完好**——这两句话处理方式不同：

| 提示 | 右键 → 打开 | 含义 |
|---|---|---|
| **无法验证开发者** | **有用** | 签名完好，只是系统不认识发布者。**0.1.1 及以上是这一种** |
| **已损坏，无法打开** | **没用** | 签名校验通不过。0.1.0 是这一种，请改用 0.1.1+ |

如果你遇到的是第二种，或者想省掉这一步：

```bash
xattr -dr com.apple.quarantine /Applications/LanDeploy.app
```

**先把 App 从 DMG 拖进「应用程序」再执行这条命令**——对着 DMG 里那份执行不会生效。不需要 `sudo`。

> 这条差异是可以客观检验的：0.1.0 的产物 `codesign --verify` 报 `code has no resources but signature indicates they must be present`（`spctl` 退出码 1），0.1.1 报 `valid on disk` / `satisfies its Designated Requirement`（`spctl` 退出码 3）。前者是文件真的坏了，后者只是没有开发者证书。

## 这个工具做什么

把用 Ad Hoc 描述文件签名的 iOS 安装包，通过局域网分发给同一 Wi-Fi 下的 iPhone / iPad。

桌面端负责导入安装包、开启本地 HTTPS 服务并出示二维码；测试设备用 Safari 扫码即可安装。首次使用需要在设备上安装并信任一次本地根证书（这一步由 iOS 强制，无法省略）。

安装包需要预先用包含测试设备 UDID 的描述文件签名——本工具不负责签名，也不注册设备。

## 更新方式

产物由 CI 从源码构建后发布到这里，不手动维护。

> CI 的自动发布需要一个 `RELEASE_TOKEN` secret（跨仓库发布用不了 `GITHUB_TOKEN`）。没配置时工作流会跳过发布步骤，安装包只留在本次运行的 Artifacts 里。
