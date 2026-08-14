---
version: "1.0.0"
name: "YiSha Design System"
description: "Source-derived design rules for consistent YiSha administration interfaces."
colors:
  content_primary: "#1ab394"
  content_primary_hover: "#18a689"
  shell_header: "#3c8dbc"
  shell_logo: "#367fa9"
  shell_sidebar: "#2f4050"
  shell_sidebar_active: "#293846"
  page_background: "#f3f3f4"
  surface: "#ffffff"
  border: "#e7eaec"
  text: "#676a6c"
  auth_dashboard_accent: "#0f8b72"
  action_secondary: "#1c84c6"
  info: "#23c6c8"
  warning: "#f8ac59"
  danger: "#ed5565"
typography:
  font_family: '"Microsoft YaHei", "open sans", "Helvetica Neue", Helvetica, Arial, sans-serif'
  modern_font_family: '-apple-system, BlinkMacSystemFont, "Segoe UI", "Microsoft YaHei", Arial, sans-serif'
  base_font_size: "12px"
  base_font_weight: "400"
  compact_line_height: "1.42857"
---

# YiSha Design System

## 1. Overview

YiSha is a compact, work-oriented administration interface built around Bootstrap 3, jQuery, server-rendered Razor pages, and a small first-party JavaScript API. Its visual identity comes from three related layers:

1. A blue header and dark collapsible sidebar form the default application shell.
2. Teal `#1ab394` is the primary action color inside list, form, table, and utility pages.
3. The sign-in page and operations dashboard use a newer deep-green `#0f8b72` accent with stronger typography and restrained 8px surfaces.

Keep these layers distinct. Do not recolor the complete shell teal, and do not apply the sign-in page's large type and deep shadows to dense CRUD pages.

This file describes visual outcomes, component composition, interaction states, and responsive behavior. It does not replace Bootstrap, plugin, routing, permission, or backend API documentation.

## 2. Source Baseline

The specification was derived from a .NET 10 MVC Web frontend snapshot. The static audit covered:

- 50 Razor files across application views, shared layouts, and three functional areas.
- Shared full-shell, list-page, white-form, and gray-form layouts.
- First-party `style.css`, `skins.css`, `yisha.css`, `login.css`, and their generated bundles.
- First-party page, data, initialization, table, tree-table, and zTree JavaScript adapters.
- Page-level styles used by sign-in, dashboard, skin picker, account, import, tree, and detail screens.
- All dependency entries in `bundleconfig.json`.

### Runtime UI stack

| Capability | Source dependency | Contract |
| --- | --- | --- |
| DOM and events | jQuery `2.1.4` | Required by the shell, forms, plugins, and `ys.*` helpers. |
| Layout and controls | Bootstrap `3.3.7` | Use its 12-column grid, forms, buttons, dropdowns, labels, badges, and pagination. |
| Icons | Font Awesome `4.7.0` | Use `fa` classes; do not mix in a second icon family on an existing page. |
| Dialogs and feedback | Layer `3.1.1` | Used behind dialog, message, confirmation, loading, and alert helpers. |
| Date input | Laydate `5.0.9` | Use for date and date-range fields; the existing green theme is `molv`. |
| Data grids | Bootstrap Table `1.12.0` | Standard list paging, selection, sorting, mobile adaptation, and localized labels. |
| Tree grids | Bootstrap TreeTable `1.0` | Hierarchical tabular records with expand and collapse actions. |
| Tree selection | zTree v3 | Departments, menus, areas, and permission selection. |
| Enhanced selects | Select2 `4.0.6` | Use only where search or multi-selection is required. |
| Checkbox and radio | iCheck `1.0.2` | Initialized with the blue checkbox and radio skin. |
| Validation | jQuery Validation `1.14.0` | Inputs require stable `id` and generated `name` attributes. |
| Uploads | File Input `5.0.3`, Cropbox `1.0`, Image Upload `1.0` | File import, image preview, and portrait cropping. |
| Charts | Vendored ECharts build | Reserved for dashboard and report surfaces. |

SmartWizard `4.0.1`, jQuery UI `1.12.1`, jQuery Layout `1.4.4`, Highlight `9.13.1`, and Bootstrap Tags Input `0.8.0` are bundled capabilities. Load and use them only when the target page already requires that interaction; their presence is not permission to decorate every screen.

## 3. Design Principles

### 3.1 Dense and operational

