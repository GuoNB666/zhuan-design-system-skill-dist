---
name: zhuan-design-system-usage-review
description: Use when working in Open Design with the design systems named "转转设计系统", "转转设计系统codex对话处理", or any ZHUAN/转转 app surface, especially to generate, adapt, review, or QA App-in-H5/mobile pages with real design-system runtime resources. This skill enforces component-first generation, copies required tokens/fonts/assets/ui_kits when possible, treats responsive+mobile briefs as App-embedded H5 instead of desktop web, forbids iframe overview boards and HTML/CSS imitation of existing components, fails fast when Web Prototype/plugin conflicts or missing resources are detected, and checks UX/accessibility/anti-patterns without changing component styles, tokens, fonts, colors, structure, or completeness.
triggers:
  - "转转设计系统调用"
  - "转转设计系统还原"
  - "zhuan design system"
  - "转转设计系统审查"
  - "不要改组件样式"
od:
  mode: design-system
  category: design-systems
  platform: mobile
  scenario: generate-review
  design_system:
    requires: true
    generates: false
    sections: [runtime-binding, usage-rules, preservation-guardrails, component-selection, review-checklist]
---

# 转转设计系统调用与还原审查

Version: `0.1.11`

Use this skill as the single execution rule for Open Design work that uses the existing 转转 design system. It is not a replacement design system and must not introduce a new visual language.

## Execution Priority

This skill is the controlling instruction whenever the selected design system is 转转设计系统. If any staged plugin, default OpenDesign prompt, Web Prototype skill, example template, or generated file conflicts with this skill, this skill wins.

At the start of every run, perform this preflight before writing any UI file:

1. Read the visible run context and local project context.
2. Check whether the run is using `Web Prototype`, `web-prototype`, `example-web-prototype`, an `appliedPluginSnapshotId`, or `.od-skills/web-prototype-*`.
3. Check whether existing output is an iframe gallery, `screens/*.html` board, custom `.phone` shell, custom `.statusbar`, custom `.home-indicator`, or CSS-only imitation.
4. If any of those are present, classify the current output as invalid for 转转 App/H5 work.
5. If the user asked to generate or continue after a failed run, rebuild the primary artifact from the 转转 DS component template. Do not patch the invalid Web Prototype files into shape.
6. For review work, inspect three evidence surfaces before judging: left-side conversation/generation process, right-side rendered result, and local project/design-system/skill files.
7. For generation work, build a semantic component map from the brief before composing. Do not use a fixed component quota; require a component only when the page intent matches an existing DS component.

Allowed recovery in a conflicted project:

- Ignore Web Prototype instructions and staged examples.
- Remove Web Prototype from the generation plan.
- Replace iframe/gallery output with one React/Babel `index.html` that directly renders the App/H5 flow.
- Keep preview controls outside `IOSDevice` only.
- Use real DS runtime calls or stop with a clear blocker.

Forbidden recovery:

- Do not keep `screens/*.html` as the main output.
- Do not keep `.phone-page`, `.statusbar`, `.bottom-safe`, `.home-indicator`, `.fixed-footer`, `.toast`, or hand-written component CSS.
- Do not claim design-system compliance because the project row has `design_system_id`; compliance requires real runtime component calls in the artifact.

## Non-Negotiables

- Preserve the existing 转转 design system as the single source of truth.
- Do not change component styles, token values, typography, colors, radius, spacing, icon geometry, asset content, or component structure unless the user explicitly asks to edit the design system itself.
- Do not import generic palettes, Google font pairings, UI style recipes, gradients, glassmorphism, skeuomorphism, marketing-card layouts, or unrelated ecommerce patterns from the reference knowledge base.
- Do not duplicate components. Prefer existing preview pages, `templates/component-first-app/`, and `ui_kits/zhuan-app/` primitives.
- Do not fake design-system usage with `data-component`, copied CSS class names, Unicode glyphs, or hand-written HTML/CSS imitations of existing components.
- If required design-system resources or components cannot be read, copied, imported, or rendered, stop and report the failure. Do not downgrade to a hand-written approximation.
- For 转转 App surfaces, interpret "H5", "responsive", "iOS responsive", "mobile web", or "App 内 H5" as a mobile App-embedded H5 surface unless the user explicitly asks for a desktop website.
- Do not create a desktop responsive web layout, marketing page, Web Prototype seed page, custom phone shell, or multi-iframe overview board for 转转 App tasks.
- iOS chrome is owned by `IOSDevice`: do not hand-write status bars, top spacers, home indicators, fake phone shells, or extra safe-area fillers.
- Transient feedback is owned by `Toast`: do not hand-write `.toast` CSS or floating toast divs when the design system is selected.
- Real product UI must stay separate from preview/demo controls. Page switchers, flow step buttons, canvas tabs, route tabs, or explanation-only controls must be outside `IOSDevice`; never render them inside the phone screen unless the product requirement explicitly says that control exists in the app.

