# YiSha DESIGN.md 1.0.0 完整性审计

审计日期：2026-09-08。范围：当前提供的 MVC Web 源码快照与首次发布的双语规范。本文记录审计发现、实施方案、覆盖证据及验证边界；**版本号保持 `1.0.0`，直接完善原目录**。

[英文规范](DESIGN.md) | [中文规范](DESIGN.zh-CN.md) | [源码指纹](SOURCE-MANIFEST.json) | [中文首页](../../README.md)

## 1. 总体结论

首次发布文档能支持普通 CRUD 页面的风格近似生成：三层色板、基本布局、常用组件、双语入口和无障碍方向均已建立。但仅凭原文，Agent 容易选错依赖行为、错误理解日期初始化和请求包装器、把移动表格当作自动隐藏列、忽略权限选择器与数据绑定边界；复杂业务流程也不足以准确复现。

本次维护稿补齐了可静态确定的页面模式、状态、数据/权限契约、主题层叠和响应式行为。它能够指导更准确的壳层、列表、树表、表单、导入、头像、Cron、看板、监控与无权限页实现；没有服务端运行证据时，仍不能保证真实接口、鉴权、竞态、键盘和移动可用性。没有为缺失的向导、富文本、抽屉等虚构组件规则。

不以文档长度、关键词数量或 UI 审计总分衡量完整性。未运行界面，因此不作“视觉反模式通过”、WCAG 合规或性能评分。技术审计重点是源码事实与规范之间可复查的差异，而非重新设计历史界面。

## 2. 基线与证据方法

### 2.1 仓库边界

