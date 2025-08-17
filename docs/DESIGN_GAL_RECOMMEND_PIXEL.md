# 像素风格设计指导文档

## 设计理念

像素风格（Pixel Style）是一种以像素为基本单位构建的视觉设计风格，具有鲜明的复古感和独特的视觉冲击力。本设计系统融合了现代UI设计原则与经典像素艺术风格，打造出既怀旧又现代的用户体验。

## 颜色系统

### 主色调
```scss
$pixel-blue: #2D7FF9;    // 像素蓝
$pixel-pink: #F82E8A;    // 像素粉
$pixel-green: #00CC88;   // 像素绿
$pixel-yellow: #FFBE0B;  // 像素黄
$pixel-purple: #8A2BE2;  // 像素紫
```

### 中性色
```scss
$pixel-dark: #1A1D27;    // 深色
$pixel-light: #F4F6FC;   // 浅色
$pixel-gray: #E2E8F0;    // 灰色
$pixel-white: #FFFFFF;   // 白色
$pixel-black: #000000;   // 黑色
```

## 排版系统

### 字体
- 主要字体：`Pixelify Sans`（像素风格字体）
- 备用字体：`Courier New`, `monospace`

### 字体大小
```scss
$font-size-xs: 12px;     // 超小
$font-size-sm: 14px;     // 小
$font-size-base: 16px;   // 基础
$font-size-lg: 20px;     // 大
$font-size-xl: 24px;     // 超大
$font-size-xxl: 32px;    // 特大
```

### 字体粗细
```scss
$font-weight-normal: 400;
$font-weight-medium: 500;
$font-weight-bold: 700;
```

## 布局系统

### 间距
```scss
$spacing-xs: 4px;        // 超小间距
$spacing-sm: 8px;        // 小间距
$spacing-base: 16px;     // 基础间距
$spacing-lg: 24px;       // 大间距
$spacing-xl: 32px;       // 超大间距
```

### 像素网格
- 所有UI元素应基于8px网格对齐
- 组件尺寸应为8px的倍数

## 组件设计

### 按钮
```scss
.pixel-btn {
  display: inline-block;
  padding: $spacing-base $spacing-lg;
  font-family: 'Pixelify Sans', monospace;
  font-size: $font-size-base;
  font-weight: $font-weight-bold;
  text-align: center;
  color: $pixel-white;
  background-color: $pixel-blue;
  border: 3px solid $pixel-black;
  box-shadow: 4px 4px 0 $pixel-black;
  transition: all 0.2s ease;
  cursor: pointer;

  &:hover {
    transform: translate(-2px, -2px);
    box-shadow: 6px 6px 0 $pixel-black;
  }

  &:active {
    transform: translate(0, 0);
    box-shadow: 2px 2px 0 $pixel-black;
  }
}
```

### 卡片
```scss
.pixel-card {
  background-color: $pixel-white;
  border: 3px solid $pixel-black;
  box-shadow: 4px 4px 0 $pixel-blue;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;

  &:hover {
    transform: translate(-2px, -2px);
    box-shadow: 6px 6px 0 $pixel-blue;
  }

  // 卡片颜色变体
  &.pink {
    box-shadow: 4px 4px 0 $pixel-pink;

    &:hover {
      box-shadow: 6px 6px 0 $pixel-pink;
    }
  }

  &.green {
    box-shadow: 4px 4px 0 $pixel-green;

    &:hover {
      box-shadow: 6px 6px 0 $pixel-green;
    }
  }

  // 其他颜色变体...
}
```

### 标签
```scss
.pixel-tag {
  display: inline-block;
  padding: $spacing-xs $spacing-sm;
  background-color: $pixel-gray;
  color: $pixel-dark;
  font-family: 'Pixelify Sans', monospace;
  font-size: $font-size-xs;
  font-weight: $font-weight-bold;
  border: 2px solid $pixel-black;
  box-shadow: 2px 2px 0 $pixel-black;
}
```

## 交互效果

### 悬停效果
- 元素悬停时上移2px，阴影增大
- 按钮悬停时颜色加深

### 点击效果
- 元素点击时下移回原位，阴影减小
- 提供明显的视觉反馈

## 像素风格图标
- 使用8位风格图标
- 保持简洁明了的视觉语言
- 图标尺寸应为16x16, 24x24, 32x32像素

## 应用示例

### 推荐卡片组件
```vue
<template>
  <div class="pixel-card {{ colorVariant }}">
    <div class="pixel-card-header">
      <h3 class="pixel-card-title">{{ game.title }}</h3>
      <div class="pixel-card-meta">
        <span>{{ game.releaseDate }}</span>
        <span>{{ game.rating }} ★</span>
      </div>
    </div>
    <div class="pixel-card-body">
      <img :src="game.cover" alt="{{ game.title }}" class="pixel-card-image">
      <p class="pixel-card-description">{{ game.description }}</p>
      <div class="pixel-card-tags">
        <span class="pixel-tag" v-for="tag in game.tags" :key="tag">{{ tag }}</span>
      </div>
    </div>
  </div>
</template>
```

## 实现规范

1. 所有元素必须有清晰的像素边界
2. 避免使用圆角，保持直角风格
3. 阴影应使用硬阴影而非模糊阴影
4. 颜色应使用鲜明的8位风格色彩
5. 字体应选择像素风格字体
6. 动画应保持简单直接，避免过度平滑

## 设计资源

- 像素风格字体: [Pixelify Sans](https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400;500;600;700&display=swap)
- 像素图标库: 使用自定义8位风格图标
- 颜色选择器: 确保颜色值为整数，避免渐变

这个设计系统将帮助开发者创建统一、美观的像素风格界面，同时保持代码的可维护性和扩展性。