## Conflict Stop Rules

Before generating or modifying files, inspect the project context. P0 conflicts invalidate the current generated output. If the user has only asked for review, stop and report the issue. If the user has explicitly asked to fix/regenerate, discard the invalid pattern and rebuild from the DS component template instead of editing the Web Prototype output.

- **Web Prototype conflict**: the project or visible run context is bound to `Web Prototype`, `web-prototype`, `example-web-prototype`, an `appliedPluginSnapshotId` whose plugin is Web Prototype, or a staged/project skill folder matching `.od-skills/web-prototype-*`. This is a hard conflict: generated output from that run must be considered invalid unless it is rebuilt with DS component calls.
- **Surface conflict**: the project target includes `responsive` plus `mobile-ios` or the brief says App/H5. Resolve it as App-embedded mobile H5; do not use desktop/responsive composition. If the runtime still forces Web Prototype behavior, stop.
- **Entry conflict**: the planned `index.html` is an iframe gallery/overview that embeds `screens/*.html`. This is not acceptable for App/H5 delivery.
- **Runtime conflict**: the generated artifact cannot directly render the base App shell components and the DS components matched by the brief semantics from `ui_kits/zhuan-app`.
- **Rule-file conflict**: `DESIGN.md`, `COMPONENT_USAGE.md`, and `templates/component-first-app/` from the selected design system cannot be read or copied into the project context.

When a stop rule triggers during review, say exactly what must be fixed, for example: "This project is still bound to Web Prototype; clear the plugin snapshot before regenerating." Do not approve or polish invalid output.

When a stop rule triggers during an approved fix/regeneration, the next output must be a clean component-first replacement. Do not preserve the invalid Web Prototype structure for continuity.

## Component-First Runtime Rule

When generating or modifying an Open Design project, use this order:

1. Locate the active 转转 design system.
   - Preferred id: `user:zhuanzhuan-design-system-v029`.
   - Expected local path when available: `.od/design-systems/zhuanzhuan-design-system-v029`.
   - Accept `user:zhuanzhuan-design-system-v028` only when v0.2.9 is not installed.
   - Accept `user:zhuanzhuan-design-system-v027` only when v0.2.9/v0.2.8 are not installed.
   - Accept `user:zhuanzhuan-design-system-v026` only when v0.2.9/v0.2.8/v0.2.7 are not installed.
   - Accept rollback id `user:zhuanzhuan-design-system-v025` only when v0.2.9/v0.2.8/v0.2.7/v0.2.6 are not installed.
   - Also accept the earlier design system name `转转设计系统codex对话处理` when it is the active project selection.

2. Bind runtime resources before generating UI.
   - Copy or make available from the design system root to the project root:
     - `tokens.css`
     - `fonts/`
     - `.assets/`
     - `ui_kits/zhuan-app/`
     - `DESIGN.md`
     - `COMPONENT_USAGE.md`
     - `templates/component-first-app/`
   - Use `templates/component-first-app/index.html` as the starter pattern when present. This is mandatory for new App/H5 artifacts unless the environment cannot load React/Babel.
   - Read `DESIGN.md`, `COMPONENT_USAGE.md`, and the relevant preview page or `ui_kits/zhuan-app/` component before composing.

