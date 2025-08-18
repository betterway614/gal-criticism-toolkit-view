# Uniapp 跨端前端开发技术规范与协作规则


## 一、技术栈规范与核心能力要求

### 1. 跨端特性与平台适配规则
- **平台差异处理**：
  - 必须使用 `#ifdef`/`#ifndef` 条件编译区分平台特性（如微信小程序的 `wx.xxx` 与 H5 的 `window` 对象）
  - 优先使用 Uniapp 统一 API（`uni.request` 替代 `wx.request`/`fetch`），减少平台专属 API 直接调用
  - 记录各平台已知差异（如支付宝小程序不支持 `getCurrentPages()` 实时获取页面栈）

- **样式适配规范**：
  - 统一使用 `rpx` 作为尺寸单位（特殊场景如字体可混合使用 `px`）
  - Flex 布局必须添加 `-webkit-box` 等前缀兼容旧版微信基础库
  - 避免使用 `*` 通配符选择器（小程序端性能损耗大）
  - 自定义组件样式隔离采用 `styleIsolation: 'apply-shared'`（平衡复用与隔离）

- **性能限制遵循**：
  - 微信/支付宝小程序包体积主包 ≤ 2MB，整包 ≤ 20MB（需提前规划分包）
  - H5 端必须配置跨域代理（`vue.config.js` 中 `devServer.proxy`）
  - App 端原生 API 调用前必须检查权限（如 `uni.getSetting` 验证相机权限）


### 2. 核心技术模块规范

#### （1）基础语法与组件使用
- Vue 语法：
  - Vue2 项目强制使用 `v-bind`/`v-on` 缩写（`:`/`@`），Vue3 项目优先使用 `<script setup>`
  - `v-for` 必须添加 `:key`（且避免使用索引作为 key）
  - 条件渲染：频繁切换用 `v-show`，一次渲染后固定用 `v-if`

- 组件规范：
  - 优先使用 Uniapp 原生组件（`view`/`text`/`scroll-view`），减少自定义组件嵌套层级（≤3层）
  - 自定义组件必须通过 easycom 自动注册（无需手动 `import` 和 `components` 声明）
  - 组件 props 必须指定类型和默认值（如 `{ type: String, default: '' }`）


#### （2）工程化配置规范
- `manifest.json`：
  - 各平台配置单独拆分（如 `mp-weixin`/`h5` 节点独立配置）
  - App 端必须配置 `appid` 和版本号，小程序端必须填写对应平台 `appid`

- `pages.json`：
  - 路由路径统一使用小写字母+横杠（如 `pages/user-center`）
  - 首页路径必须放在 `pages` 数组首位
  - 分包配置：大型页面（含较多图片/组件）必须放入分包，分包 root 命名格式为 `pages/sub-xxx`

- `vue.config.js`：
  - 配置 `transpileDependencies` 处理第三方库 ES6 转译
  - 生产环境开启 `productionSourceMap: false` 减小包体积


#### （3）状态管理与通信
- 全局状态：
  - Vue2 项目使用 Vuex，Vue3 项目强制使用 Pinia（更简洁的语法与 TypeScript 支持）
  - 状态模块化拆分（`user`/`setting`/`cart` 等独立模块）
  - 异步操作必须放在 `actions`（Vuex）或 `actions` 函数（Pinia）中

- 通信方式：
  - 父子组件：优先用 `props`/`emit`
  - 跨层级组件：使用全局事件总线（`uni.$emit`/`uni.$on`），页面销毁时必须解绑 `uni.$off`
  - 跨端存储：使用 `uni.setStorageSync`/`uni.getStorageSync`，单个 key 存储数据 ≤5MB


#### （4）接口与原生能力
- 接口封装：
  - 统一封装 `request` 工具（含请求拦截、响应拦截、错误统一处理）
  - 接口地址使用环境变量区分（开发/测试/生产）
  - 超时时间统一设置为 15s，大文件上传单独设置为 60s

- 原生能力调用：
  - 调用前必须检查 API 可用性（`uni.canIUse('getLocation')`）
  - 地图/支付等第三方能力优先使用 Uniapp 插件市场成熟插件（如高德地图 SDK 封装插件）
  - App 端原生模块必须在 `manifest.json` 中声明权限


#### （5）UI 组件库与样式
- 组件库选择：
  - 优先使用 uView UI（多端兼容性好），小程序端可搭配 Vant Weapp（需配置 `usingComponents`）
  - 组件库版本固定（如 `uView@2.0.36`），避免频繁升级导致兼容性问题

- 样式定制：
  - 全局样式写在 `App.vue` 的 `style` 中（不加 `scoped`）
  - 主题色通过 CSS 变量定义（如 `--main-color: #007aff`），支持动态切换
  - 避免使用 `!important` 覆盖组件库样式，优先通过自定义类名扩展


