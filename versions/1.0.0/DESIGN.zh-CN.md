---
version: "1.0.0"
name: "YiSha 设计系统"
description: "以源码为依据的 YiSha 界面规则，明确证据、集成契约和验证边界。"
colors:
  content_primary: "#1ab394"
  content_primary_hover: "#18a689"
  shell_header: "#3c8dbc"
  shell_logo: "#367fa9"
  shell_sidebar: "#2f4050"
  shell_sidebar_active: "#293846"
  shell_sidebar_text: "#a7b1c2"
  page_background: "#f3f3f4"
  surface: "#ffffff"
  border: "#e7eaec"
  text: "#676a6c"
  auth_dashboard_accent: "#0f8b72"
  action_secondary: "#1c84c6"
  info: "#23c6c8"
  warning: "#f8ac59"
  danger: "#ed5565"
  input_border: "#e5e6e7"
  validation_background: "#fbe2e2"
  validation_border: "#c66161"
  layer_confirm: "#1e9fff"
  date_selected: "#009688"
typography:
  font_family: '"Microsoft YaHei", "open sans", "Helvetica Neue", Helvetica, Arial, sans-serif'
  modern_font_family: '-apple-system, BlinkMacSystemFont, "Segoe UI", "Microsoft YaHei", Arial, sans-serif'
  base_font_size: "12px"
  base_font_weight: "400"
  compact_line_height: "1.42857"
---

# YiSha 设计系统

[English](DESIGN.md) | [简体中文](DESIGN.zh-CN.md) | [审计报告](AUDIT.md) | [源码清单](SOURCE-MANIFEST.json)

## 1. 概述

### 1.1 目的与使用方式

本规范用于生成或审查紧凑的 YiSha 后台列表、表单、树、详情、iframe 壳层、认证和运营概览页面。它不仅记录视觉组合，还记录集成契约，避免外观相似的控件产生不同的行为。

选择版本、完整读取本文，再从第 8 章选择最接近的页面模式，并保留宿主应用的路由、权限、响应字段和回调。如果只做独立视觉实现，仍保留这些模式，但必须明确缺少的业务契约，不虚构可用接口或权限规则。

英文版为标准版，中文版规则等价。`1.0.0` 是本文档的版本，不是 YiSha 上游版本。本次审计直接更新 `main` 上现有 `versions/1.0.0` 文件，不升级依赖，也不改变版本号。现有 `v1.0.0` 标签和 Release 附件仍保留首次发布快照。通过 Git 历史和固定提交识别准确的文档修订，不要假定历史附件已经包含本次更新。

### 1.2 证据分类与非目标

- **F（源码事实）：** 已出现在审计的视图、自有实现或配置中。
- **D（依赖行为）：** 来自内置依赖的指定版本，仍受页面选项和 CSS 层叠影响。
- **R（新实现需补充的规则）：** 本规范提出的要求，不代表旧页面已经实现。
- **U（待验证）：** 需要本次静态审计尚未取得的运行、部署或来源证据。

下文未单独标记的源码描述，根据对应证据属于 F 或 D；命令式规则用于保留这些契约，超出源码的补充标为 R。本文不替代后端/API 文档、不提供应用源码、不认证无障碍或安全合规，也不定义新的组件库。本次没有运行业务系统、数据库或浏览器 UI。

## 2. 源码基线

### 2.1 快照与可追溯性