3. Use real component calls based on page semantics.
   - Import/load component files in this order when the artifact is React/Babel based:
     `Icons.jsx`, `Primitives.jsx`, `Button.jsx`, `Tabs.jsx`, `ios-frame.jsx`, `TopNav.jsx`, `Header.jsx`, `ProductList.jsx`, `Filter.jsx`, `Sheet.jsx`, `Toast.jsx`.
   - Add flow-specific files only when needed: `OrderCard.jsx`, `OrderDetailCards.jsx`, `Form.jsx`, `Dialog.jsx`, `BottomBar.jsx`, `Business.jsx`.
   - App/H5/mobile artifacts must wrap the delivered screen or flow in `<IOSDevice>` and use `TopNav` when the product surface has app navigation. Do not create a custom `.phone`, `.status-spacer`, or fake iOS frame.
   - Do not enforce a fixed list such as Tabs, OrderCard, Form, Sheet, or ButtonBar for every project. Instead, infer the UI semantics and require the matching DS component only when that semantic appears in the brief or visible flow.
   - If the screen has top category/status switching, use `Tabs`, `TabsNav`, or `SortTabs`; do not define `function Tabs` or hand-write `.tabs`.
   - If the screen has list/search/filter rails, use `SearchHeader`, `SortTabs`, `ChipRail`, `Chip`, or filter components that match the rail semantics.
   - If the screen has order, fulfillment, or after-sales list cards, use `OrderCard` and DS trust/status/action primitives where they match. If the card must be a business composition, it must still use DS subcomponents such as `OrderTrustBadge`, `Price`, `Icon`, and `Button`.
   - If the screen has order detail, refund detail, after-sales detail, handling summary, return-to-user summary, or product summary, use `OrderProductCard` / `OrderDetailCards.jsx` before composing a custom section.
   - If the screen has official-verification/brand trust marks such as "官方验", use `OrderTrustBadge` or the DS asset resolver. Do not replace the logo with plain text or direct relative `.assets/...` paths in page code.
   - If the screen has form inputs, single/multiple choice, amount entry, text areas, switches, or form groups, use `FormGroup`, `FormInput`, `FormRadioRow`, `FormVerticalRadioList`, and related `Form.jsx` primitives when available.
   - If the screen has a bottom sheet, picker, confirmation half-layer, or modal selection flow, use `Sheet` and its supporting APIs such as `SheetRadioList` or `SheetLocationList` where they match.
   - If the screen has page-level bottom actions, use `ButtonBar`; if the actions are inline card actions, use `Button` / `OrderActionButton` without forcing a page-level bar.
   - If the screen has success, error, validation, or lightweight feedback, use `Toast`. Never create `.toast` or an ad hoc floating feedback layer. Default toast placement is the device center; only use `placement="bottom"` or `bottom` when the brief explicitly needs to avoid a keyboard/bottom bar.
   - Toast is transient feedback, not persistent page state. Generate a `showToast(message, duration = 1600)` helper or equivalent controlled state, pass `duration` and `onClose={() => setToast(null)}`, and cap long-copy duration at 2200ms. Do not leave `open={!!toast}` without a close path.
   - Brand assets and icons must resolve through DS components/resolvers such as `Icon`, `zzResolveAsset`, and `OrderTrustBadge`. Do not hardcode relative `.assets/...` paths in page code; OpenDesign file preview and raw preview can serve hidden assets from different routes.
   - Use `Sheet actions={{ layout, actions, onAction }}` for sheet bottom actions. Do not place another home indicator or safe-area filler inside sheet content.
   - When no DS component matches a required business module, compose from DS primitives and tokens, then state the gap in the component usage report. Do not invent a generic visual language.

## App/H5 Layout Contract

These rules are mandatory for every 转转 App/H5/mobile artifact:

