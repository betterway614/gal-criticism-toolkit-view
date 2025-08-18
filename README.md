# Galgame 批评工具箱前端应用

## 项目简介

Galgame 批评工具箱是一款专为Galgame爱好者打造的探索、分析、发现游戏偏好的跨端应用。通过智能对话、偏好问卷和趣味游戏等多种交互方式，帮助用户深入了解自己的游戏喜好，并提供个性化的游戏推荐。

## 功能特点

- **智能对话**：与AI助手深度交流，发现你的游戏偏好
- **偏好问卷**：通过科学问卷快速构建个人画像
- **趣味游戏**：在游戏中探索隐藏的喜好倾向
- **游戏推荐**：基于你的偏好提供个性化的游戏推荐
- **个人中心**：管理你的个人信息和历史记录

## 技术栈

- **前端框架**：Uniapp 3.0
- **构建工具**：Vite
- **样式语言**：SCSS
- **跨端支持**：微信小程序、H5、App等多平台

## 主题风格

项目采用二次元粉色主题设计，融合了现代化的UI元素和动效，提供沉浸式的用户体验。

## 项目结构

```
/src
  /pages         # 页面目录（按功能模块划分）
    /dialogue    # 智能对话页面
    /game        # 趣味游戏页面
    /home        # 首页
    /personal    # 个人中心页面
    /questionnaire # 偏好问卷页面
    /recommend   # 游戏推荐页面
  /static        # 静态资源（图片、logo等）
  /styles        # 全局样式文件
  App.vue        # 应用入口组件
  main.js        # 应用入口文件
  manifest.json  # 应用配置文件
  pages.json     # 页面路由配置
  uni.scss       # 全局样式变量文件
```

## 开发说明

### 安装依赖

```bash
npm install
```

### 开发运行

```bash
# H5开发
npm run dev:h5

# 微信小程序开发
npm run dev:mp-weixin

# 其他平台开发
npm run dev:mp-xxx  # xxx为对应平台代码
```

### 构建打包

```bash
# H5打包
npm run build:h5

# 微信小程序打包
npm run build:mp-weixin

# 其他平台打包
npm run build:mp-xxx  # xxx为对应平台代码
```

## 开发团队

Galgame 批评工具箱开发团队

## License

MIT License