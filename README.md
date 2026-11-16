# TikToh

鸿蒙（HarmonyOS NEXT）短视频客户端。本仓库**仅用于分发安装包与更新日志**，不提供源码。

---

## 📢 重要声明

- **本项目仅用于学习和研究目的，不得用于任何商业用途。**
- TikToh 是一款**非官方的第三方视频浏览器**，仅提供浏览视频的能力，**不提供也不分发任何视频内容**；视频内容及账号服务均由对应平台提供，相关名称、商标及内容的权利归各自权利人所有。
- 本软件与字节跳动、TikTok、抖音及其关联公司**无任何隶属、授权、合作或其他关联关系**，不代表上述平台；抖音版为非官方第三方客户端。
- 本软件**不旨在替代官方客户端，也没有能力替代**官方客户端。功能通过抖音与 TikTok Web 端公开 API 实现，包括视频浏览、用户主页查看、评论阅读、消息通知等**只读**功能。

---

## 📦 下载前须知：默认提供未签名包

Release 里目前提供两种未签名资产，按习惯挑选：

| 资产 | 是什么 | 适合 |
| --- | --- | --- |
| `TikToh-<版本>.hap` | HarmonyOS **HAP 单模块安装包（未签名）** | 最省事：用手机端「轻启安装器」一键装（见方式一）；也可用 HoKit 可视化签名 |
| `TikToh-<版本>.app.zip` | HarmonyOS **App Pack（未签名）**，解压得到 `TikToh-<版本>.app` | 走 DevEco / 命令行签名流程 |

这不代表「必须自己签」：装好**轻启安装器**后，未签名的 HAP 可以在手机端直接一键安装（方式一）。
其余情况下需要先签名 —— 签名包会绑定签名者的开发者证书，换个人或换台设备往往装不上；
未签名包让你用自己的证书签名，装进自己的设备生态里，责任边界也更清晰。

**目前不随 Release 发布：**

| 类型 | 说明 |
| --- | --- |
| `*-signed.hap` / `*-signed.app` | 签名产物会绑定开发者签名材料，一般不对外分发 |
| `*.p12 / *.cer / *.p7b` | 证书与私钥属于个人敏感材料，不随仓库分发 |

---

## 怎么安装

### 方式一：轻启安装器（最推荐，最省事）

思路：先把「轻启安装器」这个 HAP 装进手机，之后 TikToh 的 HAP 在手机端轻启里**一键安装**，不用自己签名，也不用每次连电脑。

**第 1 步：手机准备（鸿蒙手机）**

设置 → 关于手机，连点「版本号」开启**开发者模式**，再进「系统和更新 → 开发人员选项」打开 **USB 调试**。

**第 2 步：找一台「宿主」设备把轻启装进手机**（三选一，按你手头有什么挑）