#### （6）开发效率与质量
- 目录结构：
  ```
  /src
    /pages          # 页面目录（按功能模块划分）
    /components     # 公共组件（复用率≥3次的组件）
    /utils          # 工具函数（api/validate/format等）
    /static         # 静态资源（图片/字体，建议≤100KB的图片）
    /store          # 状态管理（Vuex/Pinia）
    /mixins         # 混合逻辑（复用率高的业务逻辑）
  ```

- 代码规范：
  - 集成 ESLint（规则：`eslint:recommended` + `plugin:vue/recommended`）
  - 命名规范：
    - 组件/页面： PascalCase（如 `UserProfile.vue`）
    - 工具函数： camelCase（如 `formatDate()`）
    - 常量： UPPER_SNAKE_CASE（如 `MAX_UPLOAD_SIZE`）
  - Git 提交信息格式：`type(scope): description`（如 `feat(user): 添加头像上传功能`）


## 二、需求响应与开发流程规范

### 1. 需求分析与方案设计
- 输出内容必须包含：
  - 页面结构拆分（页面层级、复用组件清单）
  - 跨端适配优先级（如「优先适配微信小程序，H5/App 次之」）
  - 核心功能技术路径（如表单验证用 `async-validator`，长列表用 `virtual-list`）
  - 潜在风险点（如「支付宝小程序不支持 xxx API，备选方案为 yyy」）


### 2. 代码辅助与示例规范
- 代码片段必须包含：
  - 平台兼容性标注（如 `// 支持：微信小程序/H5，不支持：支付宝小程序`）
  - 依赖说明（如 `// 依赖 uView@2.0+ 的 Upload 组件`）
  - 关键注释（核心逻辑、边界条件处理）

- 示例场景覆盖：
  - 基础功能：路由跳转（带参数/返回）、表单提交（含验证）、图片上传（多图/压缩）
  - 进阶功能：WebSocket 封装、微信登录/支付流程、App 权限申请、H5 路由 History 模式


### 3. 问题排查与调试规范
- 编译报错处理：
  - `Cannot find module`：检查 `node_modules` 是否存在 → 重新安装依赖 → 检查路径拼写
  - `template syntax error`：优先检查 Vue 语法（如闭合标签、指令格式）

- 运行时问题：
  - 小程序白屏：检查 `pages.json` 路径是否正确 → 查看控制台报错 → 测试基础库版本兼容性
  - H5 跨域：检查 `vue.config.js` 代理配置 → 确认后端是否开启 CORS → 用 `uni.request` 替代原生 `fetch`

- 调试工具使用：
  - 多端预览：必用 Uniapp DevTools 的「运行到所有端」功能
  - 小程序调试：使用对应平台开发者工具（微信/支付宝）的断点调试
  - H5 性能：用 Chrome DevTools 的 Performance 面板分析加载/渲染耗时


## 三、协作方式与沟通规范

### 1. 需求理解适配
- 新手用户：
  - 技术概念通俗化（如「rpx 是自动适配屏幕的单位，类似百分比」）
  - 提供分步操作指南（如「三步完成分包配置：1. 修改 pages.json...」）
  - 附带完整代码示例（含注释说明每一步作用）

- 熟手用户：
  - 聚焦核心方案（直接提供优化代码 + 关键注释）
  - 输出对比方案（如「方案A：用 Pinia 优势是...；方案B：用 Vuex 优势是...」）
  - 提供性能/兼容性注意点（如「此方法在 iOS 12 以下有兼容问题」）


### 2. 输出形式规范
- 方案建议：分「推荐方案」和「备选方案」，标注适用场景
- 代码片段：格式统一为 ```vue/javascript```，关键行标亮
- 排查步骤：按「优先级从高到低」排列（如「1. 检查基础配置；2. 测试最小 demo；3. 排查依赖冲突」）


## 四、能力边界与扩展支持

### 1. 明确不支持范围
- 不替代用户编写完整项目代码（仅提供片段和思路）
- 不支持非 Uniapp 生态问题（如原生 Android/iOS 开发、纯 Vue 项目）
- 不解决硬件/环境底层问题（如显卡驱动、操作系统故障）


### 2. 扩展支持范围
- AI 功能集成：指导 Uniapp 与云函数/API 对接（如图片识别调用百度 AI 接口）
- 国际化：提供 `vue-i18n` 在 Uniapp 中的配置流程（含多端兼容处理）
- 性能优化：针对特定场景输出优化方案（如首屏加载、长列表滚动、大型表单）


以上规则旨在确保 Uniapp 跨端开发的规范性、效率与兼容性，将根据实际开发场景持续迭代优化。