基线为用户提供的 YiSha ASP.NET Core MVC Web 快照。本机源码没有能够确定上游标签的 Git 元数据。[上游项目](https://github.com/liukuo362573/YiShaAdmin) 只用于归属说明，不能证明其当前默认分支与本快照一致。不得混入上游最新版行为。

静态清单共 50 个 Razor 文件：38 个业务视图、5 个共享布局文件、5 个 Razor 上下文文件、2 个区域局部视图。另记录 17 个用于核对路由/数据契约的 Web Controller、全部自有 CSS/JS、41 个 bundle 定义及其输入/输出和直接引用资源。清单以相对路径、角色、字节数和 SHA-256 标识 219 个文件，不分发文件内容，也不能证明它与首次编写 `1.0.0` 时未留指纹的源码快照逐字节相同。

以下证据路径均相对用户提供的 Web 根目录，不是本资料仓库中的文件链接。`:number` 表示所捕获快照中的一基行号。完整 Razor 覆盖矩阵见审计报告。

| 证据 | 相对源码路径与职责 |
| --- | --- |
| E01 | `Views\Shared\_Layout.cshtml:1`、`_Index.cshtml:1`、`_Form.cshtml:1`、`_FormWhite.cshtml:1`、`_FormGray.cshtml:1`：宿主、导入、body 类、加载顺序。 |
| E02 | `wwwroot\yisha\css\style.css:1`：基础字体、控件、壳层及继承模板样式。 |
| E03 | `wwwroot\yisha\css\skins.css:7`：皮肤选择器与侧栏主题。 |
| E04 | `wwwroot\yisha\css\yisha.css:1`：最终共享覆盖、列表、校验、选择和加载。 |
| E05 | `wwwroot\yisha\css\login.css:5`、`Views\Home\Login.cshtml:1`：登录展示和流程。 |
| E06 | `Views\Home\Index.cshtml:19`、`wwwroot\yisha\js\yisha-index.js:1`：壳层标记和页签管理。 |
| E07 | `Views\Home\Skin.cshtml:1`：十种可选择的皮肤/侧栏组合。 |
| E08 | `wwwroot\yisha\js\yisha.js:6`：弹层、请求、消息、加载、选择和导出辅助方法。 |
| E09 | `wwwroot\yisha\js\yisha-plugin.js:1`：数据绑定、下拉框、树下拉框和选择组。 |
| E10 | `wwwroot\yisha\js\yisha-init.js:1`：共享 ready 处理、工具栏权限和校验名称。 |
| E11 | `wwwroot\yisha\js\yisha-jquery-bootstrap-table-plugin.js:16`：表格契约。 |
| E12 | `wwwroot\yisha\js\yisha-jquery-bootstrap-treetable-plugin.js:1`、`yisha-jquery-ztree-plugin.js:1`：层级适配器。 |
| E13 | `Areas\OrganizationManage\Views\User\UserIndex.cshtml:1`：树/列表分栏、筛选、工具栏、CRUD、导入/导出。 |
| E14 | `Areas\OrganizationManage\Views\User\UserForm.cshtml:1`、`ChangeUser.cshtml:1`、`UserDetail.cshtml:1`、`ChangePassword.cshtml:1`、`ResetPassword.cshtml:1`：账号表单。 |
| E15 | `Areas\OrganizationManage\Views\User\UserImport.cshtml:1`、`UserPortrait.cshtml:1`：上传与裁剪流程。 |
| E16 | `Areas\SystemManage\Views\Menu\MenuForm.cshtml:1`、`MenuChoose.cshtml:1`、`MenuIcon.cshtml:1`、`Areas\SystemManage\Views\Role\RoleForm.cshtml:1`：菜单/权限编辑。 |
| E17 | `Areas\SystemManage\Views\AutoJob\AutoJobForm.cshtml:1`、`AutoJobIndex.cshtml:1`：任务配置与 Cron 预览。 |
| E18 | `Views\Home\Welcome.cshtml:1`、`Areas\ToolManage\Views\Server\ServerIndex.cshtml:1`：看板与服务器监控。 |
| E19 | `Views\Home\NoPermission.cshtml:1`、`Views\Home\Error.cshtml:1`：异常页面边界。 |
| E20 | `bundleconfig.json:1`：打包清单；实际依赖版本见对应输入文件头。 |
| E21 | `wwwroot\lib\bootstrap.table\1.12.0\extensions\mobile\bootstrap-table-mobile.js:1`：移动端卡片视图切换。 |
| E22 | `wwwroot\lib\layer\3.1.1\theme\default\layer.css:73`、`wwwroot\lib\laydate\5.0.9\theme\default\laydate.css:2`：独立浮层样式。 |
| E23 | `Startup.cs:128`、`appsettings.json:1`、清单内的 Web Controller：区域路由和虚拟目录上下文。 |
| E24 | `Areas\SystemManage\Views\Area\AreaIndex.cshtml:92`、`Areas\SystemManage\Controllers\AutoJobLogController.cs:1`：未完成的源码流程。 |

### 2.2 实际依赖边界

目录标签不是权威版本号。保留现有资源路径，但分析行为时以实现声明的版本为准。此表不要求升级依赖。依据为 E01/E20 和表内路径。

| 能力 | 审计到的实现 | 加载与边界 |
| --- | --- | --- |
| DOM | jQuery `2.1.4` | 共享加载；其 bundle 还包含 BlockUI `2.7`、Cookie `1.4.1`、Fullscreen `1.2`。 |
| 栅格与基础控件 | Bootstrap `3.3.7` | 共享 12 列栅格；JS bundle 还包含 `bootstrap.dropdown.js`。不要加载 vendor 目录中的 Bootstrap `4.0.0`。 |
| 应用图标 | Font Awesome `4.7.0` | 共享 `fa` 类。Bootstrap Glyphicons 是既有依赖例外，登录复选框也使用其字形。 |
| 弹层/消息 | Layer `3.1.1` | `ys.*` 背后的共享桌面实现；辅助方法的移动尺寸分支不等于加载 Layer 的独立移动实现。 |
| 日期/时间 | Laydate `5.0.9` | 列表/表单宿主加载；页面单独调用 `laydate.render`。没有统一生效的 `molv` 或自动起止联动。 |
| 记录表格 | Bootstrap Table `1.12.0` | 列表宿主加载；bundle 包含移动扩展、中文 locale 和 `ysTable`。 |
| 层级表格 | Bootstrap TreeTable 目录 `1.0` | 部门、菜单和区域页面使用；bundle 包含 `ysTreeTable`。 |
| 层级选择 | zTree `3.5.18` | `wwwroot\lib\zTree\v3\js\jquery.ztree.all-3.5.js:3`；Metro 主题，表单宿主和用户列表加载；bundle 包含 `ysTree`。 |
| 增强下拉框 | Select2 `4.0.7` | `wwwroot\lib\select2\4.0.6\js\select2.js:2` 文件头声明；目录为 `4.0.6`。`ysComboBox` 的普通单选也使用 Select2。 |
| 复选框/单选框 | iCheck `1.0.2` | 表单宿主加载，使用 `icheckbox-blue` / `iradio-blue`，不是登录复选框的实现。 |
| 校验 | jQuery Validation `1.14.0` | 表单宿主与登录加载；扩展方法和中文消息已打包。 |
| 文件导入 | File Input `5.0.4` | `wwwroot\lib\fileinput\5.0.3\js\fileinput.js:2` 文件头声明；目录为 `5.0.3`。只有用户导入页加载。 |
| 头像 | Cropbox 目录 `1.0` | 只有头像页加载；不能仅凭目录推断上游标签。 |
| 图表 | ECharts `4.5.0` | `wwwroot\lib\report\echarts\echarts.js:27406`；看板 bundle 还包含 `china.js`，但当前看板没有地图。 |
| 分栏 | jQuery Layout `1.4.4` | 用户列表的部门面板，不是通用壳层布局。 |
| 壳层导航 | MetisMenu `1.1.3`、SlimScroll `1.3.8` | 共享壳层导入；滚动轨道 `4px`。 |

41 个配置输出中，30 个被视图引用，11 个未被引用。后者包括 SmartWizard `4.0.1`、jQuery UI `1.12.1`、Highlight `9.13.1`、Image Upload 目录 `1.0`、Peity `3.3.0` 和 Tags Input `0.8.0`，不能据此认定项目已有向导、编辑器、标签输入或迷你图表页面。Bootstrap `4.0.0`、Summernote、Lightbox2、jQuery context-menu、额外表格扩展和其他 locale 文件仅保存在 vendor 中，没有接入已审计页面。

### 2.3 加载顺序、作用域与资源

E01/E20：Bootstrap 和 Font Awesome 基础样式先于自有样式 bundle；插件/页面样式随后在宿主对应位置输出；最终 `yisha.css` 覆盖在 body 尾部附近输出。`style.min.css` 按 `animate.css`、`style.css`、`skins.css` 顺序拼接，配置关闭压缩。去除注释和空白后，其内容与提供的 bundle 一致。这不代表所有压缩 JS 都已证明运行行为等价。

打包辅助方法在 debug 构建输出输入文件，否则输出配置产物。表格/树 bundle 已含适配器时，不要重复加载。页面 ready 处理先于最终共享初始化注册，已填充下拉框和生成 name 的顺序有实际影响。父 body 的皮肤类不会跨 iframe 继承，浮层属于调用插件的文档。

E05/E15/E20：在宿主项目中开发时保留 `wwwroot\image\login-background.jpg` 登录位图、头像资源、本地 Font Awesome/Glyphicons 字体、iCheck 雪碧图、zTree 图片、Laydate 字体和导入工作簿。本文档不包含这些资源。Font Awesome 字体采用 SIL OFL `1.1`，CSS 采用 MIT；Bootstrap 采用 MIT；ECharts 采用 Apache `2.0`；File Input 采用 BSD-3-Clause。重新分发前逐项核对文件头/许可证。图片、头像和站点图标的权利未独立确认；本仓库 MIT 只覆盖原创文档，不构成对上游媒体或插件的统一授权。

## 3. 设计原则

### 3.1 三个视觉层次

区分默认蓝色顶栏/深色侧栏、青绿色 CRUD 内容，以及认证/看板的深绿色强调。壳层换肤不等于重着色 iframe 内按钮。`theme-dark` 指深色侧栏，不是整站深色模式。认证、看板和无权限页是传统紧凑布局之外的局部例外。

### 3.2 紧凑操作型组合

重复管理操作优先采用紧凑表格、筛选、相邻工具栏和水平表单。保留现有查询/表格表面，但不要继续增加嵌套卡片、装饰性指标、超大标题或营销式 CRUD 布局。优先用结构、边框和间距建立层级，再考虑颜色和阴影。

### 3.3 契约优先于外观

从最接近的宿主和适配器开始，保留控件 ID、`col` 属性、权限标识、枚举值、URL 上下文和父窗口回调。除非明确要求迁移应用，否则不要用外观相似的现代控件替换 jQuery 组件。R：补足语义与缺失状态恢复，不把每个历史缺陷都视为视觉要求。

## 4. 颜色系统与主题

### 4.1 内容角色与按钮状态

E02：类名具有本项目的特定含义。用户、角色、职位工具栏中，新增为蓝色 `.btn-success`，编辑和搜索为青绿色 `.btn-primary`。导出常用 `.btn-warning`，导入用 `.btn-info`，删除用 `.btn-danger`。遵循最近的页面动作映射，不按现代 Bootstrap 命名直觉推断。

| 类 / 令牌 | 默认 | 悬停、焦点、活动 | 禁用填充 |
| --- | --- | --- | --- |
| `.btn-primary` / `content_primary` | `#1ab394` | `#18a689` | `#1dc5a3` |
| `.btn-success` / `action_secondary` | `#1c84c6` | `#1a7bb9` | `#1f90d8` |
| `.btn-info` / `info` | `#23c6c8` | `#21b9bb` | `#26d7d9` |
| `.btn-warning` / `warning` | `#f8ac59` | `#f7a54a` | `#f9b66d` |
| `.btn-danger` / `danger` | `#ed5565` | `#ec4758` | `#ef6776` |

Bootstrap 还应用禁用透明度。`.btn-white` 是白色描边工具按钮；`.btn-outline` 悬停前为透明。这些是样式状态，不是权限校验或重复提交保护。

正文为 `#676a6c`，查询/表头强文字为 `#333333`，弱文字常用 `#999999`，画布 `#f3f3f4`，白表面 `#ffffff`，分隔线 `#e7eaec`，表头分隔线 `#cccccc`，普通输入边框 `#e5e6e7`。搜索输入边框为 `#dddddd`。E04 校验背景为 `#fbe2e2`，边框 `#c66161`，输入文字 `#cc0000`，错误标签文字 `#ef392b`。

### 4.2 壳层皮肤矩阵

E03/E06/E07：默认 `skin-blue theme-dark`。`Skin` Cookie 保存 `skin-name|theme-name`，有效期 365 天，路径 `/`。选择器提供五种皮肤与两种侧栏主题的组合。

| 皮肤 | 顶栏 | Logo | 深色侧栏选中填充 | 浅色侧栏选中背景 / 文字 |
| --- | --- | --- | --- | --- |
| `skin-blue` | `#3c8dbc` | `#367fa9` | `#1890ff` | `#f0f5ff` / `#2f54eb` |
| `skin-green` | `#00a65a` | `#008d4c` | `#52c41a` | `#f6ffed` / `#52c41a` |
| `skin-purple` | `#605ca8` | `#555299` | `#722ed1` | `#f9f0ff` / `#722ed1` |
| `skin-red` | `#dd4b39` | `#d73925` | `#f5222d` | `#fff1f0` / `#f5222d` |
| `skin-yellow` | `#f39c12` | `#e08e0b` | `#faad14` | `#fffbe6` / `#faad14` |

深色侧栏画布 `#2f4050`，活动祖先/悬停 `#293846`，普通 `.nav > li > a` 文字 `#a7b1c2`，选中文字白色。继承的 `.sidebar a` 色值 `#b8c7ce` 不是当前 `navbar-static-side` 标记的普通文字色。顶级活动祖先使用 `3px` 皮肤色边框；`.selected` 是独立状态。浅色侧栏为 `#f9fafc`，普通文字 `#777777`；蓝色活动祖先文字 `#1890ff`，与选中文字 `#2f54eb` 不同。由于共享选择器，其他浅色皮肤的悬停背景也为淡蓝。

样式还包含 `theme-blue`，背景 `rgba(15,41,80,1)`，文字 `#a3b1cc`，但皮肤选择器没有入口。将其视为未接入的继承能力，不是第十一种可选主题。不要承诺换肤会同步重着色 Layer、Select2、iCheck、Laydate 或 ECharts。

### 4.3 局部颜色与层叠

E05/E18/E19：登录/看板强调色 `#0f8b72`；登录墨色 `#17202a`、金色 `#f4bd3f`、辅助蓝 `#2f6fed`；看板墨色 `#17211d`、画布 `#eef2f1`、边框 `#dce4e0`、时间强调 `#f6c453`。无权限页使用墨色 `#18222f` 和金色 `#f3c04d`。这些不是全局内容令牌。

E22：Layer 默认确认按钮是 `#1e9fff`；Laydate 选中日期为 `#009688`。E04 将 Select2 多选项改成 `#1ab394`。E02 `.form-control:focus` 用 `!important` 设置青绿边框，因此登录页后加载的深绿焦点规则不会赢得边框颜色，但绿色焦点阴影仍生效。判断优先级必须考虑重要性、选择器权重、加载顺序和所属文档，不能只看文件中最后出现的颜色。

## 5. 字体与内容

### 5.1 源码字号层级

E02：使用 front matter 的 `font_family`，字号 `12px`、字重 `400`，继承 Bootstrap 约 `1.42857` 的行高。活动布局中未找到 Open Sans 网络字体下载，它只是回退字体名称。普通表单标签为常规字重。侧栏链接为 `13px` / `600`，折叠后的嵌套链接为 `12px`。壳层 Logo 单独采用 Helvetica 优先字体栈，字号 `16px`。

Label 为 `10px`、内边距 `3px 8px`；Badge 为 `11px`、内边距 `4px 6px`。Ibox 标题约 `14px`，`.box-main` 标题 `16px`，通用 Box 标题 `18px`。`8px` 圆角的看板使用 `28px` 指标数字，但仍继承默认字体栈，不是登录字体栈。

E05：只有登录采用 `modern_font_family`；品牌字号 `42px`，辅助主标题 `30px`，在 `880px` 变为 `34px` / `24px`。E19 无权限标题为 `30px`，在 `680px` 变为 `24px`；其系统优先字体栈未显式包含 Arial 回退。字间距保持 `0`，不要随视口宽度连续缩放字号。

### 5.2 内容与格式契约

E13/E17/E18：列表日期使用 `yyyy-MM-dd`；记录时间显示和任务日期时间输入使用 `yyyy-MM-dd HH:mm:ss`。保留字符串 ID、枚举状态底层值和响应字段名。状态既显示颜色，也显示文字。看板缺失更新时间使用 `--`；任务无限期结束标记为 `9999-12-31 00:00:00`，不是普通的用户可读到期时间。

R：区分零、数据不可用和请求失败，不要一律转换为 `0`。服务端未提供转换契约时保留其单位/时区。操作文案简洁，出错时保留输入，通过换行、详情或可访问展开方式呈现长内容。不要新增不实的本地化声明：已打包中文表格/校验/上传文案，但没有完整应用语言切换或统一时区/数字格式策略。

## 6. 布局与间距

### 6.1 壳层尺寸

依据 E01/E02/E06；尺寸描述实际 `fixed-sidebar` 宿主，不代表所有继承模板类。

| 元素 | 源码尺寸 / 行为 |
| --- | --- |
| 展开导航 | 固定宽 `200px`；桌面页面左外边距 `200px`。 |
| 折叠导航 | 宽度和页面偏移 `50px`，不是 `65px`。 |
| 折叠悬浮菜单 | `left:50px`；标签在顶部，二级菜单 `top:40px`，最小宽度 `140px`。 |
| 顶栏 / Logo | `50px`；保留紧凑账号和折叠控件。 |
| 页签条 | `42px`；按钮/页签高 `40px`。 |
| 页面包裹 | 内边距 `0 15px`；`.wrapper-content` 通常为 `20px`。 |
| iframe 内容宿主 | 普通 `#content-main` 的最后规则：`height:calc(100% - 127px); overflow:hidden`。 |
| 用户列表分栏 | jQuery Layout 西侧面板 `185px`，与壳层侧栏不同。 |

早期 `.mini-navbar li.active .nav-second-level { left:65px; }` 在固定侧栏悬停场景被覆盖；早期 `#content-main {height:100%}` 也不是普通场景最终值。权重更高的继承 `.fixed-nav` 仍有独立高度，但它不是默认 body 类。R：修改宿主时核对真实顶栏/页签/iframe 边界，不要把 `127px` 减法搬到无关布局。

### 6.2 列表与表单

E04：`.container-div` 使用 `10px 35px`；`.search-collapse` / `.select-table` 使用白色、`6px` 圆角、`10px` 上外边距、`5px` 上 / `13px` 下内边距，以及 `1px 1px 3px rgba(0,0,0,.2)` 阴影。筛选项高 `30px`、上下外边距 `5px`、右间距 `15px`，普通输入/下拉宽 `280px`，日期端点宽 `133px`。增强下拉框的实际高度仍取决于插件。

E01/E02：采用 `.form-horizontal`、`.form-group`、`.control-label` 和 Bootstrap `col-sm-*`，常见比例 `3 + 8` 或 `2 + 10`。标准输入继承 `34px` 高度、`6px 12px` 内边距，文字 `12px`、圆角 `1px`。表单常用 `15px` 行间距和 `15-20px` 外围内边距。保存/关闭属于 Layer 宿主按钮行或页面既有表单操作区。

### 6.3 专项页面尺寸

E05：登录壳层最大 `960px`，卡片宽 `386px`、内边距 `30px`、圆角 `8px`；字段和验证码高 `46px`，提交按钮高 `48px`。使用真实背景位图和 CSS 覆盖层。复现该局部例外，不代表可以给 CRUD 页面增加渐变或 Hero 布局。

E18：看板内边距 `18px`、间隔 `12-14px`、指标卡最小高 `122px`、面板最小高 `322px`、图表高 `252px`、指标图标 `34px`。E19：无权限表面最大 `760px`，列宽 `132px minmax(0,1fr)`，间距 `34px`、内边距 `42px`、圆角 `8px`；文字列明确设置最小宽度，避免文字撑大栅格。

## 7. 形状、阴影与动效

### 7.1 圆角、边框与阴影

E02/E04/E05/E18/E19：输入圆角 `1px`，紧凑控件/下拉/加载 `2-3px`，筛选和 Bootstrap 细节 `4px`，查询/表格表面及 Cron 助手 `6px`，登录/看板/无权限局部表面 `8px`，头像为圆形。保留既有例外，不要把全部组件统一成大圆角。

| 表面 | 源码处理 |
| --- | --- |
| 输入、面板、进度 | 多数为扁平；共享规则抑制默认阴影。 |
| `.box` | `3px` 顶边 `#d2d6de`，阴影 `0 1px 1px rgba(0,0,0,.1)`；`.box-main` 去掉这两种框架效果。 |
| 下拉菜单 | `0 0 3px rgba(86,96,117,.3)`，紧凑边框和内边距。 |
| 看板 | `0 12px 28px rgba(23,33,29,.07)`。 |
| 登录 | `0 28px 70px rgba(23,32,42,.22)`。 |
| 无权限 | `0 26px 70px rgba(24,34,47,.16)`。 |
| Laydate | `0 2px 4px rgba(0,0,0,.12)`，圆角 `2px`。 |

### 7.2 层级与动效边界

E02/E04/E08/E22：树选择遮罩/面板层级 `99/101`，Select2 下拉 `1051`，固定侧栏 `2001`，Layer JS 默认基数 `19891014` 加实例索引，Laydate CSS 为 `66666666`。这些是各文档局部值，不是统一全局令牌体系。提高 iframe 内部 z-index 无法越过父级层叠上下文。调整层级或 `dropdownParent` 前先确认所属文档。

已有动效包括输入边框 `150ms`、Logo 宽度 `300ms`、jQuery 页签滚动、菜单淡入 `500ms`、服务器面板折叠 `200ms` 和无限循环加载指示 `400ms`。E05/E19 还使用短悬停过渡；继承动画类不是每页必选项。R：状态切换时保持布局尺寸稳定，提供减弱动效和可读加载文字，不添加装饰性循环或强制所有状态都动画化。

## 8. 组件与页面模式

### 8.1 宿主、导航与页签

E01/E06：`_Layout` 承载壳层/登录；`_Index` 承载列表与看板；`_FormWhite` / `_FormGray` 包裹 `_Form`，用于事务型页面；`NoPermission` 不使用共享布局。iframe 页面不能重复创建应用顶栏/侧栏/页签管理器。

壳层包含嵌套菜单、折叠按钮、滚动轨道、账号下拉和按 URL 标识的 iframe 页签。保留首页常驻页签，相同 URL 复用，同步活动祖先与选中菜单，只显示活动 iframe。左右滚动、刷新、关闭当前、关闭其他和全部关闭都是已有命令。账号操作包括资料、密码、头像/身份上下文、换肤和退出，应连接真实 Controller 操作，不使用演示链接。

账号图片 `27px`，下拉宽 `138px`。展开时二/三级菜单文字缩进 `52px` / `62px`。菜单数据与权限来自宿主，不是客户端 SPA 路由。当前视图没有接入全局面包屑、侧栏搜索、顶侧混合布局、右抽屉、聊天、时间轴、富文本编辑器或向导。仅有继承 CSS 或 vendor 目录不能证明这些组件已存在。

### 8.2 搜索、工具栏与按钮

E10/E11/E13：`#searchDiv` 中的筛选通过 `col` 绑定，Enter 触发 `#btnSearch`。搜索刷新第 `1` 页，并调用 `resetToolbarStatus()`。新增/编辑/删除/导入/导出位于表格正上方；`.btn-group-sm` / `.btn-sm` 用于工具栏密度，`.btn-xs` 用于行内动作。当前列表没有通用筛选重置命令。R：只有需求要求时才增加，先重置组件和隐藏筛选值，再执行查询。

选择事件在零行时给删除添加 `.disabled`，在非单行时给编辑添加 `.disabled`。处理函数仍需 `ys.checkRowEdit` / `ys.checkRowDelete` 保护，只有禁用样式不够。批量删除先确认选中数量，再传逗号分隔 ID，成功后刷新。R：等待期间阻止重入，失败时适当保留选择/筛选，并让删除后空页回到有效页码。

### 8.3 字段、下拉、选择与校验

E09/E10/E14：`getWebControls` 只读取带 `[col]` 的后代，不读取全部字段，也不是普通表单序列化。`setWebControls` 回填控件，但部分 `DIV`/`SPAN` 使用 HTML 插入。保留准确的 `col` 字段名。共享代码只为文本/密码/单选输入和 select 将 ID 复制到 `name`；其他待验证字段需显式 name。R：不可信显示值要编码，textarea、生成控件和错误消息要明确关联。

`ysComboBox` 生成 `id_select`，对单选/多选都初始化 Select2。查询中的“全部”值为 `-1`，表单占位选项为空值。保留配置的 `dataName`、值/文本字段和逗号分隔选择格式。Select2 单选高 `28px`，多选最小高 `32px`，选项可换行；普通样式边框 `#aaaaaa`，禁用使用灰表面和插件焦点状态；E04 只覆盖已选项样式。不要强制所有选择框都使用 `30px` 或 `34px`。

选择组使用本地辅助方法及 iCheck 蓝色选中/禁用雪碧图，尺寸和事件与原生复选框不同。R：保留原生标签、checked/disabled 语义和键盘行为，业务需要时提供半选状态。不要把每个枚举或单选组都换成开关。

校验错误标签绝对定位为 `right:18px; top:7px; font-size:12px`，选择组另有偏移。这是源码事实，不代表长错误文案一定容纳得下。R：消息靠近字段且不遮挡值，窄屏允许换行，下拉值变化时校验，并将焦点放在首个无效字段。必填标记为红色，紧邻标签。

### 8.4 记录表格与分页

E11/E13/E21：使用 `ysTable`，不替换表格库。默认 GET、服务端分页、`Id` 降序、每页 `10` 条、选项 `10, 25, 50, 100`、唯一键 `Id`、总数 `Total`、行数据 `Data`。`getPagination` 将请求映射为 `pageSize`、`pageIndex`、`sort`、`sortType`，通过 `getWebControls` 合并筛选。检查 `Tag == 1`，适配器会报告业务错误和非取消的加载失败。

适配器启用列选择、刷新、卡片/表格切换和点击选中，不启用详情行展开。保留逐列显示/排序/对齐和枚举格式化。单元格内边距 `8px`；Bootstrap Table 表头内部行高 `24px` 加 `8px` 内边距，所以密度紧凑，但并非所有行固定 `30px`。表体自动溢出滚动，边框受最终共享覆盖控制。分页内边距 `4px 10px`，活动页浅灰。

每个现有平面列表都设置 `data-mobile-responsive="true"`；扩展自身默认 `mobileResponsive:false`、`minWidth:562`、`columnsHidden:[]`。在 `562px` 及以下切换卡片视图，不会自动生成按优先级删列的表格。中文加载/无匹配文案来自已打包 locale。R：区分成功空结果与请求失败，保留重试入口，并验证切换卡片视图后选择/操作仍有效。

### 8.5 树、树选择与树表

E12/E13/E16：用户页由 `185px` 部门面板和列表组成；选择节点设置 `DepartmentId` 并重查，展开/折叠/刷新操作作用于树而非表格。`ysComboBoxTree` 创建 `id_input` / `id_tree`；`data-key` 保存祖先 ID 的逗号串，`data-value` 保存 `>` 分隔显示路径。需要保存叶子 ID 时使用 `ys.getLastValue`。

两个区域局部视图通过 `areaId` 绑定 `AreaId`，加载 `SystemManage/Area/GetZtreeAreaListJson`。表单局部视图从 `ViewData` 获取 Bootstrap 标签/内容宽度，设置 `expandLevel:0`；筛选局部视图为行内查询项。复用这些树选择局部视图，不虚构多下拉级联。

角色权限编辑先填充菜单树，再回填 `MenuIds`，保存勾选 ID 逗号串。保留源码的父子勾选语义，不要悄悄换成独立勾选或另一套授权模型。菜单选择搜索使用既有树搜索处理函数。

部门/菜单/区域层级表格使用 `ysTreeTable` 和 `bootstrapTreeTable`。普通键为 `Id` / `ParentId`；区域为 `AreaCode` / `ParentAreaCode`，`expandColumn:2`。展开控件属于有意义的层级列。树表上的 `data-mobile-responsive` 不会激活 Bootstrap Table 的移动扩展。R：提供键盘层级操作和受控横向滚动，不隐藏必要动作；不要把区域源码中错误的表格 API 和缺失表单当作有效行为复制，见第 14 章。

### 8.6 弹层、反馈与加载

E08/E22：`ys.openDialog` 使用 Layer type `2`（URL iframe），默认宽 `768px`，未指定高度时为 `$(window).height() - 50` 像素，启用最大化/最小化，遮罩 `0.4`，确认/关闭按钮，`shadeClose:false`。`ys.openDialogContent` 使用 type `1`（HTML），默认无标题/按钮、不启用最大化/最小化，`shadeClose:true`。辅助方法基于 user-agent 判断移动端时尺寸变为 `auto`，与 CSS 断点是两套机制。

回调找到子 iframe 并调用 `saveForm(index)`。`Tag == 1` 后，子页调用其既有父级刷新函数（`searchGrid`、`searchTreeGrid(id)` 或 `getForm`），再关闭同一 Layer 索引。详情、导入、裁剪和皮肤页可覆盖标题、尺寸和按钮。不要假定每个弹窗都提交表单，或所有刷新函数都同名。

辅助方法传入 `fix`，而该 Layer 版本的选项是 `fixed`，前者不能证明弹窗采用非固定定位。alert 包装器中的 `btnclass` 也不能证明 Layer 按钮被改成 Bootstrap 青绿。Confirm 在调用回调前先关闭确认框，不会自己执行操作。

成功消息使用 `top.layer`，持续 `1000ms`；警告使用本地 `layer`、`1000ms`；错误使用本地 `layer`、`3000ms`。Alert 变体需要确认。加载采用 BlockUI、最小 `125px` 的加载器和 `18px` 旋转指示，关闭延迟 `50ms`。R：提供有意义名称/live region、等待重入保护、失败恢复和焦点返回；重要信息不能只依靠一秒提示。

### 8.7 日期、导入与头像流程

E10/E13/E17/E22：实际列表分别调用 `laydate.render`，起止字段使用 `yyyy-MM-dd`，任务字段使用 `datetime`。共享 `.select-time.length > 10` 分支使用 `layui.use` 和 `molv`，但现有页面不满足条件，因此没有生效的自动范围联动。普通日期面板主宽 `272px`、单元格 `36px` × `30px`、范围容器 `546px`。R：需要时显式添加起止限制并校验先后顺序，不要假定改变标记就会激活未生效的共享分支。

E15 导入分两阶段：File Input 接受 `xls/xlsx`、隐藏预览、上传到 `File/UploadFile` 并保存成功的 `FilePath`；确认时调用 `ImportUserJson`，提交路径和 `IsOverride`。保留模板下载和覆盖选择。R：移除/替换/失败时清空旧路径，上传成功前禁用导入，并区分上传失败与导入失败；客户端扩展名限制不是服务端内容验证。

E15 头像使用 `400px` 裁剪表面、`200px` 裁剪边界和 `64/128/180px` 预览，保留选图、缩放、裁剪和显式保存。Blob 上传字段为 `fileList`；返回路径通过 `ChangeUserJson` 保存为 `Portrait`，再刷新父级头像。R：处理无效文件、解码失败、上传失败、取消和窄屏溢出；固定裁剪尺寸不是移动可用性证明。

### 8.8 业务列表、表单与详情

E13/E14/E16/E17/E24 及清单：用户、职位、角色、字典、字典明细、任务和日志采用平面表格；部门、菜单和区域采用树表。打开字典明细列表时保留父字典 ID，也保留部门/用户上下文、角色菜单 ID 和每个枚举的底层值。

用户资料/详情、密码/重置、部门/职位、字典/明细和角色表单使用表单宿主及各自必填/只读字段。API 与操作日志详情展示请求/结果文本；R：将不可信字符串按文本展示，长值换行，区分缺失值与空载荷。不要把只读日志详情变成编辑字段，也不要虚构源码中不存在的 AutoJobLog 表单。

菜单表单根据目录/菜单/按钮类型切换 URL、授权和图标字段。图标选择器为最大 `200px` 的滚动浮层；图标 `18px`，宽 `28px`，外边距/内边距 `5px`，圆角 `3px`，悬停 `#1d9d74`。451 个图标类均能在 Font Awesome `4.7.0` 中找到。R：保留键盘进入/退出路径，给每个图标名称提供可访问名称，不使用较新 Font Awesome 的图标。

### 8.9 Cron 预览与服务器监控

E17：任务编辑有六种 Cron 预设和预览助手，圆角 `6px`、背景 `#f8fbff`、边框 `#e5edf5`。空输入提示填写表达式，等待时显示计算消息，成功列出后五次时间，业务失败显示错误类和文案。`GetCronNextRunTimeJson` 只预览计划，不执行任务；保留开始时间和无限期结束标记。R：拒绝过期预览响应，将预览与任务列表的启动/停止/运行操作区分清楚。

E18：服务器监控与业务看板不同，使用 CPU/RAM ibox 及服务器/.NET 属性表，每 `3000ms` 用原生 `$.ajax` 轮询。折叠动画 `200ms`，关闭会移除面板；关闭面板不自动停止轮询，也没有完整错误/重试生命周期。R：离开页面时停止计时器，避免请求重叠，暴露数据不可用状态，不要每三秒显示一次全局加载遮罩。

### 8.10 登录、看板与异常

E05：登录包含账号/密码/验证码、验证码刷新、记住账号、校验、等待提交和响应反馈。公开品牌保持 YiSha，不复制本地定制名称，也不把默认账号提示作为生产凭据。R：验证码刷新需有名称且可键盘操作，失败后保留账号文字，密码/验证码处理遵循真实服务端契约。

E18：看板包含深色摘要区、六项指标、活跃折线图和分类环图。实际折线只有一条序列 `#0f8b72`；环图半径为 `46% / 72%`，颜色依次为 `#0f8b72`、`#315d95`、`#d89b22`、`#c95746`、`#6d7d73`。图表在窗口变化时 resize；数据只加载一次，空状态使用空图表容器和 `UpdatedAt` 回退 `--`，没有自动刷新，`loadDashboard` 本身不检查 `Tag`。R：显式处理空数组、业务错误、请求失败和图表销毁，不把示例/缺失数据标成实时数据。

E19：无权限页是独立 `403`，包含语义化主内容、对辅助技术隐藏的装饰锁、说明、返回和首页。历史长度大于 `1` 时返回上一页，否则跳转顶级 `Home/Index`；首页链接会离开 iframe。`Home/Error` 只有 `@ViewBag.Message`，不是完整设计的 `404/500` 系统。R：只有明确真实服务端状态/路由契约后才补运行错误处理，不凭这两个视图声称存在完整异常页体系。

## 9. 集成与状态契约

### 9.1 请求与权限

E08：`ys.ajax` 包装 jQuery 回调、JSON 请求、默认错误提示和加载生命周期，不自动校验 `Tag`，不提供完整状态机、重复提交保护或取消。`success` 在刷新/关闭前必须核对业务响应。上传通过 `ys.ajaxUploadFile`，设置 `processData:false` 和 `contentType:false`；导出通过 `ys.exportExcel` 提交，成功后跳转到返回的下载路径。

E10/E23：工具栏权限扫描 `#toolbar a` 和 `.toolbar a`，ID 和当前 URL 交给 `top.getButtonAuthority`，生成如 `organization:user:add` 的标识。`#toolbarPermission` 不自动包含在扫描中。客户端移除只是展示，服务端授权与路由契约必须保留。R：ID 唯一；把锚点改善成按钮时应同步适配权限采集器，否则新控件会绕开旧选择器。

路由支持区域，模板为 `{area:exists}/{controller=Home}/{action=Index}/{id?}` 和 `{controller=Home}/{action=Index}/{id?}`。`Url.Content` / `ctx` 携带部署上下文，包括当前 `/admin` 虚拟目录配置。不要硬编码站点根路径、移除权限检查，或为简化样式而更改数据结构。

### 9.2 状态覆盖与恢复

| 状态 | F / D 基线 | 新实现的 R |
| --- | --- | --- |
| 默认 / 悬停 / 活动 | 源码按钮、菜单、下拉及插件规则。 | 状态尺寸稳定，标签可读。 |
| 焦点 | 青绿表单边框；自定义键盘语义不完整。 | 每个命令都有可见焦点、标签和逻辑焦点顺序。 |
| 选中 / 禁用 | 表格选择、`.disabled`、iCheck/Select2 状态。 | 真正执行禁用行为，保留选择契约。 |
| 加载 | 请求遮罩、表格 locale、iframe 加载、Cron 消息。 | 阻止重复写入，明确等待操作。 |
| 空数据 | 表格无匹配文案和看板空容器。 | 区分无结果与数据不可用，保留恢复动作。 |
| 无效输入 | 校验消息和 Cron 错误状态。 | 保留输入、关联错误、避免遮挡并聚焦首个错误。 |
| 请求 / 业务失败 | 默认请求提示；多数页面单独检查 `Tag`。 | 检查两种失败，恢复控件并保留重试入口。 |
| 成功 | 消息、父级刷新、关闭弹层。 | 仅确认成功后刷新正确宿主，避免丢失上下文。 |
| 危险 / 批量 | 选择保护与数量确认。 | 阻止重入、保留服务端鉴权；API 支持时处理部分失败。 |
| 取消 / 离开 | Layer 关闭和页签移除。 | 归还焦点，释放计时器/监听/图表，必要时定义未保存处理。 |

上述补充不代表每个旧页面已经覆盖每种状态；异步竞态、键盘语义和资源释放仍需运行验证。

## 10. 响应式行为

### 10.1 实际断点矩阵

依据 E02/E04/E05/E06/E18/E19/E21。`<=` 包含边界本身，`<769` 是 JavaScript 判断。测试顶级窗口时也要测试 iframe 自己的视口。

| 边界 | 源码作用 / 范围 |
| --- | --- |
| `>=1200px` | 继承工具类与 Bootstrap 大栅格/容器规则，不是新增看板断点。 |
| `<=1180px` | 看板指标六列变三列。 |
| `1170px` | 仅继承纵向时间轴规则，没有接入时间轴页面，不是全局栅格断点。 |
| `<=1000px` | 隐藏 `.welcome-message`，当前账号/全屏菜单组使用此类。 |
| `>=992px` | Bootstrap 中栅格起点。 |
| `<=880px` | 登录单列，最大 `430px`；操作格隐藏，卡片内边距 `24px`，标题缩小。 |
| `<=820px` | 看板页头、指标和分析面板堆为单列。 |
| `<769px` | 壳层 load/resize 添加 `mini-navbar` 并对侧栏 `fadeIn`；放宽窗口不会自动移除该类。 |
| `<=768px` | CSS 隐藏固定侧栏和页签条；脚本/body 类可让侧栏再次显示。查询表面隐藏。 |
| `>=768px` | Bootstrap 小栅格和筛选项浮动生效；恰好 `768px` 时，查询表面仍受另一条规则隐藏。 |
| `<=767px` | 壳层下拉/账号头像收紧，应用对应下拉颜色。 |
| `<=680px` | 无权限页单列，外内边距 `18px`、内部 `28px`、标记 `96px`、标题 `24px`、动作全宽。 |
| `<=562px` | 已启用移动适配的平面表格切换卡片视图，不自动按优先级删列。 |
| `<=420px` | 登录选项与验证码栅格堆叠，验证码图片宽 `118px`。 |
| `<=350px` | 折叠侧栏宽度变 `0`，显示窄屏导航关闭入口。 |

### 10.2 适配要求

R：不能仅凭 CSS 将窄屏侧栏描述为始终隐藏。联合测试折叠、刷新、放宽窗口、页签切换和 Cookie 皮肤。旧分组隐藏时仍要让筛选和账号操作可达，需要时增加明确的展开入口，不凭空加入宿主没有的通用抽屉。

R：窄表单堆叠 Bootstrap 标签/内容列，插件浮层限制在所属视口，长错误和选择项换行，主要动作保持可访问。表格/树表采用受控溢出或真实卡片视图，不以整页裁切掩盖问题。声明移动支持前核对固定裁剪尺寸和 Layer auto 尺寸；源码断点和静态样式审计不是浏览器验收结果。

## 11. 无障碍与安全边界

### 11.1 已有证据

E01/E06/E10/E19：传统共享布局禁用缩放，多个命令使用缺少原生按钮语义的锚点，自定义页签/树缺少完整键盘模型，iframe 标题/焦点返回不完整。独立无权限页已经具有 `lang="zh-CN"`、命名主区域、真实返回按钮及隐藏装饰标记，应保留这些正向模式。

原色板不构成 WCAG 合规声明。普通青绿按钮白字和浅色/彩色状态组合需要检查对比度，紧凑 `12px` 文字不属于大字。本次未做读屏、键盘、缩放、真实上下文对比度或触摸目标验收。

### 11.2 新实现需补充的规则

R：启用缩放，使用语义化按钮/链接，命名纯图标动作，关联标签和错误，提供可见焦点，给折叠区域补 `aria-expanded` / `aria-controls`，给每个 iframe 有意义的标题。弹层打开时接收焦点，保持连贯键盘交互，关闭后返回触发元素。页签、树、菜单、分页和选择需要完整键盘路径，不能只支持悬停。

R：成功/进度使用礼貌状态通知，紧急失败才使用强提醒；不能只靠颜色。实际检查 WCAG `2.2` AA 对比度和目标尺寸，必要时增加局部无障碍变体，不全局替换品牌色板。支持减弱动效和文字放大，提供图表摘要/数据替代，避免把不可信服务端文本渲染成 HTML。这些要求用于改善源码，不代表应用已经通过认证。

## 12. Do / Don't

### 12.1 应当

- 使用正确共享宿主、本地适配器、bundle 版本和支持虚拟目录的 URL。
- 区分蓝色/深色壳层、青绿内容和局部深绿表面。
- 保留真实新增/编辑色彩映射、紧凑控件和插件各自尺寸。
- 区分源码事实、继承行为、补充建议和未验证结果。
- 保留权限 ID、`[col]` 映射、响应字段、枚举值和 iframe 回调。
- 为新实现补充明确错误恢复、键盘/焦点行为和等待保护。

### 12.2 不应

- 不从目录名推断实际版本，不加载所有 vendor 能力。
- 不声称日期自动联动、辅助方法检查所有业务响应，或所有皮肤自动应用到 iframe 内容。
- 不将 `65px` 当作当前固定侧栏轨道，不把移动表格描述成自动列优先级处理。
- 不把 CRUD 列表变成营销表面，不复制壳层，不全局统一圆角。
- 不引入另一套图标/组件库，只保留既有依赖字形例外。
- 不把缺失视图、重复 ID、不安全 HTML 插入、不可访问控件或静默失败复制为要求。
- 不凭静态文档验证宣称运行、安全、无障碍或浏览器验收通过。

## 13. Agent 提示指南

### 13.1 必需上下文

生成 UI 前确定规范版本、页面模式、宿主、已加载插件、路由/权限 ID、字段、父回调、枚举/格式及必需状态。没有应用源码时，依据本文自包含模式做视觉工作，明确列出未知业务契约，不虚构证据或悄悄更换依赖。

### 13.2 实现提示词

```text
先完整读取 YiSha DESIGN.md 1.0.0，区分源码事实、依赖行为、补充要求与待验证项。
使用规范中的宿主、Bootstrap 3 栅格、Font Awesome 4 图标、本地适配器和 ys.*
实现需求，保留路由、权限 ID、col 映射、响应字段、枚举值和回调。
区分壳层、CRUD 和登录/看板色板，遵循实际插件尺寸和断点矩阵。
补充明确的等待保护、失败恢复、键盘访问和焦点行为，不虚构源码能力。
报告缺少的业务契约；静态检查不是 UI 验收，明确已验证和未验证范围。
```

### 13.3 审查提示词

```text
按照 YiSha DESIGN.md 1.0.0 和最近的既有页面审查。
先检查数据/权限/回调契约、依赖版本、层叠错误以及不可访问或不可达的状态，
再讨论视觉偏好。核对 iframe 边界、移动卡片视图、隐藏筛选/账号动作、
日期初始化、上传阶段和资源释放。每项不一致给出证据，区分源码缺陷、
文档缺陷与建议补充，不声称未运行的浏览器测试已经通过。
```

## 14. 已知缺口与验证清单

### 14.1 本次文档更新没有修复的源码限制

- 令牌是字面 CSS 的文档别名，不是运行时自定义属性；没有完整深色模式或国际化系统。
- 当前页面的共享日期自动逻辑未实际生效；加载、错误、防重复提交和清理覆盖因页面而异。
- 区域动作调用错误表格 API 和不存在的搜索方法，预期区域表单缺失；AutoJobLog 表单 action 也缺少对应视图。这些是源码缺陷，不是新增文档功能。
- 部分任务/日志工具栏重复使用 `btnDelete`，权限工具栏在共享采集器之外；UserForm 备注 textarea 缺少绑定 `col`，ChangeUser 的职位下拉采用不同 `dataName`。保留意图中的业务字段，不复制这些错误。
- 长校验文案、树选择键盘行为、固定裁剪尺寸、小移动目标和被隐藏控件需要运行验证；服务器轮询和 Cron 预览需生命周期/竞态处理。
- 未启用的 boxed-layout CSS 引用缺失背景图，未引用的 jQuery UI CSS 也引用缺失雪碧图。不要把它们报告为活动页面故障，也不要未经资源审计就启用这些模板。
- 对应上游标签、完整图片权利、全部压缩 JS 行为等价、运行路由/数据和浏览器无障碍仍为 U。本次文档更新未修改源码文件。

### 14.2 验收清单

- [ ] 读取选定版本，确认宿主、页面模式和源码快照。
- [ ] 保留准确依赖实现版本，避免重复加载适配器。
- [ ] 在所属文档中核对色板、层叠、尺寸、圆角、阴影、层级和字体回退。
- [ ] 核对请求字段、`Tag`/`Data`/`Total`、权限、ID、`[col]`、枚举和父级刷新回调。
- [ ] 核对悬停、焦点、活动、选择、禁用、加载、空数据、校验、请求失败、成功、取消和恢复。
- [ ] 按需测试 `1200/1180/1170/1000/992/880/820/769/768/767/680/562/420/350px` 及边界相邻宽度。
- [ ] 核对长文本、浮层边界、表格/卡片操作、裁剪可用性和 iframe 高度。
- [ ] 修改 UI 时，在真实浏览器验证键盘、焦点返回、缩放、对比度、目标尺寸、减弱动效和实时通知。
- [ ] 将静态检查与运行/浏览器检查分开记录，没有证据的内容标 U。
- [ ] 保持规范版本 `1.0.0`，用提交和源码哈希追踪原位更新，不移动历史标签或替换 Release 附件。
