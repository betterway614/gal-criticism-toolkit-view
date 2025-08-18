# GAL批评工具箱前端应用

这是一个基于Uniapp开发的GAL游戏批评与推荐平台，采用二次元粉色主题设计。

## 功能特点

- 游戏推荐与搜索
- 游戏详情展示
- 用户评价与评分
- 个人中心管理
- 问卷反馈系统

## 技术栈

- Uniapp（跨平台开发框架）
- Vue.js
- Sass
- Vite

## 主题风格

本项目采用二次元粉色主题设计，具有以下特点：
- 柔和的粉色渐变背景
- 圆角元素设计
- 精美过渡动画
- 少女感十足的色彩搭配

## 项目结构

```
/src
  /pages          # 页面目录
    /recommend    # 游戏推荐页
    /game         # 游戏详情页
    /personal     # 个人中心
    /questionnaire # 问卷页
    /dialogue     # 对话页
    /home         # 首页
  /styles         # 样式文件
  /static         # 静态资源
  App.vue         # 应用入口
  main.js         # 主入口
  pages.json      # 页面配置
```

## 开发说明

1. 克隆项目
2. 安装依赖：`npm install`
3. 开发模式：`npm run dev:h5`
4. 构建生产版本：`npm run build:h5`