| 你的情况 | 用的工具 | 下载 |
| --- | --- | --- |
| 有 Windows 电脑 | 小白调试助手 **3.1.0** | [hap_installer-Windows-3.1.0.zip](https://github.com/likuai2010/auto-installer/releases/tag/3.1.0) |
| 有 macOS 电脑 | 小白调试助手 **3.1.0** | [hap_installer-Mac-3.1.0.zip](https://github.com/likuai2010/auto-installer/releases/tag/3.1.0) |
| **没有电脑，只有一台安卓手机** | 小白调试助手 **2.5.0 Android 版** | [hap_instaler-android-2.5.0.apk](https://github.com/likuai2010/auto-installer/releases/tag/2.5.0) |
| 想要图形化重签名 / 投屏一起用 | HoKit | [HoKit Releases](https://github.com/yabi-zzh/HoKit/releases) |

> 安卓手机这条路完全可行：安卓机装 2.5.0 的 APK，用 **OTG 数据线**直连鸿蒙手机，安卓机就充当调试主机，
> 把轻启的 HAP 推进鸿蒙手机。适合「只有两部手机、没有电脑」的场景。

**第 3 步：装轻启安装器**

1. 下载 [轻启安装器 v0.4.67](https://github.com/thx853068675-dev/QSStore/releases/tag/v0.4.67) 里的 **HAP** 文件。
2. 连接设备：
   - **电脑路线**：数据线连鸿蒙手机，打开小白调试助手（Windows 解压后运行 exe / macOS 打开 app）或 HoKit，等它识别到设备。
   - **安卓手机路线**：安卓机装好 2.5.0 APK 并打开，用 **OTG 线**把安卓机和鸿蒙手机连起来（鸿蒙手机选「传输文件」），在安卓端应用里授权 USB 访问、等到设备出现在列表里。
3. 在工具里选中**轻启安装器的 HAP**，点安装 / 推送：
   - 小白调试助手：连接后在界面里选择 HAP 文件 → 安装。
   - HoKit：左侧「应用重签名」拖入 HAP → 自动拉证书签名 → 安装/推送到设备。
4. 看到「安装成功」后，鸿蒙手机桌面会出现**轻启**图标。

**第 4 步：手机端一键装 TikToh**

1. 在鸿蒙手机上打开**轻启安装器**。
2. 从本仓库 Release 下载 `TikToh-<版本>.hap`（未签名也可以）。
3. 在轻启里选中这个 HAP，点一键安装；之后每次更新版本，同样丢进轻启点一下就完事。

> 首次用「电脑 / 安卓机」装轻启这一步是绕不开的（鸿蒙不允许第三方 HAP 直接在手机里侧载）；
> 装好轻启之后，后续所有 HAP 都能在手机上自助安装，不必再连任何设备。


### 方式二：DevEco Studio 自动签名

1. 用 **DevEco Studio 5.0 / 6.0+** 新建一个空工程（任意包名）。
2. 打开 `File > Project Structure > Signing Configs`，勾选 **Automatically generate signature**，
   登录华为开发者账号，等它生成 `xxx.p12 / xxx.cer / xxx.p7b`。
3. 到 `File > Project Structure > Signing Configs` 右侧可以看到证书路径，
   一般是 `C:\Users\<你>\.ohos\config\default_<工程名>_<hash>.p12`。
4. 用 `hap-sign-tool` 或直接改一个空工程，把本项目的 `.app` 解开、签名后安装。

> 更快的土办法：把 `TikToh-<版本>.app.zip` 解压，得到 `TikToh-<版本>.app`（App Pack），
> 再解一层得到 `entry-default-unsigned.hap`；在你的空工程里用同一套证书给它签名：
> ```
> java -jar hap-sign-tool.jar sign-app \
>   -mode localSign -keyAlias "<你的别名>" \
>   -keyPwd "<密码>" -keystorePwd "<密码>" \
>   -appCertFile "<xxx.cer>" -profileFile "<xxx.p7b>" \
>   -inFile entry-default.hap -outFile entry-signed.hap \
>   -signAlg SHA256withECDSA -keystoreFile "<xxx.p12>"
> hdc install entry-signed.hap
> ```

### 方式三：命令行签名 + 安装

```bash
# 1. 解包：TikToh-<版本>.app.zip 解压得到 .app，再解一层得到 entry-default-unsigned.hap
mv TikToh-0.1.6.app.zip TikToh-0.1.6.zip && unzip TikToh-0.1.6.zip
unzip TikToh-0.1.6.app
# 得到 entry-default-unsigned.hap

# 2. 用你自己的证书签名（见上面的 hap-sign-tool 命令）

# 3. 安装到真机
hdc list targets
hdc install entry-signed.hap
```

设备需要开启 **开发者模式 + USB 调试**，并且签名证书里包含该设备的 UDID
（自动签名会自动带上你登录账号下的设备）。

### 方式四：HoKit 可视化签名

[HoKit](https://github.com/yabi-zzh/HoKit) 是开源的 HarmonyOS 桌面辅助工具（投屏 / 性能监控 / 应用解析 / **可视化重签名**），
重签名功能会**自动拉取你华为开发者账号下的证书与 Profile**，拖一下文件点个按钮就完成，全程不用敲命令。

1. 到 [HoKit Releases](https://github.com/yabi-zzh/HoKit/releases) 下载安装：
   Windows `.exe` / macOS `.dmg` / Linux `.AppImage`（macOS 首次打开若提示「已损坏」，
   终端执行 `sudo xattr -rd com.apple.quarantine /Applications/HoKit.app`）。
2. 在 HoKit 里登录**华为开发者账号**（重签名功能需要；目标设备的 UDID 可在「已注册设备」页一键注册）。
3. 左侧导航 →「应用重签名」，把本仓库 Release 里的 `TikToh-<版本>.hap`
   （或 `TikToh-<版本>.app.zip` 解压得到的 `.app`）拖进上传区。
4. 点「开始重签名」，等日志跑完后点「输出目录」拿到签名包，`hdc install entry-signed.hap` 装进设备。

> 注意：重签名仅用于调试分发；同一应用若有多个包，先签 HAP 主包再签 HSP 共享包。

---

## 应用内更新

客户端启动时会匿名请求本仓库的 latest Release：

```
GET https://api.github.com/repos/DUDUXXR2/tiktoh/releases/latest
```

- 有更高版本 → 弹窗展示 **Release 说明正文** 与下载入口；
- 勾选「不再提示此版本」后，同版本不再打扰，出了新版本仍会提示；
- 网络不通 / API 限流 → 静默失败，不影响使用。

因此**本仓库必须是 public**，private 仓库的匿名请求会一直 401。

---

## 版本记录

见 [Releases](../../releases)。

发版约定：

- tag 用纯版本号 `0.1.1`（客户端会去掉 `v` 前缀再比较）；
- Release 正文写人话的更新说明，它会原样显示在应用内更新弹窗里；
- 资产为 `TikToh-<版本>.hap` 与 `TikToh-<版本>.app.zip`（未签名；`.app` 因 GitHub 扩展名黑名单以 zip 上传，zip 内保留 `.app` 命名）。