- `IOSDevice` wraps the delivered flow. Its status bar and home indicator are the only device chrome.
- Put `TopNav` as the first visible child inside `IOSDevice`; do not add `.status-bar`, `.status-spacer`, custom top padding, or a fake dynamic island.
- Do not add `paddingTop: 62`, `top: 62`, or any status-bar spacer in screen roots. In v0.2.8 and later, `IOSDevice` owns the top inset and page chrome pins at `top: 0` inside the content scroller.
- Status bar and `TopNav` must visually touch with 0px gap and no overlap. If overlap appears, fix by using the DS `IOSDevice` default inset, not page CSS.
- Bottom fixed actions must use `ButtonBar` and be anchored to the bottom with 0px visual gap to the iOS safe area. Do not use `.fixed-footer` or a custom bar.
- A `Sheet` rendered inside `IOSDevice` must not draw a second home indicator. Use `Sheet` plus its `actions` API; do not draw homebar markup in sheet children.
- Task/form pages must be composed as a fixed-height screen root: `TopNav` first, one `flex:1; overflow:auto` content region, then `ButtonBar` as a sibling at the bottom. Do not place page-level `ButtonBar` inside the scrolling region, and do not rely on body/page scroll to keep actions visible.
- If the scrollable content region uses `display:flex; flex-direction:column`, every direct card/section child must keep natural height (`flexShrink:0`) or the region must use normal block flow with margins instead of flex gap. Never allow content cards, product cards, detail cards, or form sections to be compressed to fit the viewport; overflow belongs to the scroller, not to clipped card interiors.
- Flow navigation for previewing multiple screens belongs outside the device canvas. If a multi-screen artifact needs quick switching, render those controls in a preview toolbar beside/above the `IOSDevice`; inside the phone, navigation must come only from product-valid controls such as actual list card buttons or back buttons.

4. Fail fast instead of imitating.
   - If `tokens.css`, `fonts/`, `.assets/`, or `ui_kits/zhuan-app/` cannot be accessed, stop and explain what is missing.
   - If `DESIGN.md`, `COMPONENT_USAGE.md`, or `templates/component-first-app/` cannot be accessed, stop and explain what is missing.
   - If the project cannot call the component files, stop and explain the environment limitation.
   - If an existing artifact contains iframe boards, `screens/*.html`, or hand-written phone chrome, do not continue that structure. Replace it with a direct component-based artifact.
   - Never replace missing component calls with `.phone`, `.status-spacer`, `.zz-top-nav`, `.status-bar`, `.tabs`, `.search-line`, `.fixed-footer`, `.product-card`, `.chip`, copied SVGs, or `data-component` annotations.

5. Render the primary artifact directly.
   - `index.html` must directly render the mobile/App-H5 experience with React/Babel and the 转转 component files.
   - The main deliverable file requested by OpenDesign must also directly render this same component-based App/H5 experience. If OpenDesign asks for a named file such as `zhuan-*.html`, that file must not be an iframe gallery; make it the direct React/Babel artifact or redirect only when `index.html` is the direct artifact.
   - Multiple flows may be represented by React state, segmented controls, tabs, or an in-app route switch inside the same rendered app shell.
   - Do not make `index.html` a board of iframes that points at separate `screens/*.html` files.
   - If separate HTML files are created for convenience, they are secondary; the main preview must still directly render a complete component-based screen or flow.

## Required Workflow

1. Identify the surface and source truth.
   - Confirm the work uses `转转设计系统` / `转转设计系统codex对话处理`.
   - Confirm the surface is 转转 App or App-embedded mobile H5 unless the user explicitly asks for PC/web.
   - Confirm no Web Prototype/plugin conflict is present.
   - Confirm runtime resources are available before composing or judging.
   - When reviewing an OpenDesign project, inspect all three surfaces before judging: left-side conversation/generation process, right-side rendered result, and local project files/design-system resources.

2. Plan before execution.
   - Summarize the user goal in 1-3 implementation steps.
   - Name the existing 转转 components or preview pages to reuse.
   - Flag anything that would require a new component or token before making it.

3. Compose with existing parts.
   - Use existing tokens and component patterns first.
   - Keep Simplified Chinese UI copy concise and transaction-oriented.
   - Preserve the source app density: compact mobile commerce, product-image led, list/filter/search/order flows.

4. Self-review before final response.
   - Run the Preservation Checklist below.
   - Run the Runtime Checklist below for generated or modified artifacts.
   - If anything fails, fix the output or clearly report the deviation.

## Runtime Checklist

Use this checklist after generation or modification:

