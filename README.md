# yisha-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![YiSha Design](https://img.shields.io/badge/YiSha%20Design-1.0.0-1ab394)](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-0f8b72)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-2f4050)](LICENSE)

面向 AI 编码 Agent 的 YiSha 版本化设计系统文档。

`DESIGN.md` 是 Google Stitch 提出的纯文本设计系统文档格式。它把颜色、字体、布局、组件状态、交互和响应式规则整理成 AI Agent 可以直接执行的约束，帮助生成与现有产品一致的界面；它不替代组件 API 或业务开发文档。

本仓库依据一份真实的 YiSha ASP.NET Core MVC Web 前端快照整理。规范覆盖该快照中的 50 个 Razor 页面、共享布局、自有 CSS/JavaScript、页面内样式和已接入的第三方前端组件。

## 支持版本

| 设计规范版本 | 英文标准版 | 简体中文版 | GitHub Release |
| --- | --- | --- | --- |
| `1.0.0` | [DESIGN.md](versions/1.0.0/DESIGN.md) | [DESIGN.zh-CN.md](versions/1.0.0/DESIGN.zh-CN.md) | [v1.0.0](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0) |

`1.0.0` 是本设计规范自身的版本号，不代表 YiSha 上游项目的正式版本。英文 `DESIGN.md` 是默认的生态兼容版本；中文版本使用相同章节、令牌和规则。

## 使用方法

1. 选择与项目采用的设计规范版本一致的目录。
2. 将英文版或中文版下载到项目根目录并命名为 `DESIGN.md`。
3. 要求 AI 编码 Agent 在生成或修改界面前读取该文件。
4. 保留项目既有的 Bootstrap 3、jQuery 和 YiSha 页面约定，不混入另一套视觉语言。

使用 Windows PowerShell 下载英文标准版：

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/yisha-design-md/main/versions/1.0.0/DESIGN.md" `
  -OutFile ".\DESIGN.md"
```

下载简体中文版：

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/yisha-design-md/main/versions/1.0.0/DESIGN.zh-CN.md" `
  -OutFile ".\DESIGN.md"
```

示例提示词：

```text
先读取项目根目录的 DESIGN.md。使用其中的 YiSha 设计令牌、后台壳层、信息密度、组件状态和响应式规则实现这个管理页面。复用既有 Bootstrap 3、jQuery 与 ys.* 交互约定，不引入新的 UI 设计体系。
```

## 规范覆盖

- 默认蓝色顶栏、深色侧栏、可折叠菜单和 iframe 多页签后台壳层
- 蓝、绿、紫、红、黄五种皮肤，以及深色和浅色侧栏组合
- 登录页、运营看板、列表页、表单页、详情页和权限树页面
- 查询区、工具栏、按钮、表格、树表、分页、表单、校验和选择控件
- 弹层、日期、上传、头像裁剪、图表、加载和消息反馈
- 响应式行为、无障碍改进边界、Do / Don't 规则和 Agent 提示词

## 技术基线

该快照以 Bootstrap `3.3.7`、jQuery `2.1.4` 和 Font Awesome `4.7.0` 为基础，并接入 Layer `3.1.1`、Laydate `5.0.9`、Bootstrap Table `1.12.0`、Select2 `4.0.6`、iCheck `1.0.2`、zTree v3 等组件。具体角色和使用边界以版本目录中的 DESIGN.md 为准。

YiSha 上游项目可见于 [liukuo362573/YiShaAdmin](https://github.com/liukuo362573/YiShaAdmin)。本仓库是独立整理的设计系统文档，不是 YiSha 官方设计文档，也不受其官方背书。

## 许可证

本仓库原创文档采用 [MIT License](LICENSE)。YiSha 及第三方组件仍分别遵循其自身许可证。