- Optimize for scanning, filtering, selecting, and repeated actions.
- Keep body text at 12px and most control labels at 12-14px.
- Place related filters in one compact search surface and actions in a nearby toolbar.
- Prefer tables and trees over card collections for record management.

### 3.2 Hierarchy through structure

- Use the dark sidebar, blue header, white content surfaces, borders, and spacing to create hierarchy.
- Reserve bright colors for actions, selection, status, and small chart series.
- Keep page backgrounds quiet and content surfaces white.

### 3.3 Familiar page templates

- List pages use the shared index layout, search area, toolbar, and Bootstrap Table.
- Edit and detail flows use white or gray form layouts and Bootstrap horizontal forms.
- Cross-page workflows open in shell tabs; focused edits open in Layer dialogs.
- Permission and organization relationships use trees or tree tables.

### 3.4 Compatibility first

- Reuse existing class names, layouts, plugin adapters, and `ys.*` helpers.
- Do not replace one control with a visually similar component that breaks established initialization or response handling.
- Treat visual modernization as contextual: the sign-in and dashboard treatments are deliberate exceptions, not a global redesign.

## 4. Color System and Themes

### 4.1 Core content colors

| Role | Value | Usage |
| --- | --- | --- |
| Primary action | `#1ab394` | Primary buttons, search action, selected Select2 chips, progress, focused accents. |
| Primary hover | `#18a689` | Hover, focus, and active state for the primary action. |
| Secondary action | `#1c84c6` | Existing `.btn-success`; use for secondary positive or edit actions. |
| Information | `#23c6c8` | Existing `.btn-info`, informational badges, small highlights. |
| Warning | `#f8ac59` | Warning buttons, labels, and attention states. |
| Danger | `#ed5565` | Delete, destructive confirmation, and error status. |
| Body text | `#676a6c` | Default text on content pages. |
| Strong text | `#333333` | Filter labels, table headings, and emphasized field content. |
| Muted text | `#999999` | Secondary metadata and inactive controls. |
| Page background | `#f3f3f4` | Default gray workspace. |
| Surface | `#ffffff` | Forms, tables, panels, dropdowns, and cards. |
| Border | `#e7eaec` | Tables, panels, tabs, and section dividers. |

Semantic class names follow the source, not modern Bootstrap expectations: `.btn-success` is blue, while the most common affirmative action is `.btn-primary` in teal. Preserve that mapping on existing pages.

### 4.2 Default shell

- Header: `#3c8dbc`.
- Logo block and darker header hover: `#367fa9`.
- Dark sidebar: `#2f4050`.
- Active and hover sidebar row: `#293846`.
- Default sidebar link: `#b8c7ce` where the skin applies it.
- Selected blue item: `#1890ff`, with a 3px shell-blue left accent for active top-level navigation.
- Main workspace: `#f3f3f4` with white content surfaces.

The default class combination is `skin-blue theme-dark`. If a persisted `Skin` cookie exists, apply its skin and side-theme classes instead.

### 4.3 Alternate shell skins

| Skin | Header pair | Light-theme selected accent |
| --- | --- | --- |
| Blue | `#367fa9` / `#3c8dbc` | `#1890ff` on a pale blue surface. |
| Green | `#008d4c` / `#00a65a` | `#52c41a` on a pale green surface. |
| Purple | `#555299` / `#605ca8` | `#722ed1` on `#f9f0ff`. |
| Red | `#dd4b39` / `#d73925` | Red accent on a pale red surface. |
| Yellow | `#f39c12` / `#e08e0b` | `#faad14` on `#fffbe6`. |

`theme-dark` keeps the sidebar at `#2f4050`. `theme-light` uses `#f9fafc`, text near `#777`, a subtle right shadow, and skin-colored active states. A skin changes the shell; it must not remap content action semantics.

### 4.4 Sign-in and dashboard accent

- Deep green: `#0f8b72` for focus, chart lines, metric icons, and interaction accents.
- Ink: `#17202a` or `#17211d` for high-contrast headings and dark action surfaces.
- Gold: `#f4bd3f` or `#f6c453` for small brand and time accents.
- Blue support accent: `#2f6fed` for small sign-in labels or secondary illustration details.
- Dashboard canvas: `#eef2f1`; dashboard borders: `#dce4e0`.

Do not substitute this palette for standard list-page actions. It is scoped to authentication and analytical overview surfaces.

## 5. Typography

### 5.1 Default stack and scale