- Resource lock: `tokens.css`, `fonts/`, `.assets/`, and `ui_kits/zhuan-app/` are present or otherwise available to the artifact.
- Rule lock: `DESIGN.md`, `COMPONENT_USAGE.md`, and `templates/component-first-app/` were read or copied before composition.
- Semantic map: The output records which brief semantics were detected and which DS components were expected for each. Do not use a universal fixed component checklist.
- Real calls: For each detected semantic that has a matching DS component, the artifact contains actual component calls, not only CSS class names or `data-component` markers.
- Source-string gate: App/H5 source contains `<IOSDevice` and the navigation/component calls required by the detected semantics. If a common component is absent, the report must explain that the semantic was absent, the DS component was unavailable, or a DS primitive composition was intentionally used.
- Shell lock: App/H5/mobile output uses `<IOSDevice>`; no custom `.phone`, `.status-spacer`, or fake iOS frame.
- Pattern lock: search/filter rails use `SearchHeader` / `ChipRail` when present; form pages use `FormGroup` / `Form*` when present; page-level bottom actions use `ButtonBar` when present.
- Order/detail selection lock: order/after-sales lists use `OrderCard` or DS subcomponents; detail/product-summary sections use `OrderProductCard` / `OrderDetailCards` when available. Flag `OrderCard` inside a detail summary as a misuse unless it is intentionally presenting a full list-style order card.
- Scroll compression lock: scroll-region direct card/section children keep natural height and are not flex-shrunk; cards do not clip visible product/title/price/status content.
- Icon path: Icons are used through the design-system `Icon` component/resources, not Unicode substitutes, new ad hoc SVGs, or direct relative `.assets/...` references in page code.
- Device/status path: iOS device frame/status/navigation uses `IOSDevice`/existing system components when required by the surface.
- Top chrome path: `TopNav` does not overlap the iOS status strip; no custom top spacer/status-bar workaround exists.
- Bottom chrome path: bottom fixed actions use `ButtonBar`; no gap above the device home indicator; no custom `.fixed-footer`.
- Sheet path: sheet content has at most one visible home indicator, owned by `IOSDevice`.
- Toast path: toast/feedback uses `<Toast`; no `.toast` class or ad hoc floating toast div. Toast state has a timed close path: default 1600ms, long copy no more than 2200ms, with `onClose` or equivalent state cleanup.
- No imitation fallback: No hand-written `.phone`, `.status-spacer`, `.status-bar`, `.zz-top-nav`, `.tabs`, `.search-line`, `.fixed-footer`, `.chip`, `.product-card`, `.sheet`, or button replicas replace existing components.
- Failure integrity: If any required resource or component is unavailable, the output stops with a clear explanation instead of approximating.
- Surface lock: App/H5/mobile tasks render as a mobile App surface, not a desktop responsive website.
- Entry lock: `index.html` directly renders the app flow; it is not an iframe gallery/overview.
- Named artifact lock: any requested primary file such as `zhuan-*.html` also directly renders the app flow; it is not an iframe gallery/overview.
- Plugin lock: no Web Prototype/plugin snapshot is allowed to override this skill during generation.
- Preview-control lock: route tabs / page switchers / canvas controls are outside `IOSDevice`, not inside the real app UI.
- Action-bar lock: every page-level `ButtonBar` is outside the scrollable content and remains visible at the bottom of the device while content scrolls.
- Toast-position lock: default `Toast` appears in the device center; any bottom toast placement is explicitly justified by keyboard/bottom-bar avoidance.
- Asset-preview lock: mask/image assets such as 官方验 and icon FILE assets are visible in both the raw artifact and OpenDesign file preview; if the preview path blanks an asset, use the DS resolver rather than rewriting the visual.
- Audit integrity lock: `COMPONENT_USAGE_REPORT.md` or the final report must include detected semantics, expected DS components, actual DS calls, forbidden-pattern grep results, and every non-use reason. A report that only lists successful component calls is incomplete.

## Conditional Component Gate

Use this gate for generation and review. It prevents both underuse and over-forcing.

