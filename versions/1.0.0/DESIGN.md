---
version: "1.0.0"
name: "YiSha Design System"
description: "Source-derived YiSha interface rules with explicit evidence, integration contracts, and verification boundaries."
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

# YiSha Design System

[English](DESIGN.md) | [简体中文](DESIGN.zh-CN.md) | [Audit](AUDIT.md) | [Source Manifest](SOURCE-MANIFEST.json)

## 1. Overview

### 1.1 Purpose and use

Use this specification to generate or review compact YiSha administration lists, forms, trees, detail screens, the iframe shell, authentication, and operational overview pages. It records visual composition and the integration contracts that keep visually similar controls from behaving differently.

Select a version, read this complete file, identify the nearest page pattern in section 8, and preserve the host application's routes, permissions, response fields, and callbacks. For a standalone visual implementation, retain these patterns but explicitly mark missing business contracts; do not invent working endpoints or permission rules.

The English edition is canonical; the Chinese edition has equivalent rules. `1.0.0` identifies this documentation, not an upstream YiSha version. This audit updates the existing `versions/1.0.0` files on `main` without upgrading dependencies or changing the version number. The existing `v1.0.0` tag and Release attachments remain the first-release snapshot. Use Git history and a pinned commit to identify an exact documentation revision; do not assume those historical attachments contain this update.

### 1.2 Evidence vocabulary and non-goals

- **F (Source fact):** present in an audited view, first-party implementation, or configuration.
- **D (Dependency behavior):** inherited from the vendored version, subject to page options and the CSS cascade.
- **R (Required improvement for new work):** a recommendation in this specification, not a claim that legacy pages already implement it.
- **U (Unverified):** requires runtime, deployment, or provenance evidence not available in this static audit.

Unmarked source descriptions below are F or D as identified by their evidence reference. Imperatives preserve those contracts; additions beyond the source carry R. This document does not replace backend/API documentation, provide application source, certify accessibility or security, or define a new component library. No business system, database, or browser UI was run for this release.

## 2. Source Baseline

### 2.1 Snapshot and traceability