- 文档仓库：[turtoncarllyle/yisha-design-md](https://github.com/turtoncarllyle/yisha-design-md)，公开、默认分支 `main`，保留中英双语仓库描述。
- 审计开始时工作区干净，`main`、`origin/main`、`v1.0.0` 均指向 `7a95aa29cf4811ba84d1485570c0e56fec1f0664`。
- 首发规范基线：[英文原稿](https://github.com/turtoncarllyle/yisha-design-md/blob/v1.0.0/versions/1.0.0/DESIGN.md)、[中文原稿](https://github.com/turtoncarllyle/yisha-design-md/blob/v1.0.0/versions/1.0.0/DESIGN.zh-CN.md)。下文“原 §”指这个不可变快照中的章节，不指更新后的同名章节。
- 前端快照位于用户指定 Web 项目中；该源码没有可识别上游提交/标签的 Git 元数据。上游归属为 [YiShaAdmin](https://github.com/liukuo362573/YiShaAdmin)，不能把其当前默认分支当作本次准确版本来源。
- 参考任务只用于沿用双语、版本目录和发布惯例；技术结论以实际文件重新核对。源码、构建输出、数据库与无关文档仓库均不修改。

### 2.2 审计方法

逐个读取 50 个 Razor 文件的结构、宿主、样式、脚本和交互；图标长列表使用类名枚举核对。检查全部自有样式/脚本的有效规则和调用点，区分继承但未接入的模板代码。17 个 Web Controller 仅用于路由、返回视图与响应契约核对，不声明完成服务端安全审计。

使用结构解析读取带注释的 `bundleconfig.json`，核定 41 个输出及输入。通过视图加载点、依赖文件头、实现默认值和局部覆盖区分：已接入、仅打包、仅保存于 vendor。校验直接资源存在性、自有 JS 语法、图标类名和样式 bundle 拼接一致性；不把压缩前后字符串相似等同于运行行为证明。

[SOURCE-MANIFEST.json](SOURCE-MANIFEST.json) 记录 219 个文件的相对路径、角色、字节长度、SHA-256 和全部 bundle 对应关系。排除构建/中间产物、未引用且不在 bundle 清单中的 vendor 实现、Web 之外的服务端实现和文件内容。路径使用跨平台 `/` 分隔；规范中的人工源码定位使用 Windows `\`。不记录本机绝对源码目录或敏感配置内容。

证据按 F（源码事实）、D（依赖行为）、R（新实现补充要求）、U（待验证）区分。规范第 2 章的 E01-E24 为快捷证据索引；以下重要发现另给准确路径和行号。

## 3. 问题清单

共 12 项文档问题：P0 为 0 项，P1 为 5 项，P2 为 7 项。P1 表示足以误导实现契约或关键交互；P2 表示信息不完整、定位不清或可绕过的偏差。以下修正已纳入维护稿，列出的源码缺陷只记录、不修源码。

### P1-01：依赖目录名被误当实际版本

- 问题与证据：原 §2 和双语 README 将 Select2 写为 `4.0.6`、File Input 写为 `5.0.3`；`wwwroot\lib\select2\4.0.6\js\select2.js:2` 声明 `4.0.7`，`wwwroot\lib\fileinput\5.0.3\js\fileinput.js:2` 声明 `5.0.4`。原文 zTree/ECharts 未给准确实现版本；对应文件 `jquery.ztree.all-3.5.js:3`、`echarts.js:27406` 为 `3.5.18`、`4.5.0`，完整路径见规范 E12/E20 及清单。
- 影响：Agent 可能套用错误 API/样式默认值，并把未接入上传或报告能力当作现有页面。
- 建议与落点：双语 README、规范 §2.2 区分目录标签、实现声明、加载位置；不重命名资源，不升级依赖。
- 验证：逐项对照文件头和实际加载路径，检查英中版本、状态一致；仅有目录证据的 Cropbox/TreeTable 明确保留目录标签限定。

### P1-02：日期自动化与请求处理被过度承诺

- 问题与证据：原 §2、§8、§9 写成统一 `molv`、起止联动及标准响应处理。`wwwroot\yisha\js\yisha-init.js:20` 条件是 `.select-time.length > 10`，而实际页面按单组范围单独调用 `laydate.render`；`wwwroot\yisha\js\yisha.js:181` 的 `ajax` 只包装回调、加载和默认错误，不自动判断业务 `Tag`。
- 影响：生成页面可能没有日期约束，或把业务失败当作保存成功并关闭弹窗。
- 建议与落点：规范 §8.7、§9.1 明确独立初始化、显式 `Tag` 检查和待补充防重复提交；§4.3 保留 Laydate/Layer 独立颜色。
- 验证：核对当前日期调用、默认选项、回调覆盖和正反响应分支；起止联动、防重与竞态仅列为新实现要求，不声称源码已有。

### P1-03：移动行为与精确断点不准确

- 问题与证据：原 §10 声称窄屏侧栏默认隐藏、表格收起低优先级列、查询仅在“低于 768px”隐藏。`wwwroot\yisha\js\yisha-index.js:43` 在 `<769` 添加折叠类并 `fadeIn`；`wwwroot\yisha\css\yisha.css:264` 使用 `<=768`；`wwwroot\lib\bootstrap.table\1.12.0\extensions\mobile\bootstrap-table-mobile.js:1` 的阈值为 `562`，`columnsHidden` 默认空数组，而现有平面列表均显式开启移动模式。
- 影响：Agent 会重建不同的窄屏导航/表格，并漏掉筛选不可达、宽屏恢复和 iframe 视口差异。
- 建议与落点：规范 §10.1 建立含精确边界的矩阵，补 `1000/680/562/420px`，区分 CSS 与 JS；§10.2 明确待补可达性。
- 验证：逐项对照媒体查询、脚本条件、页面属性；实际渲染/切换验证留待浏览器执行。

### P1-04：数据绑定、权限与弹层回调缺少可执行契约

- 问题与证据：原 §8、§9 只罗列 helper，未说明 `wwwroot\yisha\js\yisha-plugin.js:352` 的 `[col]` 遍历、生成控件值格式和 `wwwroot\yisha\js\yisha-init.js:114` 只采集 `#toolbar a` / `.toolbar a`；`wwwroot\yisha\js\yisha-init.js:131` 也不是为全部控件生成 name。
- 影响：字段未保存、textarea 未校验、树选择错误、将锚点改按钮后脱离权限采集，以及父页刷新失败。
- 建议与落点：规范 §8.3、§8.5、§8.6、§9.1 明确字段、树路径/叶子值、权限 ID、响应和父级回调；指出 DOM 隐藏不能替代服务端鉴权。
- 验证：对照 User/Role/Menu 表单和 shared ready 顺序，核对服务端路由与虚拟目录；授权实际效果仍需运行验证。

### P1-05：关键状态与恢复路径缺失

- 问题与证据：原 §9 把“每个异步操作具有完整状态”写成统一事实，但 `Views\Home\Welcome.cshtml:256` 不自行检查 `Tag`，服务器页面轮询没有完整失败/释放流程；Cron、导入和头像只有各自局部状态。
- 影响：Agent 会遗漏业务失败、过期结果、重复写入、上传后陈旧路径或关闭页面后的轮询清理。
- 建议与落点：规范 §8.7、§8.9、§8.10、§9.2 为流程逐项描述既有状态和 R 补充要求，明确不改变源码。
- 验证：核对各页面的请求入口、成功/失败、关闭和重试分支；本次仅静态列出真实存在和缺失部分，防重/竞态/清理效果为 U。

### P2-01：壳层、控件尺寸与 CSS 优先级误述

- 问题与证据：原 §6 的固定折叠菜单起点 `65px` 来自通用规则，但 `wwwroot\yisha\css\style.css:553` 的固定侧栏悬停为 `left:50px`；最终 `wwwroot\yisha\css\style.css:6998` 对普通 `#content-main` 使用减 `127px`。`wwwroot\yisha\css\login.css:219` 输入为 `46px`，按钮才为 `48px`。Select2 单选/多选为 `28px/32px` 最小高度，不等同于 Bootstrap 表单。
- 影响：侧栏和 iframe 对不齐，生成所有控件统一高度，登录表单密度失真。
- 建议与落点：规范 §6、§8.3、§8.4 分别定义宿主、插件尺寸与真实表头密度，区分普通选择器和高权重例外。
- 验证：核对声明、选择器、加载顺序和 `!important`；生成 CSS 与输入拼接去注释/空白后相同，最终布局为 U。

### P2-02：皮肤状态、操作映射与局部字体边界错误

- 问题与证据：原 §4、§5、§8 的红/黄顶栏色对含糊，浅蓝选中文字与活动祖先混用，侧栏沿用不匹配标记的 `.sidebar` 色值；将蓝色按钮描述为编辑默认，且把看板字体归入登录字体。证据为 `wwwroot\yisha\css\skins.css:334`、`:487`、`:842`、`wwwroot\yisha\css\style.css:49`、用户列表工具栏和 `Views\Home\Welcome.cshtml:1`。
- 影响：无法区分 active/selected、壳层/内容、默认/局部字体，换肤可能污染 iframe。
- 建议与落点：规范 §4 给完整五皮肤矩阵、深浅状态和全部按钮状态；§5 明确看板继承默认字体；解释未接入 `theme-blue` 和独立插件浮层。
- 验证：匹配实际 DOM 与 CSS 选择器，核对 Cookie 十种选项；没有声明全站深色模式。

### P2-03：专项业务页面仅被笼统列入范围

- 问题与证据：原 §8 仅泛述图表、上传和表单，未描述 `AutoJobForm` Cron 预览、`ServerIndex` 轮询、`UserImport` 两阶段流程、`UserPortrait` 裁剪预览，以及 `MenuForm` 条件字段和图标选择。路径完整列于规范 E15-E18。
- 影响：Agent 只能生成相似外壳，无法正确组合页面状态或维持数据流。
- 建议与落点：规范 §8.7-§8.10 补具体流程、尺寸、字段、状态与边界；普通业务模块也按列表/树表/详情模板映射。
- 验证：按 50 文件矩阵逐项检查宿主、组件、事件和关联保存流程，无未分类 Razor 文件；不虚构页面数量和运行成功。

### P2-04：异常页与源码缺陷未明确区分

- 问题与证据：原文没有说明 `Views\Home\NoPermission.cshtml:1` 的独立布局和 `680px` 单列规则；`Views\Home\Error.cshtml:1` 仅为消息输出。`Areas\SystemManage\Views\Area\AreaIndex.cshtml:73` / `:92` 调用平面表格 API，`:102` 调用未定义搜索函数；区域和任务日志 Controller 有 form action 但没有对应视图。
- 影响：误推断完整错误页系统，或复制无法完成的区域/任务日志编辑流程。
- 建议与落点：规范 §8.10、§14 将无权限真实设计与缺失流程分开，记录错误但不代替源码修复。
- 验证：核对全部视图文件和 Controller 返回入口；浏览器路由和异常响应状态仍为 U。

### P2-05：无障碍、长文本和内容规则缺少现状边界

- 问题与证据：原 §11 有正确建议，但未充分连接 `Views\Shared\_Layout.cshtml:10` 的缩放限制、错误标签绝对定位、锚点命令、`setWebControls` HTML 回填和缺失 iframe 语义。原文也未区分默认时间格式、空值、无限期任务时间和中文 locale 能力。
- 影响：只照抄外观会重复覆盖文字、键盘不可达和不安全内容展示；可能误认为全站已国际化或满足对比度要求。
- 建议与落点：规范 §5.2、§8.3、§11 区分 F 与 R，给出明确内容/焦点/缩放/对比度边界，保留 `NoPermission` 已有正向语义。
- 验证：静态检查原生元素、标签、错误位置、格式化调用和资源；键盘、读屏、200% 缩放、减弱动效、真实对比度和触摸尺寸不冒充已验收。

### P2-06：来源覆盖与资源许可缺少证据链

- 问题与证据：原 README 把 50 个 Razor 文件泛称页面，无逐文件矩阵、准确上游标签或快照指纹；原许可只作总体说明。`bundleconfig.json:1` 含未接入输出，文件头也存在 MIT、Apache、BSD、字体 OFL 等不同授权。
- 影响：无法复查是哪份源码、哪些只是依赖库存，容易把文档 MIT 误读为图片/字体的统一授权。
- 建议与落点：新增本报告与哈希清单，规范 §2 区分 38 个业务视图和其他 12 个文件；分别说明已接入/仅打包/vendor 以及授权未知资源。
- 验证：50 文件全覆盖、41 bundle 结构映射、30 被引用输出、11 未被引用输出；451 个图标均有定义。对未接入 boxed-layout 和 jQuery UI 的缺失资源作局部记录，不报告为活动页面失败。

### P2-07：同版本维护与不可变发布引用需要明确

- 问题与证据：原规范只有首次版本索引；当前用户明确要求在原 `1.0.0` 基础上更新，不新增修订号。若移动标签/覆盖附件，会让历史快照不可追溯；若不解释，维护稿与首次 Release 内容会被误认为一致。
- 影响：Agent 或用户下载到旧附件却认为获得当前更新，或者无法复现原文档。
- 建议与落点：双语 README、规范 §1 和本报告 §6 明确 `main` 是维护稿、`v1.0.0` 为首发快照；下载继续指向原版本目录的 `main`，固定复现则使用提交 SHA。
- 验证：不新增版本目录/标签/Release，不移动旧标签或替换旧附件；新提交为旧提交的后继，推送后核对远端文件和历史引用。

## 4. 覆盖矩阵

### 4.1 模块覆盖

“完整”仅表示本次静态范围内可确认的规则已说明，不代表运行功能完整或 UI 已验收。“部分”表示边界已说明但仍需具体运行/业务证据；“不适用”表示没有实际接入，不能凑清单新增。

| 模块 | 首次文档 | 本次维护稿 | 位置 / 依据 |
| --- | --- | --- | --- |
| 文档用途、读取、Agent 提示 | 部分 | 完整 | README、规范 §1/§13；补非目标和未知契约处理。 |
| 快照来源、文件覆盖 | 缺失 | 完整 | 规范 §2、本报告、manifest；上游标签仍待验证。 |
| 色板与动作语义 | 部分 | 完整 | §4；五类按钮全状态与真实工具栏。 |
| 字体、字号、格式、长文本 | 部分 | 完整 | §5；看板/登录分界，日期、空值、内容安全。 |
| 尺寸、间距、圆角、边框、阴影 | 部分 | 完整 | §6/§7；有效选择器与插件区别。 |
| 层级、加载顺序、动画 | 部分 | 完整 | §2.3/§7.2；iframe 与依赖局部默认。 |
| 固定导航、折叠、iframe 页签 | 部分 | 完整 | §6.1/§8.1；实际按钮和状态。 |
| 面包屑、混合导航、抽屉、全局菜单搜索 | 不适用 | 不适用 | 仅继承代码或完全未接入，不新增。 |
| 五皮肤与深浅侧栏 | 部分 | 完整 | §4.2；十种入口，未接入 theme-blue 单列说明。 |
| 断点与移动状态 | 部分 | 完整（静态） | §10；真实可达性、尺寸切换待运行验证。 |
| 查询、工具栏、按钮 | 部分 | 完整 | §8.2；无通用 Reset，选择保护和权限范围。 |
| 表格、树表、分页 | 部分 | 完整 | §8.4/§8.5；服务端字段、移动卡片、树表键。 |
| 表单、选择、校验、只读详情 | 部分 | 完整 | §8.3/§8.8；绑定、默认值、name 和日志展示。 |
| 弹层、日期、反馈 | 部分 | 完整 | §8.6/§8.7；独立主题、实际初始化和回调。 |
| 导入、头像 | 部分 | 完整 | §8.7；上传/保存两阶段、固定裁剪边界。 |
| Cron 预览、服务器轮询 | 缺失 | 完整 | §8.9；页面现状与补充生命周期要求。 |
| 登录、运营看板 | 部分 | 完整 | §8.10；字体、图表类型、状态、动态资源。 |
| 无权限、异常与缺失视图 | 缺失 | 完整（边界） | §8.10/§14；不声称存在完整 404/500 系统。 |
| 请求、批量、危险操作、恢复 | 部分 | 部分 | §9；已明确缺口，真实服务端行为待验证。 |
| 键盘、焦点、对比度、读屏 | 部分 | 部分 | §11；明确 F/R/U，未进行浏览器或辅助技术验收。 |
| 依赖加载与资源授权 | 部分 | 部分 | §2；本地版本可核实，图片权利/全部构建行为待验证。 |
| 双语、版本、标签、历史发布 | 部分 | 完整 | README、§1、本报告 §6；保持原版本。 |

### 4.2 全部 Razor 文件

路径由“相对目录 + 文件名”组成；每个文件仅出现一次。矩阵以实际文件而非推测路由计数。完整名称/哈希可在 manifest 逐项查找。

| 相对目录 | 文件 | 数量 | 页面 / 组件分类与规范落点 |
| --- | --- | --- | --- |
| `Views\Home` | `Index.cshtml`、`Skin.cshtml` | 2 | 壳层、账号入口、页签、十皮肤选择；§4/§6/§8.1。 |
| `Views\Home` | `Login.cshtml` | 1 | 登录、验证码、记住账号、验证/反馈；§5/§6.3/§8.10。 |
| `Views\Home` | `Welcome.cshtml` | 1 | 六指标、折线/环图、更新时间和空状态；§8.10。 |
| `Views\Home` | `NoPermission.cshtml`、`Error.cshtml` | 2 | 独立无权限与最小错误输出；§8.10/§10。 |
| `Views\Shared` | `_Layout.cshtml`、`_Index.cshtml`、`_Form.cshtml`、`_FormWhite.cshtml`、`_FormGray.cshtml` | 5 | 共享宿主、导入顺序、body/样式边界；§2.3/§8.1。 |
| `Views` | `_ViewImports.cshtml`、`_ViewStart.cshtml` | 2 | Razor 导入及默认布局，非业务页面；§2/§8.1。 |
| `Areas\OrganizationManage` | `_ViewImports.cshtml` | 1 | 组织区域 Razor 上下文；§2。 |
| `Areas\SystemManage` | `_ViewImports.cshtml` | 1 | 系统区域 Razor 上下文；§2。 |
| `Areas\ToolManage` | `_ViewImports.cshtml` | 1 | 工具区域 Razor 上下文；§2。 |
| `Areas\OrganizationManage\Views\Department` | `DepartmentIndex.cshtml`、`DepartmentForm.cshtml` | 2 | 树表、父部门选择、保存和树表刷新；§8.5/§8.8。 |
| `Areas\OrganizationManage\Views\Position` | `PositionIndex.cshtml`、`PositionForm.cshtml` | 2 | 平面列表、状态、排序/校验；§8.2-§8.4/§8.8。 |
| `Areas\OrganizationManage\Views\User` | `UserIndex.cshtml` | 1 | 部门分栏、用户表格、筛选、批量、导入/导出；§8.2/§8.4/§8.5。 |
| `Areas\OrganizationManage\Views\User` | `UserForm.cshtml`、`UserDetail.cshtml`、`ChangeUser.cshtml` | 3 | 编辑/详情/个人资料、部门职位角色、回填；§8.3/§8.8。 |
| `Areas\OrganizationManage\Views\User` | `ChangePassword.cshtml`、`ResetPassword.cshtml` | 2 | 自助修改与管理重置密码、校验/回调；§8.3/§8.8。 |
| `Areas\OrganizationManage\Views\User` | `UserImport.cshtml`、`UserPortrait.cshtml` | 2 | Excel 两阶段导入和头像裁剪/保存；§8.7。 |
| `Areas\SystemManage\Views\Area` | `AreaIndex.cshtml` | 1 | 区域树表；不完整编辑/删除路径列为源码限制；§8.5/§14。 |
| `Areas\SystemManage\Shared` | `AreaIndexPartial.cshtml`、`AreaFormPartial.cshtml` | 2 | 行政区域树选择局部视图；§8.3/§8.5。 |
| `Areas\SystemManage\Views\AutoJob` | `AutoJobIndex.cshtml`、`AutoJobForm.cshtml` | 2 | 任务列表/操作、日期、Cron 预设和预览；§8.9。 |
| `Areas\SystemManage\Views\AutoJobLog` | `AutoJobLogIndex.cshtml` | 1 | 任务日志与列表上下文；缺失 form 独立记录；§8.8/§14。 |
| `Areas\SystemManage\Views\DataDict` | `DataDictIndex.cshtml`、`DataDictForm.cshtml` | 2 | 字典父级列表、表单和明细入口；§8.8。 |
| `Areas\SystemManage\Views\DataDictDetail` | `DataDictDetailIndex.cshtml`、`DataDictDetailForm.cshtml` | 2 | 字典 ID 上下文、明细和值/状态；§8.8。 |
| `Areas\SystemManage\Views\LogApi` | `LogApiIndex.cshtml`、`LogApiDetail.cshtml` | 2 | API 日志、日期、只读载荷；§8.4/§8.8。 |
| `Areas\SystemManage\Views\LogLogin` | `LogLoginIndex.cshtml` | 1 | 登录日志、筛选和状态；§8.4/§8.8。 |
| `Areas\SystemManage\Views\LogOperate` | `LogOperateIndex.cshtml`、`LogOperateDetail.cshtml` | 2 | 操作日志、只读请求/结果；§8.4/§8.8。 |
| `Areas\SystemManage\Views\Menu` | `MenuIndex.cshtml`、`MenuForm.cshtml`、`MenuChoose.cshtml`、`MenuIcon.cshtml` | 4 | 菜单树表、类型字段切换、权限树和图标；§8.5/§8.8。 |
| `Areas\SystemManage\Views\Role` | `RoleIndex.cshtml`、`RoleForm.cshtml` | 2 | 角色表格、菜单权限勾选和保存；§8.4/§8.5/§8.8。 |
| `Areas\ToolManage\Views\Server` | `ServerIndex.cshtml` | 1 | 资源/属性面板、轮询和折叠/关闭；§8.9。 |
| 合计 | 无未分类 Razor 文件 | 50 | 38 个业务视图 + 5 个布局 + 5 个上下文 + 2 个局部视图。 |

### 4.3 依赖与资源分类

| 类别 | 实际范围 | 处理 |
| --- | --- | --- |
| 活动基础与组件 | jQuery、Bootstrap 3、Font Awesome、Layer、Laydate、Table、TreeTable、zTree、Select2、iCheck、Validation、File Input、Cropbox、ECharts、Layout、MetisMenu、SlimScroll | 明确实现版本、宿主和样式/事件边界；基础 jQuery bundle 内含三个工具依赖。 |
| 仅打包 | SmartWizard、jQuery UI、Highlight、Image Upload、Peity、Tags Input，共 11 个输出 | 不虚构页面；新增接入前重新核对 API、资源和许可。 |
| 仅 vendored | Bootstrap 4、Summernote、Lightbox2、context-menu、未加载 table 扩展、其他 locale | 不属于当前页面视觉契约。 |
| 活动图片/字体/模板 | 登录背景、头像、站点图标、Font Awesome/Glyphicons、iCheck/zTree/Laydate 资源、用户导入模板 | 验证本地路径，记录哈希；不复制内容，不扩大授权。 |
| 继承但未启用样式 | timeline、boxed-layout、top-navigation、right-sidebar、RTL 等 | 不套用到默认壳层；缺失资源不等于活动页面故障。 |

## 5. 实施文件与优先级

按用户最新要求直接实施原版本更新，不新增规范版本，也不升级业务依赖。

| 优先级 | 文件 | 具体修改 | 状态 |
| --- | --- | --- | --- |
| P1 | `DESIGN.md` | §2 版本/来源；§8/§9 绑定、权限、请求/状态、日期和流程；§10 实际断点 | 已补充。 |
| P1 | `DESIGN.zh-CN.md` | 同步章节编号、令牌、版本、边界、契约和建议语义 | 已同步。 |
| P2 | 双语 README | 38/50 区分、实际依赖版本、用途/非目标、当前维护稿/首发快照、审计入口、提示词 | 已更新。 |
| P2 | `DESIGN.md` / `DESIGN.zh-CN.md` | §4-§7 主题/尺寸/字体/层级；§8 专项页；§11/§14 已知限制 | 已补充。 |
| P2 | `AUDIT.md` | 具体问题、证据、影响、建议、验证、模块与文件矩阵、版本/验收策略 | 本报告。 |
| P2 | `SOURCE-MANIFEST.json` | 219 文件 SHA-256、50 Razor 分类、41 bundle 输入/输出与引用 | 已生成。 |
| 保持 | `LICENSE`、上游源码、旧标签/Release 附件 | 不改变许可主体，不修改源码，不改历史发布 | 不纳入修改范围。 |

## 6. 版本与推送策略

规范 front matter、目录、首页徽章、提示词仍使用 `1.0.0`。本次属于同一规范的原位文档维护，不代表新增上游版本、更新插件或声明对应上游 tag。

只向 `main` 追加正常后继提交，用 Git 历史识别修订；不强推、不新增 PR、不创建其他版本目录、标签或 Release。首次发布 `v1.0.0` 继续指向原提交，其 Release 名称 `YiSha DESIGN.md v1.0.0` 和两份附件不变。新维护稿与旧附件内容不同是有意的，并在双语入口说明。

原附件 SHA-256 基线如下，发布后用于只读复核，不上传覆盖：

| 附件 | SHA-256 |
| --- | --- |
| `DESIGN.md` | `3b890f941ebd7cfb830e7412fd3fc94ff9b8f13441927846a8bdd837bba21e8d` |
| `DESIGN.zh-CN.md` | `98642dc51e23aa266d416f18a4e428d29713d06aa41495b7199019e780d1d670` |

如 Git HTTPS 传输超时而 GitHub API 可用，只允许发布与本地同一 tree、同一 parent 的普通后继提交，并校验对象后同步引用；不得覆盖别人的远端进展。仓库中英描述维持现有内容。

## 7. 验收标准与范围

### 7.1 文档检查

- 所有跟踪 Markdown 严格 UTF-8 解码，完整解析 YAML/Markdown；规范元数据必需字段存在且版本为 `1.0.0`。
- 双语章节编号/顺序、表格结构、颜色、尺寸、依赖版本、代码标识、证据 ID、事实/建议/未验证边界对应，进行语义复核，不只比关键词。
- 本地相对链接和锚点有效；源码相对路径/行号能在清单快照定位；没有本机绝对路径、用户禁用品牌词、临时产物或源码副本。
- 只存在 `versions/1.0.0`；不混入其他文档仓库改动，LICENSE 不变。

### 7.2 源码核对

- 清单 50 个 Razor 文件与矩阵逐个一致、无重复或未分类；41 个 bundle 输入/输出、30 个被视图引用输出及 11 个未引用输出可复查。
- 依赖版本按实现头核对，重点复核 Select2/File Input 目录差异、zTree/ECharts 版本、表格移动扩展默认值。
- 核对有效颜色/尺寸/断点/层级、初始化条件、URL/权限/字段/回调和页面状态。219 文件哈希在提交前重算，防止审计期间源码漂移。
- 直接视图资源无缺失，451 个图标类均有定义；样式 bundle 的规范化拼接一致，自有 JS 语法检查通过。未启用模板资源缺失单独记录。

### 7.3 推送后只读检查

- `main` 的远端 tree/commit 与本地一致，远端文件内容哈希一致，工作区干净；仓库仍公开、默认分支不变、描述仍中英双语。
- GitHub README 渲染、双语入口、维护稿/首发快照链接正确；若仅用 Markdown 渲染接口，明确它不是浏览器验收。
- `v1.0.0` 仍指向原提交，历史 Release 和附件哈希不变，没有新增版本标签或 Release。

### 7.4 未执行的运行验证

没有安装依赖、启动 Web/数据库或运行业务页面。以下在真正修改 UI 时再安排：十种皮肤的实际层叠；各断点及相邻宽度的导航/筛选/账号可达性；iframe 高度和浮层定位；表格卡片切换后的选择/动作；日期先后、Cron 竞态、上传恢复、重复提交；页面卸载清理；真实权限与接口错误；键盘、焦点返回、读屏、缩放、对比度、减弱动效和触摸目标。

不能由本次静态检查推导出上述验证已通过，也不能由源码文件存在推导出服务端数据或资源许可已验证。

## 8. 无需改动的部分

- 三层视觉定位正确：默认蓝/深色壳层、青绿 CRUD、深绿登录/看板有实际源码支持，继续保留。
- Bootstrap `3.3.7`、jQuery `2.1.4`、Font Awesome `4.7.0`、Layer `3.1.1`、Laydate `5.0.9`、Table `1.12.0` 的原版本描述与文件头相符，不升级。
- 已有语言切换、MIT 许可主体、仓库公开性、默认 main、双语描述和首发标签/附件约定有效，保留其身份和历史。
- 原无障碍方向并无要求复制禁用缩放等问题；本次增加现状证据和验收边界，不撤回改进原则。