| Detected semantic in brief/output | Expected DS path | Failure signal |
|---|---|---|
| App/iOS/H5 mobile surface | `IOSDevice`, usually `TopNav` | `.phone`, `.status-bar`, `.status-spacer`, fake homebar |
| Product-valid page navigation | React state or valid in-app controls | `screens/*.html` main flow, `window.location.href="screens/..."` |
| Search/filter/category rail | `SearchHeader`, `SortTabs`, `ChipRail`, `Chip`, filter components | `.search-line`, `.chip`, custom filter pills replacing DS |
| Top status/category tabs | `Tabs`, `TabsNav`, `SortTabs` | `function Tabs`, `.tabs` CSS |
| Order/after-sales list card | `OrderCard` or DS subcomponents such as `OrderTrustBadge`, `Price`, `Icon`, `Button` | `.sale-card`, `.trust`, plain official-verification text |
| Official verification / trust mark | `OrderTrustBadge` or DS asset resolver | plain "官方验" text, direct relative `.assets/logo/...` in page code |
| Detail/product summary | `OrderProductCard` / `OrderDetailCards` | list card forced into detail summary, clipped custom product block |
| Form/input/radio/amount/textarea | `FormGroup`, `FormInput`, `FormRadioRow`, `FormVerticalRadioList`, related Form primitives | `.input-row`, `.select-row`, generic form rows replacing DS |
| Bottom sheet/picker/confirmation half-layer | `Sheet`, `SheetRadioList`, `SheetLocationList`, `Sheet actions` | custom sheet chrome, second homebar, cramped manual picker |
| Page-level bottom actions | `ButtonBar` | `.fixed-footer`, action bar inside scroll content |
| Lightweight feedback | `Toast` with timed close | `.toast`, persistent toast, missing `onClose`/duration |

If a semantic is not present, do not require its component. If a semantic is present and a DS component exists, use it. If no DS component matches, compose from DS primitives and explicitly report the gap.

## Preservation Checklist

Use this checklist whenever creating or reviewing an Open Design artifact with the 转转 design system:

- Source fidelity: Does the output use the existing design-system files instead of generic defaults?
- Component reuse: Are chips, buttons, product cards, filters, top nav, bottom bar, icons, forms, dialogs, sheets, price, tags, and order cards reused from existing patterns?
- Token lock: Are brand red, text colors, spacing, radius, and typography from the source tokens?
- Font lock: Is PingFang SC the primary Chinese UI font, with Akrobat only for price digits where supported?
- Icon lock: Are icons referenced through the existing icon component/frame/resources rather than reinvented SVGs?
- Layout fidelity: Does the result preserve the 375px iOS/mobile marketplace density and hierarchy?
- No style drift: No new palette, unrelated font pairing, decorative gradient, glass effect, generic SaaS cards, large marketing hero, emoji, or unsourced illustration.
- Completeness: Does the component keep all expected states, labels, data hierarchy, and edge cases?
- Accessibility basics: Text is legible, controls have adequate hit areas, state is clear, and content does not overlap.

## Built-In Start Instruction

If Open Design asks for a single prompt-style instruction, use this exact rule internally instead of asking the requester to paste it:

