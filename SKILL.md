---
name: Peeches 实时系统音频转录
description: |
  帮助用户安装和使用 Peeches——基于 whisper.cpp 的实时系统音频转录与翻译桌面应用，
  支持歌词式字幕叠加、英中翻译，全程本地离线运行。
---

# Peeches 实时系统音频转录

帮助用户安装、配置和使用
[Peeches](https://github.com/Leeeon233/peeches)——
一款基于 Rust + Tauri 构建的桌面端实时系统音频转录与翻译工具（159 stars，MIT 许可）。

**来源**

- Skill URL: https://github.com/Leeeon233/peeches
- Source URL: https://www.v2ex.com/t/1109747

## 定位

指导用户完成 Peeches 从安装到日常使用的全流程，包括实时系统音频捕获、
Whisper 模型转录、英中翻译配置和歌词式字幕显示。

## 用户输入

用户可能提供：

- **操作系统信息**——macOS 或 Windows 版本、芯片型号
- **使用场景描述**——看英文视频/直播、在线会议、播客收听、学习外语
- **配置截图或日志**——排查音频捕获失败、模型加载错误、翻译不准等问题
- **Whisper 模型偏好**——对速度/精度的不同需求

## 工作流程

### 1. 安装部署

引导用户完成环境准备：

- 从 [Releases](https://github.com/Leeeon233/peeches/releases) 下载最新版（v0.3.0）
- macOS：安装 .dmg，首次运行需在系统偏好设置中允许
- Windows：安装 .msi 或解压便携版
- 首次启动自动下载 Whisper 模型和翻译模型（whisper + opus-mt-en-zh）
- 模型由 Hugging Face Candle 框架加载，纯 Rust 推理，无需 Python 环境

### 2. 系统音频捕获

核心功能配置：

- Peeches 捕获系统级音频输出（非麦克风），可转录任何正在播放的音频
- macOS 需要授予屏幕录制/音频录制权限
- Windows 通过 WASAPI loopback 捕获系统音频
- 支持选择特定音频输出设备

### 3. 实时转录

- 启动后自动开始监听系统音频
- 使用 whisper.cpp（通过 whisper-rs 绑定）进行实时语音识别
- 转录结果以歌词式滚动字幕显示在屏幕叠加层上
- 支持多种 Whisper 模型大小（tiny/base/small/medium/large）

| 模型 | 大小 | 速度 | 精度 | 推荐场景 |
|------|------|------|------|---------|
| tiny | 75MB | 最快 | 基础 | 快速预览、低配设备 |
| base | 142MB | 快 | 较好 | 日常使用 |
| small | 466MB | 中 | 好 | 准确度优先 |
| medium | 1.5GB | 慢 | 很好 | 专业场景 |

### 4. 英中翻译

- 内置 opus-mt-en-zh 翻译模型（Helsinki-NLP）
- 转录英文文本后自动翻译为中文
- 翻译结果与原文同步显示在字幕叠加层
- 当前仅支持英 → 中翻译方向

### 5. 字幕显示

- 歌词式（lyrics-style）滚动显示，当前句高亮
- 半透明悬浮窗口，可叠加在任何应用上方
- 支持调整字体大小、窗口位置、透明度
- 双语显示：上方英文原文，下方中文翻译

## 常见问题

- **音频捕获无声**：检查系统权限设置，确认选择了正确的音频输出设备
- **模型下载失败**：检查网络连接，或手动下载模型文件放置到应用数据目录
- **转录延迟高**：尝试使用更小的 Whisper 模型（tiny/base）
- **翻译不准确**：opus-mt 模型适合通用场景，专业术语可能需要上下文辅助理解

## 技术架构

- **前端**：TypeScript + Tauri WebView
- **后端**：Rust，whisper-rs（whisper.cpp 绑定）+ Candle ML 框架
- **语言占比**：Rust 40.5%、TypeScript 33.2%、CSS 13.1%、Python 12.7%
- **平台**：macOS、Windows
