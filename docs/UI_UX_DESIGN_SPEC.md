# Galgame 批评工具箱 UI/UX 设计规范 v1.0

本文档基于现有设计文档与样式系统，输出生产级 UI/UX 重设计规范，指导首页、推荐页、个人中心及功能页（对话、问卷、小游戏）的重构落地。

- 参考样式文件：<mcfile name="global.scss" path="src/styles/global.scss"></mcfile>
- 参考设计文档：
  - <mcfile name="STYLE_GUIDE_GAL_RECOMMEND.md" path="docs/STYLE_GUIDE_GAL_RECOMMEND.md"></mcfile>
  - <mcfile name="DESIGN_GAL_RECOMMEND_PIXEL.md" path="docs/DESIGN_GAL_RECOMMEND_PIXEL.md"></mcfile>

## 1. 设计目标与原则
- 面向 galgame 玩家，融合像素风（复古、硬边框、硬阴影）与现代 UI（清晰层级、良好留白、顺畅交互）。
- 在微信小程序/H5 多端一致，采用移动优先与 rpx 单位，保证 8px 基础网格。
- 强调组件化、可复用与性能优化，关注首屏加载和小程序主包大小限制（≤ 2MB）。

设计原则：
- 清晰：信息层级明确、标题与内容区对比强烈。
- 一致：视觉与交互在各页面一致，组件状态统一。
- 直接：动效简洁，反馈及时，避免冗余装饰。
- 可达：触控面积 ≥ 88rpx，文本可读性高，色彩对比度达标。

## 2. 设计令牌（Design Tokens）与变量映射
统一以 <mcfile name="global.scss" path="src/styles/global.scss"></mcfile> 的 CSS 变量为核心令牌，并建立语义别名，方便语义化使用：

- 基础色板（已存在）
  - --pixel-blue / --pixel-pink / --pixel-green / --pixel-yellow / --pixel-purple
  - --pixel-dark / --pixel-light / --pixel-gray
- 阴影（已存在）
  - --pixel-shadow-*: 4px 4px 0 硬阴影，对应像素风格。

语义别名（建议新增于全局样式顶部）：
- --color-primary: var(--pixel-pink)
- --color-secondary: var(--pixel-blue)
- --color-bg-page: var(--pixel-light)
- --color-bg-card: #FFFFFF
- --color-text-primary: var(--pixel-dark)
- --color-text-secondary: rgba(26, 29, 39, 0.75)
- --color-border: #EDEFF3

使用建议：
- 页面背景使用 --color-bg-page；卡片背景使用 --color-bg-card；标题文本使用 --color-text-primary；辅助文案使用 --color-text-secondary。
- 交互主色使用 --color-primary；强调/链接可用 --color-secondary。

## 3. 排版与栅格
- 字体：主要字体 Pixelify Sans；回退 Courier New, monospace。
- 标题层级：
  - H1: 48rpx；H2: 40rpx；H3: 32rpx；正文：28rpx；说明：24rpx。
- 行高：标题 1.3；正文 1.6。
- 栅格：以 8px 网格对齐；组件尺寸与内外间距按 8 的倍数。
- 容器：.container 最大宽 1200px，移动优先，自适应断点见 STYLE_GUIDE。

## 4. 组件规范

4.1 按钮（像素风 + 现代交互）
- 基础类：.pixel-btn（硬边框、硬阴影、hover 上移 2px、active 归位）。
- 状态：default/hover/active/disabled。禁用态降低饱和度与阴影强度。
- 尺寸：
  - md: 高度 88rpx，padding: 0 28rpx；
  - sm: 高度 72rpx，padding: 0 24rpx。

4.2 卡片（列表与信息容器）
- 基础类：.pixel-card（硬边框、硬阴影、无圆角）。
- 头部：.pixel-card-header 使用深色背景与彩色楔形装饰；标题 .pixel-card-title；元信息 .pixel-card-meta。
- 内容：.pixel-card-body 内排版 20~24rpx 间距；摘要使用多行截断，优先 -webkit-line-clamp，H5/小程序均为 WebView 内核，兼容性可接受；对不支持的端降级为固定最大高度 + 渐隐遮罩。
- 变体：.pink/.green/.yellow/.purple 对应不同阴影主色。

4.3 标签（Tag）
- 基础类：.pixel-tag；用于关键词与分类；保持像素硬边、硬阴影。

4.4 表单
- 输入框 .input、选择 .select、勾选 .checkbox 与 STYLE_GUIDE 一致；聚焦边框使用 --color-primary。
- 校验态：error 边框使用 #FF4D4F；success 使用 #52C41A。

4.5 布局网格
- 列表网格：.pixel-cards-grid 使用 auto-fill + minmax 自适应；移动端 1 列，平板 2 列，桌面 3+ 列。

4.6 反馈与占位
- 骨架屏：加载大图列表使用方块/行式 skeleton。
- 空状态：图文并茂，提供主要 CTA（如“去填写问卷”）。
- Toast/Modal：统一使用 uni-ui 组件（uToast/uModal），标题与按钮遵循按钮与标题规范。

## 5. 页面规范

5.1 首页（/pages/home/index）
- 结构：
  - 顶部 Header：Logo（64~96rpx）、主标题（H1，像素阴影）、副标题（次要文本）。
  - 互动入口卡片区：三卡（对话/问卷/游戏），统一卡片高度 240~280rpx，图标 64~80rpx。
  - 次级内容（可选）：最新推荐预览（横向滑动 3~5 项），CTA "查看更多" 跳转推荐页。
