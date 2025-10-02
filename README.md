# 🤖 AI 开发对接文档模板集

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/oloiverrrr/ai-handoff-templates)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/oloiverrrr/ai-handoff-templates/pulls)

> 让 AI 自动生成标准化交接文档，解决跨团队、跨 AI 协作中的信息不对等问题

[English](./README_EN.md) | 简体中文

---

## 📖 项目简介

在使用不同 AI 工具（Claude、ChatGPT、GitHub Copilot 等）进行开发时，经常遇到：
- ❌ 代码交接信息缺失
- ❌ 文档格式不统一
- ❌ 关键信息遗漏
- ❌ 下一个开发者看不懂

**本项目提供三套标准化提示词模板**，让 AI 自动生成完整、规范、可执行的对接文档。

---

## ✨ 特性

- 🎯 **三种场景覆盖**：极简版 / 标准版 / 测试版，适配不同项目规模
- 📋 **标准化输出**：统一的文档结构，团队协作更顺畅
- 🚀 **开箱即用**：复制提示词即可，无需学习成本
- 🔄 **跨 AI 兼容**：支持 Claude、ChatGPT、Gemini、Copilot 等
- ✅ **质量保障**：内置检查清单，确保文档完整性
- 🌍 **多语言示例**：包含 JavaScript、Python、Bash 等代码示例

---

## 📦 模板版本

| 版本 | 字数 | 章节数 | 适用场景 | 填写时间 | 推荐指数 |
|-----|------|-------|---------|---------|---------|
| [极简版](./templates/minimal.md) | 800-1200 | 8 | Bug 修复、小功能、工具脚本 | 5-10 分钟 | ⭐⭐⭐⭐ |
| [标准版](./templates/standard.md) | 2500-4000 | 12 | 大多数开发场景、API 开发 | 15-25 分钟 | ⭐⭐⭐⭐⭐ |
| [测试版](./templates/test.md) | 3000-5000 | 17 | 复杂项目、架构重构 | 20-30 分钟 | ⭐⭐⭐⭐ |

---

## 🚀 快速开始

### 1. 选择模板

根据你的项目选择合适的版本：

- **Bug 修复 / 配置修改** → [极简版](./templates/minimal.md)
- **正常功能开发** → [标准版](./templates/standard.md) ⭐ 推荐
- **大型重构 / 新系统** → [测试版](./templates/test.md)

### 2. 复制提示词

打开对应模板文件，复制完整的提示词

### 3. 发送给 AI

开发完成后，将提示词发送给 AI：

```
我刚刚完成了用户认证功能的开发，
请使用【标准版对接文档模板】生成完整的对接文档。
```

### 4. 获取文档

AI 会自动生成包含以下内容的标准化文档：
- ✅ 项目概览
- ✅ 技术栈信息
- ✅ 代码变更清单
- ✅ API 接口文档
- ✅ 数据库设计
- ✅ 部署指南
- ✅ 测试说明
- ✅ 已知问题与坑点

---

## 📝 使用示例

### 示例 1：使用标准版模板

**场景**：开发了一个用户认证 API

```bash
# 1. 开发完成后，向 AI 发送：
我完成了用户认证 API 的开发，包括注册、登录、JWT 验证。
请使用【标准版对接文档模板】生成对接文档。

# 2. AI 会生成完整文档，包括：
- API 接口详细说明（curl 示例、响应格式）
- 数据库表结构（users 表设计）
- JWT 配置说明
- 本地部署步骤
- 测试用例
- 已知坑点（如密码加密、失败锁定机制）
```

---

## 📚 文档结构

```
ai-handoff-templates/
├── README.md                   # 项目说明（本文件）
├── LICENSE                     # MIT 协议
├── .gitignore
│
├── templates/                  # 模板文件
│   ├── minimal.md                 # 极简版模板
│   ├── standard.md                # 标准版模板 ⭐
│   └── test.md                    # 测试版模板
│
├── examples/                   # 示例文档
│   ├── example-minimal.md         # 极简版示例
│   └── example-standard.md        # 标准版示例
│
└── docs/                       # 使用指南
    ├── getting-started.md         # 快速开始
    └── best-practices.md          # 最佳实践
```

---

## 🎯 适用场景

### ✅ 推荐使用

- 个人开发项目（切换不同 AI 工具时）
- 团队协作开发（成员之间交接）
- 外包项目交付（甲方需要完整文档）
- 开源项目贡献（PR 附带对接文档）
- 技术债务记录（重构时了解历史）

---

## 💡 最佳实践

### 1. 在开发开始时就告知 AI

```
我要开发一个用户认证功能，
完成后请使用【标准版对接文档模板】生成文档。
```

这样 AI 在开发过程中会有意识地记录关键信息。

### 2. 将文档与代码放在一起

```
project/
├── src/
├── docs/
│   └── handoff/
│       ├── HANDOFF_auth_20251002.md
│       └── HANDOFF_payment_20251001.md
└── README.md
```

### 3. 使用统一命名规范

```
HANDOFF_[模块名]_[日期YYYYMMDD].md
```

---

## 🤝 贡献

我们欢迎所有形式的贡献！

- 🐛 报告 Bug
- 💡 提出建议
- 📝 改进文档
- 🎨 提交模板

---

## 📄 许可证

本项目采用 [MIT 许可证](./LICENSE)

---

## 🔗 相关链接

- **GitHub 仓库**: https://github.com/oloiverrrr/ai-handoff-templates
- **问题反馈**: https://github.com/oloiverrrr/ai-handoff-templates/issues

---

## ❓ 常见问题

### Q1: 哪个版本最适合我？

**A**: 90% 的场景使用【标准版】就够了。只有在快速修复或大型重构时才需要极简版/测试版。

### Q2: 可以混合使用吗？

**A**: 可以！例如主要功能用标准版，小 Bug 用极简版。

### Q3: 支持哪些 AI？

**A**: Claude、ChatGPT、Gemini、GitHub Copilot、通义千问、文心一言等主流 AI 都支持。

---

<p align="center">
  Made with ❤️ by <a href="https://github.com/oloiverrrr">oloiverrrr</a>
</p>

<p align="center">
  <sub>如果这个项目帮到了你，别忘了点个 ⭐️ 哦！</sub>
</p>