- Default stack: `"Microsoft YaHei", "open sans", "Helvetica Neue", Helvetica, Arial, sans-serif`.
- Base size: 12px, weight 400, color `#676a6c`.
- Use 10-11px only for compact counters, helper labels, or dense metadata.
- Use 13-14px for navigation, buttons, field emphasis, and common panel titles.
- Use 16-18px for section titles, dialog headings, and compact dashboard titles.
- Use 24-30px only for page-level numbers, error states, or dashboard metrics.

### 5.2 Modern authentication and dashboard scale

Use the system-first stack from the front matter. The sign-in brand may reach 42px, sign-in copy 30px, and dashboard metrics 28px. These sizes belong to spacious first-view surfaces and must not appear inside toolbars, table cells, or dialogs.

### 5.3 Text behavior

- Letter spacing is `0`; do not apply tight negative tracking.
- Use normal-weight form labels. Required markers are red and remain adjacent to the label.
- Use ellipsis only where a stable column or tab width requires it; expose the full value with a title or accessible equivalent.
- Keep button labels short and task-oriented: Search, Reset, Add, Edit, Delete, Export, Save, Close.

## 6. Layout and Spacing

### 6.1 Application shell

| Element | Dimension | Rule |
| --- | --- | --- |
| Expanded sidebar | `200px` | Fixed left navigation on desktop. |
| Collapsed sidebar | `50px` | Icon rail used by `mini-navbar`. |
| Collapsed flyout origin | `65px` | Second-level flyouts begin after the icon rail and its spacing. |
| Header | `50px` | Navigation links have at least 50px height. |
| Tab strip | `42px` | Inner tab controls are 40px high. |
| Page offset | `200px` | `#page-wrapper` desktop left margin. |
| Shell content padding | `0 15px` | Applied to `#page-wrapper`. |
| General wrapper | `20px` | Standard `.wrapper-content` padding. |

The shell uses a dark fixed sidebar, a colored top bar, a horizontal scrollable tab strip, and one visible `.admin-iframe` per active tab. Keep the first home tab permanent. New tabs reuse an existing URL instead of opening duplicates.

### 6.2 List pages

- `.container-div`: 10px vertical and 35px horizontal padding, full available height.
- `.search-collapse` and `.select-table`: white surface, 6px radius, 10px top margin, light `1px 1px 3px rgba(0,0,0,.2)` shadow.
- Filter row items: 30px high, 5px vertical margin, 15px right separation.
- Common text/select filter: 280px by 30px.
- Date-range field: 133px for each endpoint, with a compact separator.
- Table cell padding: 8px; use `#e7eaec` row borders and `#cccccc` header divider.
- Keep the filter, toolbar, table, and pagination visually connected as one work area.

### 6.3 Forms

- Use Bootstrap's 12-column grid and `.form-horizontal` for standard dialogs.
- Common label/content splits are `col-sm-3` + `col-sm-8` or `col-sm-2` + `col-sm-10`.
- Standard Bootstrap form controls are approximately 34px high with 6px by 12px padding.
- Use 15-20px panel padding; avoid oversized blank regions in transactional forms.
- Place Save and Close in the dialog footer or host Layer button row, not in an unrelated floating card.

### 6.4 Dashboard and sign-in

- Dashboard outer padding: 18px; grid gaps: 12-14px.
- Dashboard cards: 8px radius and 16-18px padding.
- Sign-in card: 386px wide, 30px padding, 8px radius.
- Sign-in controls and primary button: 48px high.
- Keep these layouts spacious, but ensure the next meaningful content remains visible on normal laptop screens.

## 7. Elevation and Shape

### 7.1 Radius scale

- 1px: standard form controls in the legacy content layer.
- 2-3px: buttons, dropdowns, loaders, compact panels, and shell details.
- 4px: filters, checkbox outlines, and common Bootstrap controls where already defined.
- 6px: list search and table surfaces.
- 8px: sign-in and dashboard cards only.
- 50%: avatars and circular status or icon controls.

Do not globally normalize everything to 8px. Radius expresses the distinction between dense CRUD surfaces and newer first-view surfaces.

### 7.2 Shadow scale

- Flat: form controls, panels, progress bars, and most content widgets use `box-shadow: none`.
- Hairline card: `0 1px 1px rgba(0,0,0,.1)` for small `.box` surfaces.
- List surface: `1px 1px 3px rgba(0,0,0,.2)`.
- Dropdown: subtle `0 0 3px rgba(86,96,117,.3)` or equivalent.
- Dashboard: `0 12px 28px rgba(23,33,29,.07)`.
- Sign-in: stronger `0 28px 70px rgba(23,32,42,.22)` only for the authentication card.