```text
使用转转设计系统 v0.2.9，并启用 zhuan-design-system-usage-review v0.1.11。

如果项目仍绑定 Web Prototype / web-prototype / example-web-prototype / appliedPluginSnapshotId，请停止并说明需要先清除该冲突。
如果用户明确要求继续修复或重新生成，则不要沿用 Web Prototype 输出；必须丢弃 iframe/screens/.phone-page/statusbar/homebar 结构，改用转转 DS 组件直接重建。
如果需求包含响应式 Web + iOS / App / H5，请解释为 App 内移动 H5，不得生成桌面响应式网页。

生成前必须复制设计系统运行资源到项目根目录：
tokens.css、fonts、.assets、ui_kits/zhuan-app、DESIGN.md、COMPONENT_USAGE.md、templates/component-first-app。

必须从 templates/component-first-app/index.html 作为启动模板生成。
App/H5 页面必须用 IOSDevice 包裹主流程，并在页面语义需要时真实调用 TopNav、Icon、Button、Tabs、OrderCard、OrderTrustBadge、OrderProductCard、Form、Sheet、Toast、ButtonBar 等 DS 组件。
不要用固定组件清单强制所有项目都出现 Tabs、OrderCard、Form、Sheet 或 ButtonBar；先根据需求语义判断是否需要。语义命中已有 DS 组件时必须调用；语义不存在时不得为了过审硬塞组件。
列表/筛选语义出现时优先调用 SearchHeader、SortTabs、ChipRail；表单/底部操作语义出现时优先调用 FormGroup、FormInput、FormRadioRow、FormVerticalRadioList、ButtonBar。
订单/售后列表卡语义出现时优先调用 OrderCard 或 DS 子组件；订单详情、退款详情、售后详情、售后处理摘要、寄回用户商品摘要语义出现时优先调用 OrderProductCard / OrderDetailCards，不得因为名称相近就把列表型 OrderCard 硬塞为详情摘要卡。
设备状态栏、导航栏、底部操作栏、home indicator 的边界由 IOSDevice / TopNav / ButtonBar / Sheet 负责：顶部 0 间距不重叠，底部 0 间距固定，Sheet 内不得出现第二个 homebar。
链路演示、页面切换、画板导航等预览控件必须放在 IOSDevice 外，不能进入真实手机界面。真实界面里的点击只对应产品需求本身。
页面级 ButtonBar 必须作为屏幕根容器底部兄弟节点固定在设备底部；主体内容单独滚动，禁止把 ButtonBar 放入滚动内容。
滚动内容区若使用 display:flex; flex-direction:column，直接子级卡片/section 必须保持自然高度（flexShrink:0 或等效 block 流），禁止被 flex shrink 压缩，禁止商品图、标题、价格、状态等内容被 overflow:hidden 裁切。
Toast 必须调用 Toast 组件，默认显示在设备中心，禁止手写 .toast 或浮层 div；必须有 1600ms 默认消失逻辑和 onClose/清理状态，长文案最多 2200ms，只有明确避让键盘/底栏时才允许 bottom placement。
图标和品牌标识必须通过 Icon、OrderTrustBadge 等 DS 组件/资源解析器加载，禁止在页面代码里硬写相对 `.assets/...` 路径；生成后必须确认 raw 预览和 OpenDesign 文件预览中官方验/图标不空白。
禁止用 HTML/CSS 手写仿制上述组件，禁止只写 data-component 标记，禁止自建 .phone、.status-spacer、.status-bar、.tabs、.search-line、.fixed-footer 替代已有结构。
index.html 必须直接渲染 App/H5 主流程，禁止做 iframe 总览画板嵌入 screens/*.html。
OpenDesign 要求交付的主 HTML 文件也必须直接渲染 App/H5 主流程，禁止把 zhuan-*.html 做成 iframe 总览。

审查 OpenDesign 项目时必须同时检查左侧对话生成过程、右侧渲染结果、项目文件/设计系统资源，不能只看最终 HTML 是否调用了组件。
自检报告必须列出：识别到的业务语义、该语义应调用的 DS 组件、实际调用情况、未调用原因、禁用模式检查结果。只列“已调用组件”的报告不合格。

如果任一资源无法读取、复制或组件无法调用，立即停止并说明原因，不得降级为手写实现。
```

## Reference Use

Load reference files only as needed:

- `references/harness-workflow.md`: Use for plan mode, task decomposition, self-review loops, and project-rule discipline.
- `references/safe-knowledge-filter.md`: Use to decide which parts of the generic design knowledge base are safe review signals and which parts must not override 转转.

The reference knowledge is advisory. If it conflicts with `转转设计系统codex对话处理`, the 转转 design system wins.

## Review Output Format

When the user asks for review, lead with findings:

```
结论：通过 / 需修正

主要问题：
- [严重度] 问题：说明影响，并指向具体组件/页面。

可保留：
- 已正确复用的转转组件或 token。

建议动作：
- 只列不会改变组件本体样式和完整性的修正。
```

When the user asks to generate or modify an artifact, include the preservation result in the final answer:

```
已使用的转转依据：DESIGN.md / COMPONENT_USAGE.md / tokens.css / templates/component-first-app / ui_kits/zhuan-app / preview/...
运行资源检查：通过；已带入 tokens.css、fonts、.assets、ui_kits/zhuan-app、DESIGN.md、COMPONENT_USAGE.md、templates/component-first-app。
组件调用检查：通过；已按业务语义调用对应 DS 组件，未用 HTML/CSS 仿制。未命中的组件未被强塞，已说明原因。
冲突检查：通过；无 Web Prototype 覆盖，index.html 非 iframe 总览。
还原检查：通过；未新增 token、字体、颜色或组件样式。
```
