---
title: app备案时安卓ios端获取应用公钥和签名方法
date: 2026-09-07 10:00:00
tags:
---

自 2023 年起，APP 也需要进行 ICP 备案。提交备案时，平台会要求填写 APP 的**特征信息**，主要包括：

| 平台 | 包名 / Bundle ID | 平台公钥 | 签名指纹 |
|------|------------------|----------|----------|
| Android | 软件包名（如 com.example.app） | 以 10 进制或 16 进制填写 | 证书 **MD5** 值（16 进制） |
| iOS | Bundle ID | 以 10 进制或 16 进制填写 | 证书 **SHA-1** 值（16 进制） |

> 注意：提取信息时使用的签名证书，必须与最终上架/发布 APK 或 IPA 时使用的证书**完全一致**，否则备案审核无法通过。

---

## 一、Android 端

### 需要获取的三项信息

1. **软件包名**：在 `AndroidManifest.xml` 的 `package` 属性，或 Gradle 的 `applicationId`
2. **平台公钥**：签名证书中的 RSA 公钥
3. **签名 MD5 值**：签名证书的 MD5 指纹

### 方法一：使用 keytool（推荐，有 keystore 文件时）

确保本机已安装 JDK，命令行可用 `keytool`。

**1. 查看 keystore 基本信息（含 MD5，部分高版本 JDK 可能不显示 MD5）**

```bash
keytool -list -v -keystore your-release.keystore -alias your-alias
```

输入 keystore 密码后，输出中包含：

- `MD5`：即备案所需的签名 MD5 值
- `SHA1`、`SHA256`：其他指纹信息

**2. 高版本 JDK 不显示 MD5 时，用 openssl 计算**

```bash
keytool -exportcert -alias your-alias -keystore your-release.keystore | openssl dgst -md5
```

输出示例：`MD5(stdin)= cde9f6208d672b54b1dacc0b7029f5eb`，去掉冒号后即为 16 进制 MD5。

**3. 导出公钥**

```bash
keytool -exportcert -alias your-alias -keystore your-release.keystore -file cert.cer
openssl x509 -inform DER -in cert.cer -pubkey -noout
```

将输出的公钥内容（去掉 `-----BEGIN PUBLIC KEY-----` 等头尾，或按备案平台要求填写 16 进制）提交即可。

### 方法二：从 APK 包获取（没有 keystore，只有已签名 APK 时）

**方式 A：keytool 直接读取 APK**

```bash
keytool -printcert -jarfile app-release.apk
```

**方式 B：解压 APK + openssl（适用于 keytool 无 MD5 的情况）**

APK 本质是 zip 文件，解压后在 `META-INF/` 目录下找到 `CERT.RSA`（或 `CERT.RSA` / `*.RSA`）：

```bash
# 1. 解压 apk，进入 META-INF 目录
# 2. 从 CERT.RSA 提取证书
openssl pkcs7 -inform DER -in CERT.RSA -print_certs -out CERT.cert

# 3. 查看 MD5 指纹（备案填这个，16 进制、去掉冒号）
openssl x509 -in CERT.cert -fingerprint -noout -md5

# 4. 查看公钥
openssl x509 -in CERT.cert -pubkey -noout
```

小写无冒号格式：

```bash
openssl x509 -in CERT.cert -outform DER | openssl dgst -md5
```

### 方法三：使用 jadx-gui（图形界面，适合不熟悉命令行的场景）

1. 下载 [jadx](https://github.com/skylot/jadx/releases)，运行 `jadx-gui`
2. 打开已签名的 APK 文件
3. 点击 `AndroidManifest.xml` 查看**包名**
4. 点击左侧 **APK signature**，可直接查看**平台公钥**和**签名 MD5 值**

macOS 可通过 Homebrew 安装：`brew install jadx`，然后运行 `jadx-gui`。

### Android Studio 查看包名

在 `app/build.gradle` 中查看 `applicationId`，即为备案所需的软件包名。

---

## 二、iOS 端

iOS 备案需要：**Bundle ID**、**平台公钥**、**签名 SHA-1 值**（注意：iOS 填 SHA-1，不是 MD5）。

### 方法一：Apple Developer 后台（官方推荐）

1. 使用 APP 对应的 Apple 开发者账号登录 [Apple Developer](https://developer.apple.com/account)
2. 进入 **Certificates, Identifiers & Profiles**
3. **Bundle ID**：点击 **Identifiers**，`IDENTIFIER` 列即为 Bundle ID
4. **公钥与 SHA-1**：点击 **Certificates**，找到 APP 发布使用的**分发证书**（Distribution），下载 `.cer` 文件
5. 双击 `.cer` 安装到「钥匙串访问」，右键证书 → **显示简介** → **详细信息**：
   - **公钥**：在详细信息中找到「公钥」相关字段，按备案平台要求填写（若显示不全，先点省略号，仍不全则复制已显示部分）
   - **SHA-1**：即备案要求的「签名 MD5 值」字段（iOS 实际填 SHA-1）

### 方法二：openssl 解析证书文件

下载 `.cer` 后，可用命令行查看：

```bash
# 查看 SHA-1 指纹
openssl x509 -in ios_distribution.cer -inform DER -fingerprint -noout -sha1

# 查看公钥
openssl x509 -in ios_distribution.cer -inform DER -pubkey -noout
```

### 方法三：从 IPA 获取（仅有打包好的 IPA 时）

IPA 也是 zip 格式，解压后在 `Payload/xxx.app/` 内可找到 embedded.mobileprovision 等信息；但更可靠的方式仍是使用 Developer 后台下载与打包一致的**分发证书**。

---

## 三、常见问题

**1. keytool 看不到 MD5？**

Java 8 以后部分版本的 `keytool -list -v` 不再显示 MD5，请改用 `openssl dgst -md5` 或从 APK 的 `CERT.RSA` 提取。

**2. 公钥格式怎么填？**

不同备案平台要求略有差异，常见为去掉 PEM 头尾的 Base64，或填写 16 进制公钥模数。以备案平台页面说明为准。

**3. uni-app / DCloud 云打包怎么获取？**

- 使用**自有证书**打包：按上文 keystore 方法提取
- 使用**DCloud 公用证书**：在 [DCloud 开发者中心](https://dev.dcloud.net.cn/) 查看应用详情，部分信息可直接获取；或使用 DCloud 提供的证书文件按 keytool 方法操作

**4. 调试证书和发布证书不一致？**

备案必须使用**正式发布**时所用的签名证书，debug 证书无效。

---

## 参考

- [腾讯云 ICP 备案 APP 特征信息填写参考规范](https://cloud.tencent.com/document/product/243/97789)
- [DCloud APP 备案公钥、签名 MD5 获取方法](https://ask.dcloud.net.cn/article/40734)