Avoid stacked cards and repeated heavy shadows. One surface boundary is usually enough.

## 8. Components

### 8.1 Header, sidebar, and account menu

- Use Font Awesome icons before navigation labels.
- Top-level sidebar rows may own nested second- and third-level lists.
- Active ancestry remains expanded and visually selected when a tab becomes active.
- The collapse button uses the bars icon; the close affordance appears on very narrow screens.
- The account menu contains portrait, identity, profile, password, skin, and sign-out actions in a compact dropdown.
- Preserve the 50px header rhythm; do not add a second toolbar above it.

### 8.2 Tab workspace

- Tabs are 40px controls inside a 42px strip.
- Provide previous, next, refresh, close-current, close-other, and close-all behavior.
- The active tab is visually distinct and synchronized back to the sidebar.
- A tab close icon is secondary and turns danger-colored on hover.
- Iframes fill the remaining workspace and show a loading state until ready.

### 8.3 Search and toolbar

- Filters sit in `.search-collapse`; use labeled inputs, selects, and date ranges.
- Search uses the primary teal action. Reset is neutral or white.
- The toolbar sits immediately above the table and uses small buttons with icons.
- Add is primary or success-contextual, Edit requires exactly one selected row, and Delete requires one or more selected rows.
- Disabled actions must look disabled and reject interaction, not merely change color.

### 8.4 Buttons

| Type | Class | Visual role |
| --- | --- | --- |
| Primary | `.btn-primary` | Teal create, save, confirm, or search action. |
| Secondary positive | `.btn-success` | Blue edit, enable, or secondary action in existing screens. |
| Information | `.btn-info` | Cyan informational or view action. |
| Warning | `.btn-warning` | Amber reset, pause, or caution action. |
| Danger | `.btn-danger` | Red delete, revoke, or destructive action. |
| Neutral | `.btn-white` / default | Close, cancel, or low-emphasis utility. |

Use `.btn-sm` in toolbars and `.btn-xs` inside dense table operation columns. Every icon-only button requires an accessible name or tooltip.

### 8.5 Forms and validation

- Use `.form-control`, `.form-group`, `.control-label`, and Bootstrap grid columns.
- Required fields show a red marker next to the label.
- Validation messages are 12px red text positioned near the associated control without covering entered content.
- Error controls use a pale red background or red border; success must not rely on color alone.
- Use Select2 only for searchable or multi-value selection. Selected chips use the primary teal.
- Initialize checkbox and radio controls through iCheck's blue skin where the shared form layout is used.
- Keep IDs stable because initialization and validation derive behavior from them.

### 8.6 Tables and pagination

- Use Bootstrap Table for sortable, pageable record lists.
- Table headings use stronger text and a clear bottom divider; rows remain white or lightly striped.
- Center narrow status, selection, date, and action columns when that improves scanning.
- Preserve server paging and the response contract expected by the local adapter.
- Pagination uses white 4px by 10px controls; the active page is light gray, not a large filled pill.
- Use badges or labels for compact status values, with text that remains meaningful without color.

### 8.7 Trees and tree tables

- Use zTree for menu, department, area, and permission selection.
- Use Bootstrap TreeTable when hierarchy and record columns must be visible together.
- Keep expand/collapse controls aligned with the first meaningful column.
- Provide Expand All / Collapse All only when the hierarchy is large enough to justify it.
- Tree selection must remain usable by keyboard in new work, even though the legacy adapter does not provide a complete ARIA tree model.

### 8.8 Panels, boxes, and dashboard metrics

- Use `.ibox` / `.ibox-title` / `.ibox-content` or `.box` for bounded content tools.
- Ibox titles are generally 14px; controls sit at the right edge.
- Collapsible and closable panels retain visible tool icons and stable content dimensions.
- Dashboard metrics may use 8px cards, a 34px icon tile, 28px value, and concise 12px supporting text.
- Do not put a card inside another card or convert every section into a floating tile.

### 8.9 Dialogs, feedback, and loading

- Open focused CRUD forms with `ys.openDialog`; use `ys.openDialogContent` for supplied markup.
- Use `ys.confirm` before destructive actions.
- Use `ys.msgSuccess`, `ys.msgWarning`, and `ys.msgError` for brief outcomes.
- Use alert variants only when acknowledgement is required.
- Use `ys.showLoading` / `ys.closeLoading` around asynchronous or iframe work.
- Success messages should be polite status updates; destructive failures should be assertive alerts.

