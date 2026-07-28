# 谛明 Deeming

> 谛听所思，明达所向。Deem Your Thoughts, Drive Your Way.

本仓库是谛明 Deeming 的官方软件包发布仓库，提供 Windows x64 安装程序、版本说明和文件校验值。项目源码与开发文档由主项目仓库维护，不在此仓库重复分发。

当前稳定版本：`v2.0.0`

## 下载

[下载 Deeming-Setup-v2.0.0-x64.exe](https://github.com/beautifulcn/Deeming-VoiceInputMethod/releases/download/v2.0.0/Deeming-Setup-v2.0.0-x64.exe)

[查看全部版本](https://github.com/beautifulcn/Deeming-VoiceInputMethod/releases)

| 项目 | 值 |
| --- | --- |
| 文件名 | `Deeming-Setup-v2.0.0-x64.exe` |
| 文件大小 | 7,244,404 字节 |
| SHA-256 | `77A8F142C0FA391886D05DEA0B00417CBFAC7415523419A3F0A0820A1BD5341F` |
| 支持系统 | Windows 7 或更高版本，x64 |
| 产品版本 | `2.0.0` |
| 文件版本 | `2.0.0.0` |

也可以下载同一 Release 中的 [`SHA256SUMS.txt`](https://github.com/beautifulcn/Deeming-VoiceInputMethod/releases/download/v2.0.0/SHA256SUMS.txt) 后校验安装包：

```powershell
(Get-FileHash .\Deeming-Setup-v2.0.0-x64.exe -Algorithm SHA256).Hash
```

输出应与上表中的 SHA-256 完全一致。当前安装程序未使用代码签名证书，Windows 可能显示 SmartScreen 提示；运行前请确认下载地址属于本仓库并核对校验值。

## 安装与首次使用

1. 运行 `Deeming-Setup-v2.0.0-x64.exe`。安装程序需要管理员权限，默认安装到 64 位 `Program Files\Deeming`，也可以在向导中选择其他目录。
2. 阅读安全与隐私说明及软件最终用户许可协议；接受协议后继续安装。
3. 首次启动时选择模型保存目录。模型目录不能位于程序安装目录内。
4. 右键托盘图标打开“设置”，在“模型管理”页下载所需的 ASR 模型；需要人声检测或本地自动标点时，再下载 FireRed VAD 或 CT-Transformer 标点模型。
5. 在“语音识别”页选择模型和输出处理方式并保存。默认长按 `CapsLock` 超过 300 ms 开始录音，松开后识别并把文本送到录音开始时绑定的输入位置；短按仍切换 Caps Lock 状态。

安装包不包含 ASR、VAD 或标点模型。准备完整模型组合时建议预留约 6 GB 可用磁盘空间。

## 升级与卸载

安装新版本前先退出正在运行的谛明，再运行新版安装程序。安装程序使用固定产品标识，可在现有安装上执行升级。

可以通过 Windows“程序和功能”卸载。卸载器默认保留 `%APPDATA%\Deeming` 中的配置和本地日志；交互式卸载时可勾选“同时删除当前用户的本地数据”。用户下载到自选目录的模型始终保留，需要时请在确认路径后手动删除。

## 主要功能

- FireRedASR2 离线识别与 Streaming Paraformer 本地流式识别。
- WASAPI 音频采集，失败时回退到 `waveIn`。
- 可选 FireRed VAD、本地 CT-Transformer 标点和第三方 LLM 文本处理。
- 可选的有界焦点上下文感应，并在请求与文本投递前复验输入目标。
- 原生模型下载器，支持断点续传、SHA-256 校验和原子安装。
- 托盘常驻及 Direct2D/DirectWrite 状态 HUD。

LLM 纠错与上下文感应默认关闭。在线流式识别的“在线”表示边录音边在本机解码，不是云端语音识别。

## 安全、隐私与许可

谛明不包含遥测，也不提供收集个人隐私的在线服务。语音识别使用本地模型；网络仅用于用户主动下载模型，或用户自行配置并显式启用第三方 LLM 文本处理。完整边界见[安全与隐私说明](PRIVACY.txt)。

安装或使用软件前请阅读[软件最终用户许可协议](LICENSE.txt)。当前许可仅面向自然人最终用户的个人内部使用，不授权企业或机构部署、共享使用、再分发或对外提供服务。

第三方组件和模型仍由各自权利人所有，相关声明与许可证随安装程序一并提供。

## 发布来源

`v2.0.0` 安装包由主项目 `beautifulcn/Deeming` 的提交 `751483f` 通过唯一发布入口 `build.bat installer` 生成。发布构建已通过依赖方向检查、Release 二进制审计和安装器 payload 完整性检查。
