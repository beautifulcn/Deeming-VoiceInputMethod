<p align="center">
  <img src="app.svg" width="128" height="128" alt="谛明 Deeming">
</p>

<h1 align="center">谛明 Deeming</h1>

<p align="center">谛听所思，明达所向。Deem Your Thoughts, Drive Your Way.</p>

谛明是面向 Windows 7 及以上 x64 系统的本地优先智能语音输入工具。程序常驻系统托盘，按住快捷键即可录音，松开后在本机完成语音识别，并把结果安全地送回录音开始时绑定的输入位置。

当前稳定版本：`v2.0.0`

## 产品优势

- **本地优先，隐私边界清晰**：语音识别在本机运行，不包含遥测，也不会自动上传录音、识别文本或本地日志。模型下载和第三方 LLM 均由用户主动配置或触发。
- **原生轻量，兼容范围广**：采用 C++ 与 Win32 原生实现，无需 Electron、浏览器内核或额外托管运行时；支持 Windows 7 及以上 x64 系统。
- **离线与流式兼得**：支持 FireRedASR2 CTC/AED 离线识别，也支持 Streaming Paraformer 边录音边在本机解码。
- **输入目标安全绑定**：录音开始时绑定窗口和输入位置，提交请求与投递文本前重新校验目标；焦点变化时取消投递，不会把内容转发到新的前台窗口。
- **适应复杂输入环境**：针对原生编辑器、Chromium/Electron、Word 和微信等不同控件采用相应的文本投递与目标复验策略。
- **稳定的音频链路**：优先使用 WASAPI Shared Mode，自动转换为 16 kHz、16-bit、单声道 PCM；初始化失败时回退到 `waveIn`。
- **灵活的文本处理**：可以保持 ASR 原文、使用本地 CT-Transformer 自动标点，或显式启用第三方 LLM 进行纠错、润色和有界上下文感应。
- **内置模型管理**：模型下载器支持断点续传、SHA-256 校验和原子安装，模型按需下载，不随安装包强制分发。

## 核心功能

- 默认长按 `CapsLock` 超过 300 ms 开始录音，松开后识别；短按仍保留系统大小写切换行为。
- FireRed VAD 只裁剪录音首尾静音，保留自然的句中停顿。
- 托盘图标和 Direct2D/DirectWrite HUD 显示“聆听中...”“识别中...”和可选的“润色中...”状态。
- 在线流式会话失败时，可以使用已安装的正式离线模型回退识别。
- LLM 纠错与上下文感应默认关闭，失败时回退到 ASR 原文。
- API Key 使用 Windows DPAPI 加密后保存在当前 Windows 用户配置中。

## 工作流程

1. **绑定输入位置**：按下录音快捷键时记录当前窗口、原生焦点和必要的输入位置身份。
2. **聆听中**：后台采集音频，并按配置执行本地 VAD 或流式 ASR；录音期间不会注入中间结果。
3. **识别中**：松开快捷键后停止采集，完成本地识别和可选的首尾静音裁剪。
4. **润色中**：只有显式选择 LLM 输出处理时，才会向用户配置的第三方 Provider 发送请求。
5. **投递结果**：重新验证会话、窗口和输入焦点，验证通过后才把文本送回原输入位置。

## 下载