The baseline is the supplied YiSha ASP.NET Core MVC Web snapshot. It has no local Git metadata identifying an upstream tag. The [upstream project](https://github.com/liukuo362573/YiShaAdmin) is attribution, not proof that its current default branch matches this snapshot. Do not mix in upstream-latest behavior.

The static inventory contains 50 Razor files: 38 application views, 5 shared layout files, 5 Razor context files, and 2 area partials. It also records 17 Web controllers for routing/data contracts, all first-party CSS/JS, 41 bundle definitions, their inputs/outputs, and directly referenced resources. The manifest identifies 219 files by relative path, role, byte length, and SHA-256; it does not redistribute contents. It cannot prove byte identity with the undocumented source snapshot used for `1.0.0`.

Evidence paths below are relative to the supplied Web root, not links to files in this documentation repository. A `:number` suffix is a one-based line in the captured snapshot. The audit contains the complete Razor coverage matrix.

| Evidence | Relative source path and responsibility |
| --- | --- |
| E01 | `Views\Shared\_Layout.cshtml:1`, `_Index.cshtml:1`, `_Form.cshtml:1`, `_FormWhite.cshtml:1`, `_FormGray.cshtml:1`: hosts, imports, body classes, load order. |
| E02 | `wwwroot\yisha\css\style.css:1`: base typography, controls, shell and inherited template styles. |
| E03 | `wwwroot\yisha\css\skins.css:7`: skin selectors and sidebar themes. |
| E04 | `wwwroot\yisha\css\yisha.css:1`: final shared overrides, lists, validation, selection and loaders. |
| E05 | `wwwroot\yisha\css\login.css:5`, `Views\Home\Login.cshtml:1`: sign-in presentation and flow. |
| E06 | `Views\Home\Index.cshtml:19`, `wwwroot\yisha\js\yisha-index.js:1`: shell markup and tab management. |
| E07 | `Views\Home\Skin.cshtml:1`: ten selectable skin/sidebar combinations. |
| E08 | `wwwroot\yisha\js\yisha.js:6`: dialogs, requests, messages, loading, selection and export helpers. |
| E09 | `wwwroot\yisha\js\yisha-plugin.js:1`: data binding, selects, tree selects and choice groups. |
| E10 | `wwwroot\yisha\js\yisha-init.js:1`: shared ready handlers, toolbar authority and validation names. |
| E11 | `wwwroot\yisha\js\yisha-jquery-bootstrap-table-plugin.js:16`: grid contract. |
| E12 | `wwwroot\yisha\js\yisha-jquery-bootstrap-treetable-plugin.js:1`, `yisha-jquery-ztree-plugin.js:1`: hierarchy adapters. |
| E13 | `Areas\OrganizationManage\Views\User\UserIndex.cshtml:1`: split tree/list, filters, toolbar, CRUD, import/export. |
| E14 | `Areas\OrganizationManage\Views\User\UserForm.cshtml:1`, `ChangeUser.cshtml:1`, `UserDetail.cshtml:1`, `ChangePassword.cshtml:1`, `ResetPassword.cshtml:1`: account forms. |
| E15 | `Areas\OrganizationManage\Views\User\UserImport.cshtml:1`, `UserPortrait.cshtml:1`: upload and crop workflows. |
| E16 | `Areas\SystemManage\Views\Menu\MenuForm.cshtml:1`, `MenuChoose.cshtml:1`, `MenuIcon.cshtml:1`, `Areas\SystemManage\Views\Role\RoleForm.cshtml:1`: menu/permission editing. |
| E17 | `Areas\SystemManage\Views\AutoJob\AutoJobForm.cshtml:1`, `AutoJobIndex.cshtml:1`: job configuration and Cron preview. |
| E18 | `Views\Home\Welcome.cshtml:1`, `Areas\ToolManage\Views\Server\ServerIndex.cshtml:1`: dashboard and server monitor. |
| E19 | `Views\Home\NoPermission.cshtml:1`, `Views\Home\Error.cshtml:1`: exception boundaries. |
| E20 | `bundleconfig.json:1`: bundle inventory; implementation versions are in the corresponding input file headers. |
| E21 | `wwwroot\lib\bootstrap.table\1.12.0\extensions\mobile\bootstrap-table-mobile.js:1`: mobile card-view switching. |
| E22 | `wwwroot\lib\layer\3.1.1\theme\default\layer.css:73`, `wwwroot\lib\laydate\5.0.9\theme\default\laydate.css:2`: independent popup styles. |
| E23 | `Startup.cs:128`, `appsettings.json:1`, Web controllers listed in the manifest: area routes and virtual-directory context. |
| E24 | `Areas\SystemManage\Views\Area\AreaIndex.cshtml:92`, `Areas\SystemManage\Controllers\AutoJobLogController.cs:1`: incomplete source workflows. |

### 2.2 Actual dependency boundary

Directory labels are not authoritative versions. Keep the existing asset paths, but use the declared implementation version when reasoning about behavior. This table is not a dependency-upgrade request. E01/E20 and the paths below are the evidence.

| Capability | Audited implementation | Loading and boundary |
| --- | --- | --- |
| DOM | jQuery `2.1.4` | Shared; its bundle also includes BlockUI `2.7`, Cookie `1.4.1`, Fullscreen `1.2`. |
| Grid and base controls | Bootstrap `3.3.7` | Shared 12-column grid; JS bundle also includes `bootstrap.dropdown.js`. Do not load Bootstrap `4.0.0` from the vendor directory. |
| Application icons | Font Awesome `4.7.0` | Shared `fa` classes. Bootstrap Glyphicons remain a dependency exception, including the sign-in checkbox glyph. |
| Dialog/message | Layer `3.1.1` | Shared desktop implementation behind `ys.*`; mobile sizing in the helper does not load Layer's separate mobile implementation. |
| Date/time | Laydate `5.0.9` | List/form hosts; page-specific `laydate.render`. No blanket `molv` theme or automatic range linkage. |
| Record grid | Bootstrap Table `1.12.0` | List host; bundle includes mobile extension, Chinese locale and `ysTable`. |
| Hierarchical grid | Bootstrap TreeTable directory `1.0` | Department, menu and area pages; bundle includes `ysTreeTable`. |
| Hierarchical selection | zTree `3.5.18` | `wwwroot\lib\zTree\v3\js\jquery.ztree.all-3.5.js:3`; Metro theme, form host and user list; bundle includes `ysTree`. |
| Enhanced select | Select2 `4.0.7` | File header at `wwwroot\lib\select2\4.0.6\js\select2.js:2`; directory says `4.0.6`. `ysComboBox` uses Select2 even for ordinary single selection. |
| Checkbox/radio | iCheck `1.0.2` | Form host, `icheckbox-blue` / `iradio-blue`. Not the login checkbox implementation. |
| Validation | jQuery Validation `1.14.0` | Form host and login; extension methods and Chinese messages are bundled. |
| File import | File Input `5.0.4` | Header at `wwwroot\lib\fileinput\5.0.3\js\fileinput.js:2`; directory says `5.0.3`. Only the user import page loads it. |
| Portrait | Cropbox directory `1.0` | Only the portrait page; do not infer an upstream tag from the directory alone. |
| Charts | ECharts `4.5.0` | `wwwroot\lib\report\echarts\echarts.js:27406`; dashboard bundle also includes `china.js`, but the audited dashboard has no map. |
| Split panes | jQuery Layout `1.4.4` | User list's department pane, not a universal shell layout. |
| Shell navigation | MetisMenu `1.1.3`, SlimScroll `1.3.8` | Shared shell imports; `4px` scroll rail. |

Of 41 configured bundle outputs, 30 are referenced by views and 11 are not. The latter cover SmartWizard `4.0.1`, jQuery UI `1.12.1`, Highlight `9.13.1`, Image Upload directory `1.0`, Peity `3.3.0`, and Tags Input `0.8.0`. They are not evidence of existing wizard, editor, tag-entry, or miniature-chart pages. Bootstrap `4.0.0`, Summernote, Lightbox2, jQuery context-menu, additional table extensions and extra locale files are vendored, not wired into the audited pages.

### 2.3 Load order, scope and assets

E01/E20: base Bootstrap and Font Awesome precede the first-party style bundle; plugin/page styles follow where their host renders them; final `yisha.css` overrides are emitted near the end of the body. `style.min.css` concatenates `animate.css`, `style.css`, then `skins.css` with minification disabled. Their comment/whitespace-normalized contents match the supplied bundle. This is not proof of runtime equivalence for every minified JS bundle.

The bundling helper emits inputs in debug builds and the configured output otherwise. Do not include an adapter again when its table/tree bundle already contains it. Page ready handlers are registered before the final shared initializer; initialization order matters for populated selects and generated names. A parent body's skin classes do not cross an iframe boundary. Popups belong to the document where their plugin is invoked.

E05/E15/E20: retain the existing login bitmap at `wwwroot\image\login-background.jpg`, avatar assets, local Font Awesome/Glyphicons font files, iCheck sprites, zTree images, Laydate font and import workbook when working in the host project. The documentation does not include those assets. Font Awesome fonts use SIL OFL `1.1`, its CSS uses MIT; Bootstrap uses MIT; ECharts uses Apache `2.0`; File Input uses BSD-3-Clause. Check each distributed header/license before redistribution. Image, avatar and site-icon rights were not independently established. This repository's MIT license covers its original documentation, not blanket rights to upstream media or plugins.

## 3. Design Principles

### 3.1 Three visual layers

Keep the default blue header/dark sidebar, teal CRUD content, and deep-green authentication/dashboard accents distinct. A selected shell skin is not a recoloring instruction for buttons inside iframes. `theme-dark` means a dark sidebar, not a full dark mode. Authentication, dashboard and permission-denied screens are scoped exceptions to the legacy density.

### 3.2 Dense operational composition

Prefer compact tables, filters, adjacent toolbars and horizontal forms for repeated administration. Preserve the existing search/table surfaces, but do not introduce more nested cards, decorative metrics, oversized headings or a marketing layout into CRUD work. Use structure, borders and spacing before adding color or elevation.

### 3.3 Contract before appearance

Start from the nearest host and adapter. Preserve control IDs, `col` attributes, permission identifiers, enum values, URL context and parent callbacks. Do not replace jQuery components with visually similar modern controls unless the application migration is explicitly in scope. R: repair semantics and missing state recovery without treating every legacy defect as a visual requirement.

## 4. Color System and Themes

### 4.1 Content roles and button states

E02: classes have project-specific meanings. In the user, role and position toolbars, Add is blue `.btn-success`; Edit and Search are teal `.btn-primary`. Export is often `.btn-warning`, Import `.btn-info`, Delete `.btn-danger`. Choose the nearest page's action mapping, not modern Bootstrap naming assumptions.

| Class / token | Default | Hover, focus, active | Disabled fill |
| --- | --- | --- | --- |
| `.btn-primary` / `content_primary` | `#1ab394` | `#18a689` | `#1dc5a3` |
| `.btn-success` / `action_secondary` | `#1c84c6` | `#1a7bb9` | `#1f90d8` |
| `.btn-info` / `info` | `#23c6c8` | `#21b9bb` | `#26d7d9` |
| `.btn-warning` / `warning` | `#f8ac59` | `#f7a54a` | `#f9b66d` |
| `.btn-danger` / `danger` | `#ed5565` | `#ec4758` | `#ef6776` |

Bootstrap also applies disabled opacity. `.btn-white` is a white, bordered utility; `.btn-outline` is transparent until hover. These states are styling, not an authorization or duplicate-submission guard.

Content text is `#676a6c`, strong filter/table text `#333333`, muted text commonly `#999999`, page canvas `#f3f3f4`, white surface `#ffffff`, divider `#e7eaec`, table header divider `#cccccc`, normal input border `#e5e6e7`. Search inputs use `#dddddd`. E04 validation uses background `#fbe2e2`, border `#c66161`, entered text `#cc0000`, and error-label text `#ef392b`.

### 4.2 Shell skin matrix

E03/E06/E07: default `skin-blue theme-dark`. The `Skin` cookie stores `skin-name|theme-name` for 365 days with path `/`. The picker exposes five skins times two sidebar themes.

| Skin | Header | Logo | Selected fill in dark sidebar | Selected background / text in light sidebar |
| --- | --- | --- | --- | --- |
| `skin-blue` | `#3c8dbc` | `#367fa9` | `#1890ff` | `#f0f5ff` / `#2f54eb` |
| `skin-green` | `#00a65a` | `#008d4c` | `#52c41a` | `#f6ffed` / `#52c41a` |
| `skin-purple` | `#605ca8` | `#555299` | `#722ed1` | `#f9f0ff` / `#722ed1` |
| `skin-red` | `#dd4b39` | `#d73925` | `#f5222d` | `#fff1f0` / `#f5222d` |
| `skin-yellow` | `#f39c12` | `#e08e0b` | `#faad14` | `#fffbe6` / `#faad14` |

Dark sidebar canvas is `#2f4050`, active ancestry/hover `#293846`, normal `.nav > li > a` text `#a7b1c2`, and selected text white. The inherited `.sidebar a` color `#b8c7ce` does not describe the current `navbar-static-side` markup. Active top-level ancestry uses a `3px` skin-colored border; `.selected` is a distinct state. Light sidebar is `#f9fafc`, ordinary text `#777777`; blue active ancestry text is `#1890ff`, different from selected text `#2f54eb`. Light hover is pale blue even for other skins due to the common selector.

The stylesheet also contains `theme-blue` with background `rgba(15,41,80,1)` and text `#a3b1cc`; the picker does not expose it. Treat it as dormant inherited capability, not an eleventh selectable theme. Do not promise that changing a skin recolors Layer, Select2, iCheck, Laydate or ECharts.

### 4.3 Scoped colors and cascade

E05/E18/E19: login/dashboard accent `#0f8b72`; login ink `#17202a`, gold `#f4bd3f`, support blue `#2f6fed`; dashboard ink `#17211d`, canvas `#eef2f1`, border `#dce4e0`, time accent `#f6c453`. The permission page uses ink `#18222f` and gold `#f3c04d`. These are not universal content tokens.

E22: Layer's default confirmation button is `#1e9fff`; Laydate's selected day is `#009688`. E04 overrides Select2 multiple chips to `#1ab394`. E02 `.form-control:focus` sets a teal border with `!important`, so login's later deep-green focus rule does not win that border; its green focus shadow still applies. Judge precedence using importance, selector specificity, load order, and document scope, not the last color mentioned in a file.

## 5. Typography and Content

### 5.1 Source scales

E02: `font_family` from the front matter, `12px`, weight `400`, Bootstrap-derived line-height approximately `1.42857`. No webfont download for Open Sans was found in the active layout; it is a fallback name. Ordinary form labels are normal weight. Sidebar links are `13px` / `600`; nested collapsed links `12px`. The shell logo has its own Helvetica-first stack and `16px` size.

Labels are `10px` with `3px 8px` padding; badges are `11px` with `4px 6px` padding. Ibox titles are around `14px`, `.box-main` titles `16px`, generic box titles `18px`. The `8px`-radius dashboard uses `28px` metric values but still inherits the default font stack, not the login stack.

E05: login alone uses `modern_font_family`; brand `42px`, supporting headline `30px`, switching to `34px` / `24px` at `880px`. E19 permission title is `30px`, then `24px` at `680px`; its system-first stack omits the explicit Arial fallback. Keep letter spacing `0`. Do not scale fonts continuously with viewport width.

### 5.2 Content and formatting contracts

E13/E17/E18: list dates use `yyyy-MM-dd`; displayed record timestamps and job datetime inputs use `yyyy-MM-dd HH:mm:ss`. Preserve string IDs, enum-backed status values and response field names. Display status text as well as a badge color. The dashboard's missing update time is `--`; a job's indefinite end sentinel is `9999-12-31 00:00:00`, not a normal user-facing expiry date.

R: distinguish zero, unavailable data and a failed request; do not coerce them all to `0`. Preserve server units/timezone unless the contract supplies conversion rules. Use concise task labels, retain entered text on errors, and expose full long values through wrapping, a detail view or an accessible disclosure. Do not add localization claims: Chinese table/validation/file-input messages are bundled, but a complete application language switch and timezone/number-format policy are not implemented.

## 6. Layout and Spacing

### 6.1 Shell geometry

E01/E02/E06; dimensions describe the actual `fixed-sidebar` host, not every inherited template class.

| Element | Source dimension / behavior |
| --- | --- |
| Expanded navigation | `200px` fixed width; desktop page left margin `200px`. |
| Collapsed navigation | `50px` width and page offset. Not `65px`. |
| Collapsed hover flyout | `left:50px`; label at the top, second-level menu `top:40px`, minimum width `140px`. |
| Header / logo | `50px`; preserve compact account and collapse controls. |
| Tab strip | `42px`; buttons/tabs `40px` high. |
| Page wrapper | `0 15px` padding; `.wrapper-content` commonly `20px`. |
| Iframe content host | Final ordinary `#content-main` rule: `height:calc(100% - 127px); overflow:hidden`. |
| User list split pane | jQuery Layout west pane `185px`, separate from the shell sidebar. |

The earlier `.mini-navbar li.active .nav-second-level { left:65px; }` is superseded for the fixed-sidebar hover case. Likewise, earlier `#content-main {height:100%}` is not the final ordinary value. More-specific inherited `.fixed-nav` rules still have their own heights, but this is not the default body class. R: verify actual header/tab/iframe bounds when changing the host; do not copy the `127px` subtraction into unrelated layouts.

### 6.2 Lists and forms

E04: `.container-div` uses `10px 35px`; `.search-collapse` / `.select-table` use white, `6px` radius, `10px` top margin, `5px` top / `13px` bottom padding and `1px 1px 3px rgba(0,0,0,.2)` shadow. Filters have `30px` rows, `5px` vertical margins, `15px` right gaps, `280px` ordinary inputs/selects, and `133px` date endpoints. Actual enhanced-select height remains plugin-specific.

E01/E02: use `.form-horizontal`, `.form-group`, `.control-label` and Bootstrap `col-sm-*` splits such as `3 + 8` or `2 + 10`. Standard inputs inherit `34px` height and `6px 12px` padding, with `12px` text and `1px` radius. Forms commonly use `15px` row gaps and `15-20px` surrounding padding. Save/Close belong to the host Layer row or the page's existing form actions.

### 6.3 Scoped page geometry

E05: login shell is at most `960px`, with a `386px` card, `30px` padding, `8px` radius; fields and captcha are `46px` high, submit button `48px`. It uses the real background bitmap with CSS overlays. Reproducing this exception is not permission to add gradients or hero layouts to CRUD pages.

E18: dashboard padding `18px`, gaps `12-14px`, metric cards at least `122px`, panel minimum `322px`, chart height `252px`, metric icon `34px`. E19: permission surface at most `760px`, columns `132px minmax(0,1fr)`, gap `34px`, padding `42px`, radius `8px`. Its explicit content min-width prevents the text column from forcing the grid wider.

## 7. Shape, Elevation and Motion

### 7.1 Radius, borders and shadows

E02/E04/E05/E18/E19: use `1px` input corners, `2-3px` compact controls/dropdowns/loaders, `4px` filters and Bootstrap details, `6px` search/table surfaces and Cron helper, and `8px` scoped login/dashboard/permission surfaces. Avatars are circular. Preserve existing exceptions rather than globally rounding every component.

| Surface | Source treatment |
| --- | --- |
| Input, panel, progress | Mostly flat; shared rules suppress default shadows. |
| `.box` | `3px` top border `#d2d6de`, shadow `0 1px 1px rgba(0,0,0,.1)`; `.box-main` removes both framing effects. |
| Dropdown | `0 0 3px rgba(86,96,117,.3)`; compact border and padding. |
| Dashboard | `0 12px 28px rgba(23,33,29,.07)`. |
| Login | `0 28px 70px rgba(23,32,42,.22)`. |
| Permission denied | `0 26px 70px rgba(24,34,47,.16)`. |
| Laydate | `0 2px 4px rgba(0,0,0,.12)`, `2px` corners. |

### 7.2 Stacking and motion boundaries

E02/E04/E08/E22: tree-select mask/panel `99/101`; Select2 dropdown `1051`; fixed sidebar `2001`; Layer JS default base `19891014` plus its index; Laydate CSS `66666666`. These are document-local values, not a coherent global token scale. An iframe cannot escape its parent stacking context by increasing an inner z-index. Diagnose the owning document before changing stacking or `dropdownParent`.

Source motion includes `150ms` input-border transitions, `300ms` logo width, tab scrolling with jQuery animation, a `500ms` menu fade, `200ms` server-panel collapse and a `400ms` infinite loading spinner. E05/E19 also have short hover transitions; inherited animation classes are not requirements for every page. R: preserve layout dimensions during state changes, provide reduced-motion alternatives and readable loading text, and avoid adding ornamental loops or forcing animation on all transitions.

## 8. Components and Page Patterns

### 8.1 Hosts, navigation and tabs

E01/E06: `_Layout` hosts shell/login; `_Index` hosts lists and the dashboard; `_FormWhite` / `_FormGray` wrap `_Form` for transactional pages. `NoPermission` has no shared layout. Never duplicate the application header/sidebar/tab manager in an iframe page.

The shell has nested menus, collapse control, scroll rail, account dropdown and URL-keyed iframe tabs. Preserve the initial home tab, reuse an existing URL, synchronize active ancestry and selected menu, and keep only the active iframe visible. Previous/next tab scrolling, refresh, close-current, close-other and close-all are established commands. Account actions include profile, password, portrait/identity context, skin selection and sign-out; use real controller actions rather than illustrative links.

The account image is `27px`, dropdown width `138px`. Sidebar second/third-level label indents are `52px` / `62px` when expanded. Menu data and authority are supplied by the host; this is not a client SPA router. No current view wires a global breadcrumb bar, sidebar search, mixed top/side layout, right drawer, chat, timeline, rich-text editor or wizard. Inherited CSS or a vendor directory alone does not establish such a component.

### 8.2 Search, toolbar and buttons

E10/E11/E13: filters bind through `col` fields in `#searchDiv`; Enter triggers `#btnSearch`. Search refreshes page `1` and calls `resetToolbarStatus()`. Add/Edit/Delete/Import/Export sit directly above the grid; `.btn-group-sm` / `.btn-sm` are toolbar density and `.btn-xs` is row-action density. The audited lists do not implement a universal Reset-filter command. R: add one only when requested, resetting both widgets and hidden filter values before searching.

Selection events toggle `.disabled` on Delete for zero rows and Edit unless exactly one row is selected. `ys.checkRowEdit` / `ys.checkRowDelete` must still guard the handler; CSS-only disabled appearance is insufficient. Bulk delete confirms the selected count, passes comma-separated IDs, then refreshes on success. R: guard re-entry while pending, retain selection/filters where appropriate after failure, and ensure empty pages after deletion recover to a valid page.

### 8.3 Fields, selects, choices and validation

E09/E10/E14: `getWebControls` reads descendants with `[col]`, not every form field and not ordinary form serialization. `setWebControls` populates controls, but uses HTML insertion for some `DIV`/`SPAN` values. Retain exact `col` field names. Shared code copies IDs to `name` only for text/password/radio inputs and selects; other validated inputs need explicit names. R: encode untrusted display values and give textareas, generated widgets and validation errors explicit associations.

`ysComboBox` generates `id_select` and initializes Select2 for single or multiple selection. Its query option uses `-1` for All; form placeholder uses an empty value. Preserve the configured `dataName`, value/text fields and comma-separated selection representation. Select2 single selection is `28px`; multiple selection minimum `32px`, with wrapping chips. Its ordinary CSS has a `#aaaaaa` border, disabled gray surface and plugin focus states; only selected chip styling is overridden by E04. Do not enforce one global `30px` or `34px` height across all selects.

Choice groups use local helpers and iCheck's blue checked/disabled sprites; source widths and events differ from native checkbox styling. R: retain native labels, checked/disabled semantics and keyboard behavior, and expose indeterminate state if the business contract needs it. Do not substitute a toggle for every enum or radio group.

Validation error labels are absolutely positioned at `right:18px; top:7px; font-size:12px`; grouped choices use a separate offset. That is a source fact, not a guarantee that long errors fit. R: keep messages near fields without covering values, allow wrapping on narrow screens, validate generated selects on change, and keep focus on the first invalid field. Required markers are red and adjacent to the label.

### 8.4 Record tables and pagination

E11/E13/E21: use `ysTable`, not a replacement grid. Defaults: GET, server pagination, `Id` descending, page size `10`, choices `10, 25, 50, 100`, unique key `Id`, `Total` total count and `Data` rows. `getPagination` maps request fields to `pageSize`, `pageIndex`, `sort`, `sortType`; merge filter values through `getWebControls`. Check `Tag == 1`; the adapter reports application errors and non-aborted load failures.

The adapter enables column selection, refresh, card/table toggle and click-to-select. It does not enable a detail-row expander. Preserve each column's visible/sortable/alignment settings and enum formatter. Cells have `8px` padding; Bootstrap Table header inner line-height is `24px` with `8px` padding, so rows are compact but not all a fixed `30px`. Body overflow is automatic; borders use the final shared override. Pagination uses `4px 10px` controls and a light-gray active state.

Every current flat list opts into `data-mobile-responsive="true"`. The mobile extension defaults are otherwise `mobileResponsive:false`, `minWidth:562`, `columnsHidden:[]`; at `562px` or less it switches to card view, not automatically to a curated reduced-column table. Chinese loading/no-match text comes from the bundled locale. R: distinguish an empty successful result from request failure, keep retry reachable, and verify selection/actions after card-view toggles.

### 8.5 Trees, tree selects and tree tables

E12/E13/E16: the user page combines a `185px` department pane with a list; selecting a node sets `DepartmentId` and re-queries. Expand/collapse/refresh operate on the tree, not the table. `ysComboBoxTree` creates `id_input` / `id_tree`; its `data-key` contains comma-separated ancestor IDs and `data-value` a `>`-separated display path. Use `ys.getLastValue` where saving a leaf ID is required.

The two area partials bind `AreaId` through `areaId` and load `SystemManage/Area/GetZtreeAreaListJson`. The form partial takes Bootstrap label/content widths from `ViewData` and sets `expandLevel:0`; the filter partial is an inline query item. Reuse these tree-select partials, not an invented multiselect cascade.

Role permission editing populates the menu tree before applying `MenuIds`, then saves checked IDs as a comma-separated value. Preserve the source parent/child checkbox semantics; do not silently replace them with independent checks or a different authorization model. Menu-choice search uses the existing tree search handler.

Department/menu/area hierarchical grids use `ysTreeTable` and `bootstrapTreeTable`. Ordinary keys are `Id` / `ParentId`; the area grid uses `AreaCode` / `ParentAreaCode`, with `expandColumn:2`. Expansion belongs to the meaningful hierarchy column. A `data-mobile-responsive` attribute on a tree table does not activate Bootstrap Table's mobile extension. R: provide keyboard hierarchy access and controlled horizontal overflow without hiding essential operations. The area source's mismatched table API and missing form must not be copied as valid behavior; see section 14.

### 8.6 Dialogs, feedback and loading

E08/E22: `ys.openDialog` uses Layer type `2` (URL iframe), default width `768px`, unspecified height `$(window).height() - 50` in pixels, maximize/minimize enabled, shade `0.4`, Confirm/Close buttons, and `shadeClose:false`. `ys.openDialogContent` uses type `1` (markup), no title/buttons by default, no maximize/minimize, and `shadeClose:true`. On the helper's user-agent mobile test, dimensions become `auto`; this is separate from CSS breakpoints.

The callback locates the child iframe and calls `saveForm(index)`. On `Tag == 1`, the child invokes its established parent refresh (`searchGrid`, `searchTreeGrid(id)` or `getForm`) and closes the same Layer index. Details, import, crop and skin pages can override title, dimensions or buttons. Do not assume every modal submits a form or every refresh callback has the same name.

The helper passes `fix`, while this Layer version's option is `fixed`; the former does not establish a non-fixed dialog. `btnclass` in alert wrappers is also not evidence that Layer buttons become Bootstrap teal. Confirm closes its prompt before invoking the callback; it does not perform the operation itself.

Success uses `top.layer`, `1000ms`; warning uses local `layer`, `1000ms`; error uses local `layer`, `3000ms`. Alert variants require acknowledgement. Loading is BlockUI plus a `125px` minimum loader, `18px` spinner; closing is delayed `50ms`. R: provide meaningful names/live regions, pending re-entry guards, failure recovery and focus return. Do not assume a one-second toast is sufficient for important information.

### 8.7 Date, import and portrait workflows

E10/E13/E17/E22: actual lists call separate `laydate.render` for start/end with `yyyy-MM-dd`; job fields use `datetime`. The shared `.select-time.length > 10` branch uses `layui.use` and `molv`, but current pages do not meet that condition. It is not active automatic range synchronization. The ordinary picker main width is `272px`, cells `36px` by `30px`, range container `546px`. R: add start/end constraints explicitly when needed and validate chronological order; do not assume changing markup activates the dormant branch.

E15 import is two-stage: File Input accepts `xls/xlsx`, hides preview, uploads to `File/UploadFile`, and stores successful `FilePath`; confirmation calls `ImportUserJson` with that path and `IsOverride`. Preserve the existing template download and overwrite choice. R: clear stale paths after removal/replacement/failure, disable import until upload succeeds, and distinguish upload failure from import failure. Client extensions are not server-side content validation.

E15 portrait editing uses a `400px` crop surface, `200px` crop boundary, and `64/128/180px` previews. Preserve image selection, zoom, crop and explicit save. The blob upload uses `fileList`; the returned path is saved as `Portrait` through `ChangeUserJson`, then the parent's portrait is refreshed. R: handle invalid files, decoding failure, upload failure, cancel and narrow-screen overflow. Fixed source crop geometry is not proof of mobile usability.

### 8.8 Business lists, forms and details

E13/E14/E16/E17/E24 and the manifest: user, position, role, dictionary, dictionary-detail, job and log lists follow the flat-grid pattern. Department, menu and area follow the tree-grid pattern. Preserve dictionary parent IDs when opening detail lists, department/user context, role-menu IDs and each enum's underlying value.

User profile/detail, password/reset, department/position, dictionary/detail and role forms use the form hosts with their own required/read-only fields. API and operation log details display request/result text; R: render untrusted strings as text, wrap long values and distinguish missing values from empty payloads. Do not turn read-only log details into editable fields or invent an AutoJobLog form absent from the supplied views.

Menu form conditionally shows URL, authorization and icon fields for directory/menu/button types. The icon selector is a maximum `200px` scrolling popup; icon choices are `18px`, width `28px`, margin/padding `5px`, radius `3px`, hover `#1d9d74`. All 451 listed icon classes resolve in Font Awesome `4.7.0`. R: retain a keyboard route into/out of the picker and give each class an accessible name; do not add icons from a newer Font Awesome release.

### 8.9 Cron preview and server monitor

E17: job editing includes six Cron presets and a preview helper with `6px` radius, `#f8fbff` background and `#e5edf5` border. Empty input prompts for an expression; pending calculation shows a message; success lists the next five times; application failure uses an error class and text. `GetCronNextRunTimeJson` previews the schedule, not an execution command. Preserve start time and the indefinite-end sentinel. R: reject stale preview responses and distinguish preview from job start/stop/run actions in the job list.

E18: server monitor is separate from the business dashboard. It uses CPU/RAM iboxes and server/.NET property tables, polling raw `$.ajax` every `3000ms`. Collapsing animates for `200ms`; closing removes the panel. Polling does not automatically stop when a panel closes, and no complete error/retry lifecycle is implemented. R: stop timers when leaving the page, avoid overlapping requests and expose unavailable data without showing a global loading mask every three seconds.

### 8.10 Login, dashboard and exceptions

E05: sign-in has account/password/captcha, refreshable captcha, remember-account control, validation, pending submission and response feedback. Keep public branding YiSha; do not reproduce local customization names or default-account hints as production credentials. R: make captcha refresh named and keyboard accessible, preserve account text after failure, and ensure password/captcha handling follows the actual server contract.

E18: dashboard has a dark summary band, six metrics, an activity line chart and a donut breakdown. The active line is one series in `#0f8b72`; the donut radii are `46% / 72%`, palette `#0f8b72`, `#315d95`, `#d89b22`, `#c95746`, `#6d7d73`. Charts resize on window resize. Data loads once, with empty chart containers and an `UpdatedAt` fallback `--`; there is no automatic refresh and `loadDashboard` does not itself check `Tag`. R: handle empty arrays, business errors, request failure and chart disposal explicitly; do not label sample/absent values as live.

E19: permission denied is a standalone `403` screen with semantic main content, a hidden decorative lock, explanation, Back and Home. Back uses history when its length exceeds `1`, otherwise top-level `Home/Index`; Home escapes the iframe. `Home/Error` is only `@ViewBag.Message`, not a designed `404/500` system. R: add runtime error handling only with the real server status/route contract, and never claim a full exception suite from these two views.

## 9. Integration and State Contracts

### 9.1 Requests and permissions

E08: `ys.ajax` wraps jQuery callbacks, JSON requests, a default error alert and loading lifecycle. It does not automatically validate `Tag`, return a complete state machine, prevent repeated submissions or supply cancellation. `success` handlers must check the business response before refresh/close. Uploads use `ys.ajaxUploadFile` with `processData:false` and `contentType:false`. Export posts through `ys.exportExcel`, then navigates to the returned download path on success.

E10/E23: toolbar authority scans `#toolbar a` and `.toolbar a`; IDs plus the current URL feed `top.getButtonAuthority`, yielding identifiers such as `organization:user:add`. `#toolbarPermission` is not included automatically. Client removal is only presentation: preserve server authorization and route contracts. R: use unique IDs and adapt the authority collector deliberately when improving anchors to buttons; otherwise the new controls would evade the old selector.

Routes are area-aware, with `{area:exists}/{controller=Home}/{action=Index}/{id?}` and `{controller=Home}/{action=Index}/{id?}`. `Url.Content` / `ctx` carry deployment context, including the supplied `/admin` virtual-directory setting. Do not hardcode site-root URLs, remove permission checks, or change data schemas to simplify styling.

### 9.2 State coverage and recovery

| State | F / D baseline | R for new work |
| --- | --- | --- |
| Default / hover / active | Source button, menu, dropdown and plugin rules. | Keep state geometry stable and labels readable. |
| Focus | Teal form border; incomplete custom keyboard semantics. | Visible focus, labels and logical focus order for every command. |
| Selected / disabled | Table selection, `.disabled`, iCheck/Select2 states. | Enforce disabled behavior and preserve selection contracts. |
| Loading | Request mask, table locale, iframe loading, Cron message. | Prevent duplicate writes; name the pending operation. |
| Empty | Table no-match message and dashboard empty containers. | Distinguish no results from unavailable data and retain recovery actions. |
| Invalid | Validation messages and Cron error state. | Preserve input, associate errors, avoid overlap and focus the first error. |
| Request / business failure | Default request alert; many page-specific `Tag` checks. | Check both failure channels, unblock controls, keep retry reachable. |
| Success | Messages, parent refresh, dialog close. | Refresh the correct host only after confirmed success; avoid losing context. |
| Destructive / bulk | Selection guards and count confirmation. | Prevent re-entry; preserve server authorization; handle partial failure if the API supports it. |
| Cancel / leave | Layer close and tab removal. | Return focus, release timers/listeners/charts and define unsaved-work handling when needed. |

These additions do not certify that every existing page already covers every row. Async races, keyboard semantics and disposal remain runtime verification items.

## 10. Responsive Behavior

### 10.1 Actual boundary matrix

E02/E04/E05/E06/E18/E19/E21. `<=` includes the exact boundary; `<769` is a JavaScript test. Test the iframe's own viewport as well as the top window.

| Boundary | Source effect / scope |
| --- | --- |
| `>=1200px` | Inherited utilities and Bootstrap large grid/container rules; not a new dashboard breakpoint. |
| `<=1180px` | Dashboard metrics: six columns become three. |
| `1170px` | Inherited vertical-timeline rules only; no active timeline page. Not a global grid breakpoint. |
| `<=1000px` | `.welcome-message` hidden; current account/fullscreen menu group uses this class. |
| `>=992px` | Bootstrap medium grid tier. |
| `<=880px` | Login becomes one column, maximum `430px`; operations lattice hidden, card padding `24px`, title scales reduced. |
| `<=820px` | Dashboard header, metrics and analysis panels stack into one column. |
| `<769px` | Shell load/resize handler adds `mini-navbar` and calls sidebar `fadeIn`; widening does not automatically remove the class. |
| `<=768px` | CSS hides fixed sidebar and tab strip; script/body classes can make the sidebar visible. Search surface is hidden. |
| `>=768px` | Bootstrap small grid and floated filter items start. At exactly `768px`, the search surface is still hidden by its other rule. |
| `<=767px` | Shell dropdown/account-image styling compacts; source-specific dropdown colors apply. |
| `<=680px` | Permission page: one column, outer padding `18px`, inner padding `28px`, symbol `96px`, title `24px`, full-width actions. |
| `<=562px` | Opted-in flat tables switch to card view; no automatic curated column hiding. |
| `<=420px` | Login options and captcha grid stack; captcha image width `118px`. |
| `<=350px` | Mini sidebar width becomes `0`; narrow navigation close affordance is exposed. |

### 10.2 Adaptation requirements

R: do not describe the narrow sidebar as reliably hidden by CSS alone. Test collapse, reload, widening, tab switches and cookie-selected themes together. Keep filters and account actions reachable when their legacy groups disappear, with a deliberate disclosure control where needed. Do not add a universal drawer that the host does not own.

R: on narrow forms, stack Bootstrap label/content columns, constrain plugin popups to the owning viewport, wrap long errors and selection chips, and keep primary actions accessible. Tables/tree tables need controlled overflow or their actual card view, not document-wide clipping. Check fixed crop dimensions and Layer auto sizing before claiming mobile support. Source breakpoints and a static stylesheet audit are not browser acceptance results.

## 11. Accessibility and Safety Boundaries

### 11.1 Existing evidence

E01/E06/E10/E19: legacy shared layouts disable zoom, several commands are anchors without native button semantics, custom tab/tree controls lack a complete keyboard model, and iframe titles/focus return are incomplete. The standalone permission page already has `lang="zh-CN"`, a named main region, a real Back button and hidden decorative markup. Retain those positive patterns.

The original palette is not a WCAG conformance claim. White text on the ordinary teal button and pale/colored status combinations needs contrast review; compact `12px` type does not qualify as large text. No screen-reader, keyboard, zoom, contrast-in-context or touch-target acceptance test was performed.

### 11.2 Required improvements for new work

R: enable zoom; use semantic buttons/links; name icon-only actions; associate labels and errors; add visible focus and `aria-expanded` / `aria-controls` for collapsible regions; give each iframe a meaningful title. Dialogs must receive focus, keep keyboard interaction coherent and return focus to the trigger. Tabs, trees, menus, paging and selection need complete keyboard paths, not just hover.

R: use polite status announcements for success/progress and assertive alerts only for urgent failures. Do not rely on color alone. Verify WCAG `2.2` AA contrast and target-size requirements in context; make local accessible variants when needed without globally replacing the brand palette. Support reduced motion and text enlargement, provide chart summaries/data alternatives, and avoid rendering untrusted server text as HTML. These requirements improve the source, not document an already-certified application.

## 12. Do / Don't

### 12.1 Do

- Use the correct shared host, local adapters, bundle versions and virtual-directory-aware URLs.
- Keep the blue/dark shell, teal content and scoped green surfaces separate.
- Preserve actual Add/Edit color mapping, compact controls and plugin-specific geometry.
- Distinguish source facts, inherited behavior, recommended improvements and unverified outcomes.
- Keep permission IDs, `[col]` mappings, response fields, enum values and iframe callbacks intact.
- Add explicit error recovery, keyboard/focus behavior and pending guards for new work.

### 12.2 Don't

- Do not derive versions from directory names or load every vendored capability.
- Do not claim dates auto-link, helpers check every business response, or all skins apply to iframe content.
- Do not use `65px` as the current fixed-sidebar rail or describe mobile tables as automatic column prioritization.
- Do not turn CRUD lists into marketing surfaces, duplicate shell chrome or globally normalize radii.
- Do not introduce another icon/component family; preserve only the existing dependency glyph exceptions.
- Do not reproduce missing views, duplicate IDs, unsafe HTML insertion, inaccessible controls or silent failures as requirements.
- Do not claim runtime, security, accessibility or browser acceptance from static document validation.

## 13. Agent Prompt Guide

### 13.1 Required context

Determine the selected specification version, page pattern, host, loaded plugins, route/permission IDs, fields, parent callback, enum/format rules and required states before generating UI. Without the application source, use the self-contained patterns here for visual work and explicitly list unknown business contracts. Do not invent evidence or silently change dependencies.

### 13.2 Implementation prompt

```text
Read the complete YiSha DESIGN.md version 1.0.0 first. Treat source facts,
dependency behavior, required improvements and unverified items separately.
Implement the requested page with the documented host, Bootstrap 3 grid,
Font Awesome 4 icons, local adapters and ys.* helpers. Preserve routes,
permission IDs, col mappings, response fields, enum values and callbacks.
Keep shell, CRUD and login/dashboard palettes separate. Follow the actual
plugin dimensions and breakpoint matrix. Add explicit pending guards,
failure recovery, keyboard access and focus behavior without inventing
source capabilities. Report any missing business contract. Static checks
are not UI acceptance; state exactly what was and was not verified.
```

### 13.3 Review prompt

```text
Review against YiSha DESIGN.md 1.0.0 and the nearest existing page.
Prioritize broken data/permission/callback contracts, wrong dependency
versions, cascade errors and inaccessible or unreachable states before
cosmetic preferences. Check iframe boundaries, mobile card view, hidden
filters/account actions, date initialization, upload stages and cleanup.
Give evidence for each mismatch. Separate source defects from document
defects and proposed improvements. Do not claim unrun browser tests passed.
```

## 14. Known Gaps and Verification Checklist

### 14.1 Source limitations not repaired by this release

- Tokens are documentation aliases for literal CSS, not runtime custom properties. No complete dark mode or internationalization system is established.
- Shared automatic date code is effectively dormant for current pages; loading, error, repeat-submit and cleanup coverage varies by page.
- Area actions call the wrong table API and an unavailable search method; the expected area form is absent. An AutoJobLog form action likewise lacks its corresponding view. These are source defects, not new document features.
- Some job/log toolbars reuse `btnDelete`; the permission toolbar is outside the shared collector. UserForm's remark textarea lacks a binding `col`, and ChangeUser's position select uses a differing `dataName`. Preserve the intended business field, not these mistakes.
- Long validation text, tree-select keyboard behavior, fixed crop geometry, small mobile targets and hidden narrow-screen controls need runtime work. Server polling and Cron preview need lifecycle/race handling.
- The inactive boxed-layout CSS references a missing pattern image; unreferenced jQuery UI CSS also references missing sprites. Do not present these as failures of active pages, or enable those templates without an asset audit.
- A matching upstream tag, complete image rights, all minified-JS behavioral equivalence, runtime routes/data and browser accessibility remain U. The source files were not modified by this documentation release.

### 14.2 Acceptance checklist

- [ ] Read the selected version and identify the host, page pattern and source snapshot.
- [ ] Preserve exact dependency implementation versions and avoid duplicate adapter loading.
- [ ] Check source palette, cascade, dimensions, radius, shadows, stacking and font fallback in the owning document.
- [ ] Check request fields, `Tag`/`Data`/`Total`, permissions, IDs, `[col]`, enums and parent refresh callbacks.
- [ ] Check hover, focus, active, selection, disabled, loading, empty, validation, request failure, success, cancel and recovery.
- [ ] Test applicable boundaries at `1200/1180/1170/1000/992/880/820/769/768/767/680/562/420/350px`, including adjacent widths.
- [ ] Check text overflow, popup bounds, table/card-view operations, crop usability and iframe height.
- [ ] Check keyboard, focus return, zoom, contrast, target sizes, reduced motion and live announcements in a real browser when UI changes are made.
- [ ] Record static checks separately from runtime/browser checks; mark unavailable evidence U.
- [ ] Keep the specification version `1.0.0`; trace in-place updates through commits and source hashes without moving historical tags or replacing Release attachments.