### 8.10 Dates, uploads, portraits, and charts

- Use Laydate for date fields and synchronized start/end ranges.
- Use File Input for file import with visible file name, progress, success, and failure states.
- Use Cropbox for portrait cropping; preserve preview, crop boundary, and explicit save/cancel actions.
- Use ECharts only when a chart makes comparison or trend materially clearer than a table.
- Dashboard chart colors begin with deep green and may add blue, amber, red, and muted green series.

## 9. Interaction Patterns

### 9.1 First-party helper contract

Prefer the existing helpers when implementing pages in the current application:

- `ys.ajax` for application requests and standard success/error response handling.
- `ys.ajaxUploadFile` for uploads.
- `ys.openDialog`, `ys.openDialogContent`, and `ys.closeDialog` for layered workflows.
- `ys.confirm` for destructive confirmation.
- `ys.msgSuccess`, `ys.msgWarning`, `ys.msgError` for transient feedback.
- `ys.alertSuccess`, `ys.alertWarning`, `ys.alertError` for blocking feedback.
- `ys.showLoading` and `ys.closeLoading` for pending work.
- `ys.getIds`, `ys.checkRowEdit`, and `ys.checkRowDelete` for selection-aware toolbar actions.
- `ys.exportExcel` for established export flows.
- `ys.formatDate`, `ys.isNullOrEmpty`, and related utility methods for existing page conventions.

Do not invent a parallel fetch, modal, or toast layer inside an established page.

### 9.2 Initialization

- Shared initialization wires iCheck, Select2, date ranges, validation names, tree search, and toolbar authority.
- Table selection enables Delete when at least one row is selected and Edit only when exactly one row is selected.
- Shell navigation initializes MetisMenu and a 4px SlimScroll rail.
- The skin picker persists `skin-name|theme-name` in the `Skin` cookie.
- Every asynchronous action exposes pending, success, empty, and error states.

### 9.3 Motion

- Use 150-300ms transitions for borders, simple transforms, menus, and state changes.
- Sidebar content may fade over approximately 500ms during collapse transitions because that behavior already exists.
- Motion must explain state change; avoid decorative bouncing, parallax, or continuous animation.
- Respect reduced-motion preferences in new CSS.

## 10. Responsive Behavior

### 10.1 Shell and list pages

- Below 769px, the shell enters `mini-navbar` behavior.
- At 768px and below, the fixed sidebar is hidden until explicitly toggled.
- At 767px and below, dropdown and account-image details compact for narrow layouts.
- At 350px and below, the collapsed sidebar width becomes 0 and the close control is exposed.
- The list search surface is hidden below 768px in the source. New pages should provide an explicit filter trigger rather than making filters unreachable.
- Bootstrap Table's mobile extension may collapse low-priority columns; preserve the primary label and operation path.

### 10.2 Sign-in

- At 880px and below, use a single-column shell no wider than 430px and hide the decorative operations lattice.
- At 420px and below, stack remember-account and default-account content vertically.
- Keep the form card within the viewport with at least 14px horizontal breathing room.

### 10.3 Dashboard

- Above 1180px, metrics use six equal columns.
- At 1180px and below, metrics use three columns.
- At 820px and below, the header, metrics, and analytical panels stack into one column.
- Charts resize with the window and retain a stable explicit height.

### 10.4 Legacy breakpoints

The source also contains Bootstrap-era boundaries at 992px, 1170px, and 1200px for grid and utility visibility. Follow the closest existing page pattern. Do not add viewport-scaled font sizes, and do not create a new breakpoint for a problem that existing grid behavior already solves.

## 11. Accessibility

The source is the visual baseline, not a requirement to repeat its accessibility gaps.

- Keep browser zoom enabled; do not copy viewport settings that disable user scaling.
- Use real `<button>` elements for commands and real links for navigation.
- Give icon-only controls an accessible name and visible focus state.
- Add `aria-expanded` and `aria-controls` to collapsible navigation and panels.
- Give each workspace iframe a meaningful `title`.
- Associate labels and validation messages with form controls.
- Move focus into an opened dialog and return it to the trigger when the dialog closes.
- Use `role="status"` / `aria-live="polite"` for normal success and progress messages.
- Use `role="alert"` or assertive live regions only for urgent errors.
- Do not communicate status by color alone; include text or an icon with an accessible label.
- Maintain keyboard access for tabs, trees, dropdowns, pagination, and table operations.
- Treat third-party defaults as a starting point and add missing semantics at the application layer.

