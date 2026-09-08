# yisha-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![YiSha Design](https://img.shields.io/badge/YiSha%20Design-1.0.0-1ab394)](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-0f8b72)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-2f4050)](LICENSE)

面向 AI 编码 Agent 的 YiSha 版本化设计系统文档。

`DESIGN.md` 是 Google Stitch 提出的纯文本设计系统文档格式。它把颜色、字体、布局、组件状态、交互和响应式规则整理成 AI Agent 可以直接执行的约束，帮助生成与现有产品一致的界面；它不替代组件 API 或业务开发文档。

本仓库依据一份真实的 YiSha ASP.NET Core MVC Web 前端快照整理。静态审计覆盖 50 个 Razor 文件（38 个业务视图及布局、局部视图和上下文文件）、自有 CSS/JavaScript、内联样式、41 个 bundle 定义和已接入的第三方组件。

适用于已有后台页面的生成、扩展和一致性审查，不提供业务源码、预览站或新的组件库，也不代替路由、权限与后端 API 契约。规范明确区分源码事实、依赖行为、新实现补充要求和待验证事项。

## 支持版本

| 设计规范版本 | 英文标准版（当前维护稿） | 简体中文版（当前维护稿） | 首次发布快照 |
| --- | --- | --- | --- |
| `1.0.0` | [DESIGN.md](versions/1.0.0/DESIGN.md) | [DESIGN.zh-CN.md](versions/1.0.0/DESIGN.zh-CN.md) | [v1.0.0](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0) |

`1.0.0` 是本设计规范自身的版本号，不代表 YiSha 上游项目的正式版本。英文 `DESIGN.md` 是标准版；中文使用相同章节、令牌和等价规则。

本次在原 `1.0.0` 目录内补充和纠错，不新增版本号。`main` 上的维护稿包含更新；现有 `v1.0.0` 标签和 Release 附件保持首次发布内容，不随维护稿变化。请用下方下载地址获取当前规范；需要不可变内容时，从文件历史选定提交 SHA，将地址中的 `main` 替换为该 SHA。

[审计报告与覆盖矩阵](versions/1.0.0/AUDIT.md) | [源码 SHA-256 清单](versions/1.0.0/SOURCE-MANIFEST.json) | [文档修改历史](https://github.com/turtoncarllyle/yisha-design-md/commits/main/versions/1.0.0)

## 使用方法

1. 选择与项目采用的设计规范版本一致的目录，并确认使用当前维护稿还是首次发布快照。
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
先完整读取项目根目录的 DESIGN.md 1.0.0。按规范选择宿主与页面模式，保留 YiSha 色板、紧凑密度、实际插件尺寸和响应式规则。复用 Bootstrap 3、jQuery、ys.* 及本地适配器，保留权限 ID、col 映射、数据契约和父级回调。区分源码已有行为与补充要求，明确处理等待、失败恢复、键盘和焦点；未知业务契约先报告，不虚构接口。静态检查不能表述为 UI 验收。
```

## 规范覆盖

- 默认蓝色顶栏、深色侧栏、可折叠菜单和 iframe 多页签后台壳层
- 蓝、绿、紫、红、黄五种皮肤，以及深色和浅色侧栏组合
- 登录、运营看板、服务器监控、Cron 预览、列表、表单、详情和权限树
- 查询区、工具栏、按钮、表格、树表、分页、表单、校验和选择控件
- 弹层、日期、上传、头像裁剪、图表、加载和消息反馈
- 无权限页面、完整断点矩阵、无障碍改进边界、Do / Don't 和 Agent 提示词

## 技术基线

该快照以 Bootstrap `3.3.7`、jQuery `2.1.4` 和 Font Awesome `4.7.0` 为基础，并接入 Layer `3.1.1`、Laydate `5.0.9`、Bootstrap Table `1.12.0`、Select2 `4.0.7`、iCheck `1.0.2`、zTree `3.5.18` 和 ECharts `4.5.0` 等组件。

版本按实际文件声明核对：Select2 目录标为 `4.0.6`，实现为 `4.0.7`；File Input 目录标为 `5.0.3`，实现为 `5.0.4`。这是文档更正，不是升级前端依赖。全部角色、加载顺序和边界见 DESIGN.md 第 2 章。

YiSha 上游项目可见于 [liukuo362573/YiShaAdmin](https://github.com/liukuo362573/YiShaAdmin)。本仓库是独立整理的设计系统文档，不是 YiSha 官方设计文档，也不受其官方背书。

本地源码无法确定对应上游标签，因此以文件哈希记录本次证据，不声明与上游最新版一致。本次仅进行了源码与文档静态检查，未启动业务系统、访问数据库或执行浏览器 UI 验收。

## 许可证

本仓库原创文档采用 [MIT License](LICENSE)，版权主体为 `turtoncarllyle`。YiSha 及第三方组件分别遵循自身许可证；上游图片、字体和图标的授权不由本文档许可证替代。
