# TikToh

鸿蒙（HarmonyOS NEXT）短视频客户端。本仓库**仅用于分发安装包与更新日志**，不提供源码。

---

## 📢 重要声明

- **本项目仅用于学习和研究目的，不得用于任何商业用途。**
- TikToh 是一款**非官方的第三方视频浏览器**，仅提供浏览视频的能力，**不提供也不分发任何视频内容**；视频内容及账号服务均由对应平台提供，相关名称、商标及内容的权利归各自权利人所有。
- 本软件与字节跳动、TikTok、抖音及其关联公司**无任何隶属、授权、合作或其他关联关系**，不代表上述平台；抖音版为非官方第三方客户端。
- 本软件**不旨在替代官方客户端，也没有能力替代**官方客户端。功能通过抖音与 TikTok Web 端公开 API 实现，包括视频浏览、用户主页查看、评论阅读、消息通知等**只读**功能。

---

## ⚠️ 下载前必读：我们只提供未签名包

Release 里的唯一资产是：

```
TikToh-<版本>-unsigned.app
```

这是 HarmonyOS 的 **App Pack（未签名）**，不是 `.hap`，也不能直接双击安装。
原因很简单：签名包必然绑定签名者的开发者证书，别人拿到也装不上；
而未签名包让你用自己的证书签名，装进你自己的设备生态里。

**本仓库永远不会上传：**

| 类型 | 说明 |
| --- | --- |
| `*.hap` | 单模块产物，无论是否签名都不发 |
| `*-signed.app` | 签名后的 App Pack，不公开发布 |
| `*.p12 / *.cer / *.p7b` | 证书与私钥材料，绝不入库 |

---

## 怎么安装（自己签名）

### 方式一：DevEco Studio 自动签名（推荐，最省事）

1. 用 **DevEco Studio 5.0 / 6.0+** 新建一个空工程（任意包名）。
2. 打开 `File > Project Structure > Signing Configs`，勾选 **Automatically generate signature**，
   登录华为开发者账号，等它生成 `xxx.p12 / xxx.cer / xxx.p7b`。
3. 到 `File > Project Structure > Signing Configs` 右侧可以看到证书路径，
   一般是 `C:\Users\<你>\.ohos\config\default_<工程名>_<hash>.p12`。
4. 用 `hap-sign-tool` 或直接改一个空工程，把本项目的 `.app` 解开、签名后安装。

> 更快的土办法：把 Release 里的 `.app` 后缀改成 `.zip` 解压，得到 `entry-default.hap`；
> 在你的空工程里用同一套证书给它签名：
> ```
> java -jar hap-sign-tool.jar sign-app \
>   -mode localSign -keyAlias "<你的别名>" \
>   -keyPwd "<密码>" -keystorePwd "<密码>" \
>   -appCertFile "<xxx.cer>" -profileFile "<xxx.p7b>" \
>   -inFile entry-default.hap -outFile entry-signed.hap \
>   -signAlg SHA256withECDSA -keystoreFile "<xxx.p12>"
> hdc install entry-signed.hap
> ```

### 方式二：命令行签名 + 安装

```bash
# 1. 解包
mv TikToh-0.1.0-unsigned.app TikToh-0.1.0.zip && unzip TikToh-0.1.0.zip
# 得到 entry-default.hap

# 2. 用你自己的证书签名（见上面的 hap-sign-tool 命令）

# 3. 安装到真机
hdc list targets
hdc install entry-signed.hap
```

设备需要开启 **开发者模式 + USB 调试**，并且签名证书里包含该设备的 UDID
（自动签名会自动带上你登录账号下的设备）。

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
- 资产只有一个 `TikToh-<版本>-unsigned.app`。