- 行为：卡片整体可点击，命中区域≥卡片内边距；点击进入对应功能页。
- 性能：首屏仅加载必要资源；卡片插图按需懒加载。

5.2 推荐页（/pages/recommend/index）
- 结构：
  - 顶部：标题（H2）、筛选与排序（下拉/Segmented），搜索入口。
  - 列表：游戏卡片 3:4 封面、标题、标签、评分/时间等元信息。
- 交互：
  - 滚动加载（分页或上拉加载）；
  - 过滤条件保持显著反馈（徽标/芯片显示当前筛选）。
- 状态：
  - 加载：skeleton 占位；
  - 空：展示引导文案；
  - 异常：错误提示与“重试”按钮。

5.3 个人中心（/pages/personal/index）
- 结构：
  - 头像与昵称区（左头像 120rpx，右侧名称与一句话签名）；
  - 功能分区卡片（对话记录、问卷档案、卡片收藏、资料、偏好标签），使用 .pixel-card 五色变体。
- 交互：卡片进入各自子页；资料编辑使用右上角操作按钮或进入详情页。

5.4 对话页（/pages/dialogue/index）
- 结构：
  - 顶部标题栏 + 历史会话入口（更多）。
  - 消息区：
    - 气泡：用户右侧、AI 左侧；像素边框与硬阴影；
    - 时间分割；
    - 长内容折叠与“展开”。
  - 底部输入区：文本输入、发送、快捷建议（chips）。
- 交互：
  - 发送时按钮 loading；
  - 流式输出动画（逐行/逐块出现）。

5.5 问卷页（/pages/questionnaire/index）
- 结构：标题 + 步骤进度（Step/进度条）；问题卡片（单选/多选/评分/文本）；底部下一步/上一步按钮。
- 交互：
  - 校验：必答题未完成禁止下一步；
  - 提交后显示结果摘要与下一步推荐引导（跳转推荐页）。

5.6 小游戏页（/pages/game/index）
- 结构：玩法规则卡片 + 互动区（按钮/卡片拖拽/点选）。
- 反馈：分数、进度、结束总结。

## 6. 导航与信息架构
- 底部 Tab（已在 <mcfile name="pages.json" path="src/pages.json"></mcfile> 配置）：首页/推荐/我的。
- 页面跳转：子功能使用 uni.navigateTo；返回使用导航栏返回或页面内返回。
- 深链接：H5 可支持 query 以复现状态（如 recommend?sort=hot）。

## 7. 动效与微交互
- 通用：fade-in/slide-in 动画 300~500ms；
- 悬停（H5）：卡片轻微上移 2px、阴影加深；
- 点击：按钮按压还原，阴影缩短；
- 列表加载：瀑布式渐入，避免大范围位移。

## 8. 响应式与跨平台
- rpx 优先；辅助媒体查询用于 H5 宽屏优化（参见 STYLE_GUIDE）。
- 平台差异：
  - 字体：小程序不应外链 Google Fonts，建议在 /src/static/fonts/ 内置字体文件并条件编译；否则回退系统等宽字体；
  - 滚动：小程序页面内滚动区域需设置高度/scroll-view 控件；
  - 图片：使用云存储或 cdn，小程序端谨慎使用 base64 大图。

## 9. 性能与包体优化
- 首屏体积控制：剥离非首屏资源，使用动态 import 分包（H5 与小程序分包策略）。
- 图片：WebP 优先；小图使用内联 SVG；预设尺寸与懒加载；
- 样式：复用像素阴影变量，避免重复定义；
- 字体：内置字体按需子集化；
- 监控：在微信开发者工具性能面板观察首屏时间、内存与掉帧。

## 10. 可用性与可访问性
- 触控区域 ≥ 88rpx；
- 界面元素间距 ≥ 16rpx；
- 颜色对比度 WCAG AA（文本与背景对比度≥4.5:1）；
- 文案：按钮动词化（例如“开始对话”、“去问卷”、“开始游戏”）。

## 11. 状态与异常场景
- Loading：按钮 loading、骨架屏、空白占位三选一或组合，避免布局跳动；
- Empty：插画 + 1~2 句文案 + CTA；
- Error：明确错误原因、重试按钮；
- Offline（H5）：提示网络断开，提供“刷新”。

## 12. 实施清单（分阶段落地）
- P0（结构与交互）：
  - 首页：Header/三卡入口点击可达；
  - 推荐页：筛选 + 列表 + 加载/空状态；
  - 个人中心：基础信息 + 功能卡入口。
- P1（细节与体验）：
  - 对话页：消息气泡与输入区、发送反馈；
  - 问卷页：步骤流与校验；小游戏页：核心玩法框架；
  - 骨架屏、错误态、空态统一风格；
- P2（优化与多端）：
  - 字体内置与子集化，分包优化；
  - 图片与动效优化；
  - 端上差异细化与 A/B 细节迭代。

## 13. 验收标准（节选）
- 视觉一致：各页标题、卡片、按钮风格一致，像素阴影与硬边统一；
- 可达成：所有入口均可点击触达目标页面，交互反馈及时；
- 性能达标：首屏时间、交互流畅度在微信开发者工具性能面板达标，无明显掉帧；
- 跨端一致：H5 与小程序视觉与交互一致，关键功能一致可用；
- 包体达标：主包 ≤ 2MB，资源分包与懒加载生效。

---

说明：本规范与现有像素风样式保持一致，通过语义化令牌与组件规范，将像素美学与现代交互统一起来。后续开发请严格对照本规范进行页面重构与验收。