## 12. Do / Don't

### Do

- Do start from the shared list, white-form, gray-form, or shell layout.
- Do keep CRUD pages compact and table-centered.
- Do use the exact source-derived palette and semantic class mapping.
- Do preserve the default blue/dark shell unless the user-selected skin says otherwise.
- Do reuse existing plugin adapters and `ys.*` helpers.
- Do provide loading, empty, success, validation, permission-denied, and failure states.
- Do keep modern green styling scoped to sign-in and analytical overview surfaces.
- Do improve semantics, focus, and keyboard behavior without changing the visual language.

### Don't

- Don't turn record lists into marketing cards or a decorative bento layout.
- Don't use oversized headings inside toolbars, dialogs, tables, or sidebars.
- Don't add gradients, glass effects, large pill controls, or heavy shadows to standard CRUD pages.
- Don't recolor all semantic actions teal or assume Bootstrap class names have modern meanings.
- Don't mix icon libraries on one page.
- Don't place cards inside cards or float every page section above the canvas.
- Don't duplicate the shell header, sidebar, or tab manager inside an iframe page.
- Don't bypass permission-aware toolbar initialization.
- Don't reproduce inaccessible anchors, disabled zoom, missing labels, or silent async failures.

## 13. Agent Prompt Guide

### 13.1 Required context

Before generating UI, determine:

1. Page type: shell, list, form, detail, tree, sign-in, or dashboard.
2. Host layout and already loaded dependencies.
3. Existing controller routes, JSON response shape, permission IDs, and helper calls.
4. Required states: loading, empty, selected, disabled, validation, success, warning, and error.
5. Responsive priorities and keyboard path.

### 13.2 Implementation prompt

```text
Read DESIGN.md and inspect the nearest existing YiSha page of the same type.
Implement the requested interface with the existing Bootstrap 3 grid, first-party
YiSha classes, Font Awesome 4 icons, and ys.* interaction helpers. Preserve routes,
permission IDs, data contracts, dialog callbacks, and plugin initialization.

Use the default blue/dark shell for application chrome, teal for standard content
actions, and the deep-green modern treatment only for sign-in or dashboard work.
Keep CRUD pages compact, table-oriented, and keyboard accessible. Include loading,
empty, disabled, validation, success, and error states. Do not introduce another
component library or a new visual language.
```

### 13.3 Review prompt

```text
Review this page against DESIGN.md. Check theme role, typography, dimensions,
spacing, radius, shadow, component choice, ys.* integration, permission-aware
actions, responsive behavior, focus order, keyboard access, feedback states, and
text overflow. Report source-contract mismatches before cosmetic preferences.
```

## 14. Known Gaps and Iteration Checklist

### Known gaps

- The source uses literal CSS values rather than a centralized custom-property token system. Tokens in this document are semantic aliases, not runtime variables.
- The compact shell/content layer and the newer sign-in/dashboard layer have different type and elevation scales. Their boundary must remain explicit.
- Several plugins are legacy jQuery components; visual replacement alone is unsafe because pages depend on their events and data shapes.
- The iframe tab workspace has limited native semantics and requires additional titles, focus management, and keyboard handling in new work.
- Tree controls do not provide a complete ARIA tree or roving-tabindex model by default.
- Mobile list filters are hidden by legacy CSS; new work should provide an explicit filter disclosure control.
- This design version documents one audited snapshot. Re-audit before applying it to a materially different YiSha frontend.

### Iteration checklist

- [ ] Select the correct page template and host layout.
- [ ] Reuse existing dependency versions and helper APIs.
- [ ] Apply the right color layer: shell, content, or sign-in/dashboard.
- [ ] Keep base content text and information density compact.
- [ ] Verify expanded, collapsed, hover, focus, active, selected, disabled, and error states.
- [ ] Verify loading, empty, success, warning, and failure feedback.
- [ ] Check 1200px, 992px, 880px, 820px, 769px, 768px, 767px, 420px, and 350px behavior as relevant.
- [ ] Check text fit, table priorities, dialog bounds, and iframe height.
- [ ] Check keyboard path, focus visibility, labels, live regions, and non-color status cues.
- [ ] Confirm no new component library, shell duplication, or visual language was introduced.
