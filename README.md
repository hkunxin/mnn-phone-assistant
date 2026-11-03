# mnn-phone-assistant

手机端侧个人助理项目（目标设备：天玑 9300+，arm64-v8a）。

## 路线

本机不安装 Android 编译环境（NDK / Android SDK），全部编译在 GitHub Actions 云端完成：

1. 工作流 `.github/workflows/build-apk.yml` 在 ubuntu runner 上：
   - 安装 NDK 27.2 + CMake 3.22.1
   - 浅克隆 alibaba/MNN，编译 arm64-v8a 核心库（CPU + OpenCL + ARM82，关闭 QNN/Hexagon —— 联发科芯片用不到）
   - 用 MNN 源码树内的 MnnLlmChat 工程打出 `app-standard-debug.apk`
   - 上传为 Artifact `mnn-chat-debug-apk`
2. MNN 核心库构建结果按 MNN commit SHA 缓存，MNN 未更新时二次构建只需跑 gradle 打包。
3. 下载 APK 安装到手机后，在 App 内「模型市场」下载 Qwen3-0.6B-MNN（约 433MB，4bit）即可离线对话。

## 后续

- 模型转换（llmexport Qwen3-0.6B → MNN 格式）也可迁入 CI
- 基于该工程二开个人助理功能（自定义 System Prompt / 界面 / 集成）
