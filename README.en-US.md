# yisha-design-md

[简体中文](README.md) | [English](README.en-US.md)

[![YiSha Design](https://img.shields.io/badge/YiSha%20Design-1.0.0-1ab394)](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0)
[![DESIGN.md](https://img.shields.io/badge/DESIGN.md-AI%20ready-0f8b72)](https://stitch.withgoogle.com/docs/design-md/overview/)
[![License](https://img.shields.io/badge/license-MIT-2f4050)](LICENSE)

A versioned YiSha design-system document for AI coding agents.

`DESIGN.md` is a plain-text design-system format introduced by Google Stitch. It turns colors, typography, layout, component states, interactions, and responsive behavior into constraints that an AI agent can follow. It helps generated interfaces remain consistent with the existing product; it does not replace component APIs or application development documentation.

This repository is derived from a real YiSha ASP.NET Core MVC Web frontend snapshot. The static audit covers 50 Razor files (38 application views plus layouts, partials and context files), first-party CSS/JavaScript, inline styles, 41 bundle definitions and the third-party components wired into the application.

Use it to generate, extend or review existing admin pages. It does not provide application source, a preview site or a new component library, and does not replace routing, permission or backend API contracts. The specification separates source facts, dependency behavior, requirements for new work and unverified items.

## Supported Versions

| Design version | Canonical English (maintained) | Simplified Chinese (maintained) | First-release snapshot |
| --- | --- | --- | --- |
| `1.0.0` | [DESIGN.md](versions/1.0.0/DESIGN.md) | [DESIGN.zh-CN.md](versions/1.0.0/DESIGN.zh-CN.md) | [v1.0.0](https://github.com/turtoncarllyle/yisha-design-md/releases/tag/v1.0.0) |

`1.0.0` versions this design specification itself; it is not an upstream YiSha release number. The English `DESIGN.md` is canonical. The Chinese edition uses the same sections, tokens and equivalent rules.

This audit corrects and extends the existing `1.0.0` directory without creating another version number. The maintained files on `main` contain the update; the existing `v1.0.0` tag and Release attachments retain their first-release contents. Use the download URLs below for the maintained specification. For immutable content, select a commit SHA from file history and replace `main` in the URL with that SHA.

[Audit and coverage matrix (Chinese)](versions/1.0.0/AUDIT.md) | [Source SHA-256 manifest](versions/1.0.0/SOURCE-MANIFEST.json) | [Document history](https://github.com/turtoncarllyle/yisha-design-md/commits/main/versions/1.0.0)

## Usage

1. Select the directory for your project's specification version and choose maintained content or the first-release snapshot.
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
Read the complete DESIGN.md 1.0.0 in the project root. Choose its host and page pattern; preserve YiSha palettes, compact density, actual plugin dimensions and responsive rules. Reuse Bootstrap 3, jQuery, ys.* and local adapters; retain permission IDs, col mappings, data contracts and parent callbacks. Separate source behavior from required improvements. Handle pending work, failure recovery, keyboard access and focus. Report unknown business contracts instead of inventing endpoints. Static checks are not UI acceptance.
```

## Coverage

- Default blue header, dark sidebar, collapsible navigation, and iframe multi-tab shell
- Blue, green, purple, red, and yellow skins with dark and light sidebar combinations
- Sign-in, operations dashboard, server monitor, Cron preview, lists, forms, details and permission trees
- Filters, toolbars, buttons, tables, tree tables, pagination, forms, validation, and selection controls
- Layers, dates, uploads, portrait cropping, charts, loading, and feedback
- Permission-denied page, complete breakpoint matrix, accessibility boundaries, Do / Don't and agent prompts

## Technical Baseline

The snapshot is built on Bootstrap `3.3.7`, jQuery `2.1.4` and Font Awesome `4.7.0`, with Layer `3.1.1`, Laydate `5.0.9`, Bootstrap Table `1.12.0`, Select2 `4.0.7`, iCheck `1.0.2`, zTree `3.5.18` and ECharts `4.5.0` among its integrated components.

Versions were checked against implementation declarations: Select2 is stored under `4.0.6` but declares `4.0.7`; File Input is stored under `5.0.3` but declares `5.0.4`. These are documentation corrections, not dependency upgrades. See DESIGN.md section 2 for roles, loading order and boundaries.

The upstream YiSha project is available at [liukuo362573/YiShaAdmin](https://github.com/liukuo362573/YiShaAdmin). This repository is an independently maintained design-system document. It is not official YiSha documentation and is not endorsed by the YiSha project.

The local source cannot be tied to an upstream tag, so file hashes identify the evidence without claiming upstream-latest parity. This work performed only static source/document checks; no business system, database access or browser UI acceptance test was run.

## License

Original documentation is released under the [MIT License](LICENSE), with `turtoncarllyle` as copyright holder. YiSha and third-party components retain their own licenses; this documentation license does not replace the rights governing upstream images, fonts or icons.
