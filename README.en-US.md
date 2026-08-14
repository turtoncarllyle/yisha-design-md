# yisha-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![YiSha Design](https://img.shields.io/badge/YiSha%20Design-1.0.0-1ab394)](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-0f8b72)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-2f4050)](LICENSE)

A versioned YiSha design-system document for AI coding agents.

`DESIGN.md` is a plain-text design-system format introduced by Google Stitch. It turns colors, typography, layout, component states, interactions, and responsive behavior into constraints that an AI agent can follow. It helps generated interfaces remain consistent with the existing product; it does not replace component APIs or application development documentation.

This repository is derived from a real YiSha ASP.NET Core MVC Web frontend snapshot. The specification covers all 50 Razor pages in that snapshot, shared layouts, first-party CSS and JavaScript, page-level styles, and the third-party UI components actually wired into the application.

## Supported Versions

| Design version | Canonical English | Simplified Chinese | GitHub Release |
| --- | --- | --- | --- |
| `1.0.0` | [DESIGN.md](versions/1.0.0/DESIGN.md) | [DESIGN.zh-CN.md](versions/1.0.0/DESIGN.zh-CN.md) | [v1.0.0](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0) |

`1.0.0` versions this design specification itself; it is not an upstream YiSha release number. The English `DESIGN.md` is the canonical ecosystem-compatible edition. The Chinese edition uses the same sections, tokens, and rules.

## Usage

1. Select the directory for the design specification version used by your project.
2. Download either language edition to the project root and name it `DESIGN.md`.
3. Tell your AI coding agent to read the file before generating or changing UI.
4. Preserve the project's Bootstrap 3, jQuery, and YiSha page conventions instead of introducing another visual language.

Download the canonical English edition with Windows PowerShell:

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/yisha-design-md/main/versions/1.0.0/DESIGN.md" `
  -OutFile ".\DESIGN.md"
```

Download the Simplified Chinese edition:

```powershell
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/turtoncarllyle/yisha-design-md/main/versions/1.0.0/DESIGN.zh-CN.md" `
  -OutFile ".\DESIGN.md"
```

Example prompt:

```text
Read DESIGN.md in the project root first. Implement this admin page with its YiSha design tokens, application shell, information density, component states, and responsive rules. Reuse the existing Bootstrap 3, jQuery, and ys.* interaction conventions without introducing another UI design system.
```

## Coverage

- Default blue header, dark sidebar, collapsible navigation, and iframe multi-tab shell
- Blue, green, purple, red, and yellow skins with dark and light sidebar combinations
- Sign-in, operations dashboard, list, form, detail, and permission-tree pages
- Filters, toolbars, buttons, tables, tree tables, pagination, forms, validation, and selection controls
- Layers, dates, uploads, portrait cropping, charts, loading, and feedback
- Responsive behavior, accessibility improvement boundaries, Do / Don't rules, and agent prompts

## Technical Baseline

The snapshot is built on Bootstrap `3.3.7`, jQuery `2.1.4`, and Font Awesome `4.7.0`. It also integrates Layer `3.1.1`, Laydate `5.0.9`, Bootstrap Table `1.12.0`, Select2 `4.0.6`, iCheck `1.0.2`, zTree v3, and other focused plugins. See the versioned DESIGN.md for their exact roles and boundaries.

The upstream YiSha project is available at [liukuo362573/YiShaAdmin](https://github.com/liukuo362573/YiShaAdmin). This repository is an independently maintained design-system document. It is not official YiSha documentation and is not endorsed by the YiSha project.

## License

Original documentation in this repository is released under the [MIT License](LICENSE). YiSha and third-party components remain subject to their respective licenses.