[下载 Deeming-Setup-v2.0.0-x64.exe](https://github.com/beautifulcn/Deeming-VoiceInputMethod/releases/download/v2.0.0/Deeming-Setup-v2.0.0-x64.exe)

[查看全部版本](https://github.com/beautifulcn/Deeming-VoiceInputMethod/releases)

| 项目 | 值 |
| --- | --- |
| 文件名 | `Deeming-Setup-v2.0.0-x64.exe` |
| 文件大小 | 7,244,404 字节 |
| SHA-256 | `77A8F142C0FA391886D05DEA0B00417CBFAC7415523419A3F0A0820A1BD5341F` |
| 产品版本 | `2.0.0` |
| 文件版本 | `2.0.0.0` |
| 支持系统 | Windows 7 或更高版本，x64 |

也可以下载同一版本的 [`SHA256SUMS.txt`](https://github.com/beautifulcn/Deeming-VoiceInputMethod/releases/download/v2.0.0/SHA256SUMS.txt) 后校验安装包：

```powershell
(Get-FileHash .\Deeming-Setup-v2.0.0-x64.exe -Algorithm SHA256).Hash
```

输出应与上表中的 SHA-256 完全一致。当前安装程序未使用代码签名证书，Windows 可能显示 SmartScreen 提示；运行前请确认下载地址并核对校验值。

## 系统要求

- Windows 7 或更高版本，x64。
- 可用的麦克风输入设备。
- 安装程序需要管理员权限，默认安装到 64 位 `Program Files\Deeming`。
- 安装包约 6.9 MiB，不包含 ASR、VAD 或标点模型；准备完整模型组合时建议预留约 6 GB 可用磁盘空间。
- 首次下载模型需要网络连接；模型安装完成后，本地语音识别可以离线使用。
- 第三方 LLM 文本处理需要用户自行准备服务地址、模型和 API Key，并可能产生第三方费用。

## 安装与首次使用

1. 下载并校验 `Deeming-Setup-v2.0.0-x64.exe`。
2. 运行安装程序，阅读安全与隐私说明及软件最终用户许可协议；接受协议后选择安装目录。
3. 首次启动时选择模型保存目录。模型目录必须位于程序安装目录之外。
4. 右键托盘图标打开“设置”，在“模型管理”页下载所需模型。
5. 在“语音识别”页选择 ASR 模型、VAD 和输出处理方式，然后点击“保存”。
6. 在任意可输入窗口按住快捷键说话，松开后等待 HUD 完成识别和可选润色。

## 推荐组合

| 使用场景 | ASR | VAD | 输出处理 |
| --- | --- | --- | --- |
| 日常离线输入 | FireRedASR2 CTC int8 | FireRed VAD | 本地标点或保持原文 |
| 优先识别准确率 | FireRedASR2 AED int8 | FireRed VAD | 本地标点或保持原文 |
| 边说边识别 | Streaming Paraformer bilingual zh-en | 按需启用 | 本地标点或保持原文 |
| 智能纠错与润色 | 任一已安装 ASR | 建议启用 | 大模型润色，可选上下文感应 |

更详细的参数选择见[谛明最优配置指南](doc/deeming_optimal_configuration_guide.md)。

## 设置页面

| 页面 | 内容 |
| --- | --- |
| 常规设置 | 录制快捷键 |
| 语音识别 | 模型根目录、识别模型、输出处理、VAD 和线程等高级设置 |
| 模型管理 | 模型选择、下载目录、进度、速度、阶段详情与取消操作 |
| 智能润色 | Provider、API 地址、API Key、模型、连接测试、审计日志和上下文感应 |
| 提示指令 | 基础修复、深度修复、润色、感应修饰及自定义系统提示词 |
| 关于谛明 | 产品详情、版本号、版权信息和功能特性 |

## 本地模型

| 模型 | 用途 | 主要文件 |
| --- | --- | --- |
| FireRedASR2 CTC | 日常本地离线 ASR | `model.int8.onnx`、`tokens.txt` |
| FireRedASR2 AED | 本地离线 ASR | `encoder.int8.onnx`、`decoder.int8.onnx`、`tokens.txt` |
| Streaming Paraformer | 本地流式 ASR | `encoder.int8.onnx`、`decoder.int8.onnx`、`tokens.txt` |
| CT-Transformer | 本地自动标点 | `model.int8.onnx` |
| FireRed VAD | 人声检测 | `fireredvad_stream_vad_with_cache.onnx` |

## LLM 与上下文

LLM 纠错默认不启用。启用时，在“智能润色”页配置 Provider、API 地址、API Key 和模型，再到“语音识别”页把输出处理设为“大模型润色”。内置 Provider 包含 OpenRouter、Mistral 和 Gemini，也可以配置兼容服务地址。

上下文感应同样需要用户显式开启。程序只读取热键按下时绑定窗口内的有界文本，拒绝密码控件；输入位置无法验证时不会提交相应上下文，目标发生变化时取消结果投递。

完整 LLM 审计日志默认关闭。只有显式开启后，程序才会在本地记录有界上下文、口述文本、最终消息和请求正文；日志不记录 Authorization 或 API Key，但仍可能包含敏感信息。

## 配置与本地数据

- 配置：`%APPDATA%\Deeming\config.json`
- LLM 审计日志：`%APPDATA%\Deeming\log\llm_refine_YYYYMMDD.log`
- 模型：首次启动时由用户选择的外部目录

## 升级与卸载

安装新版本前先退出正在运行的谛明，再运行新版安装程序。安装程序使用固定产品标识，可以在现有安装上升级。

可以通过 Windows“程序和功能”卸载。卸载器默认保留 `%APPDATA%\Deeming` 中的配置和本地日志；交互式卸载时可勾选“同时删除当前用户的本地数据”。用户下载到自选目录的模型始终保留，需要时请在确认路径后手动删除。

## 已知边界

- 最终文本在松开快捷键后投递。流式模型会在录音期间后台解码，但不会显示或注入中间结果。
- 单次录音最多保留约 5 分钟 PCM。
- 文本注入可能被更高权限的目标窗口拦截。
- UI Automation 能力由目标应用提供；上下文不可验证时按无上下文路径处理。

## 安全、隐私与许可

谛明不包含遥测，也不提供收集个人隐私的在线服务。语音识别使用本地模型；网络仅用于用户主动下载模型，或用户自行配置并显式启用第三方 LLM 文本处理。完整边界见[安全与隐私说明](PRIVACY.txt)。

安装或使用软件前请阅读[软件最终用户许可协议](LICENSE.txt)。当前许可仅面向自然人最终用户的个人内部使用，不授权企业或机构部署、共享使用、再分发或对外提供服务。

第三方组件和模型仍由各自权利人所有并适用各自许可证，详见[第三方软件与模型声明](THIRD_PARTY_NOTICES.txt)和 [`licenses`](licenses) 目录。

## 主要依赖

- [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx)
- [FireRedASR](https://github.com/FireRedTeam/FireRedASR)
- [FireRedVAD](https://github.com/FireRedTeam/FireRedVAD)
- [ONNX Runtime](https://github.com/microsoft/onnxruntime)
