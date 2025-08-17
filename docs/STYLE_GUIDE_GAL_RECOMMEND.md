# STYLE_GUIDE_GAL_RECOMMEND.md

## 设计理念

本项目目标用户是视觉小说(galgame)玩家，设计风格应体现二次元文化特色，同时保持现代UI的简洁和易用性。整体设计遵循以下原则：
- 色彩鲜明但不刺眼
- 排版清晰易读
- 交互流畅自然
- 视觉元素富有动漫风格
- 响应式设计适配不同设备

## 颜色方案

### 主色调
- **primary**: `#FF6B6B` (珊瑚红) - 代表热情与活力，符合二次元文化的情感表达
- **primary-dark**: `#E84A5F` (深珊瑚红) - 用于主色调的深色变体
- **primary-light**: `#FF8E8E` (浅珊瑚红) - 用于主色调的浅色变体

### 辅助色
- **secondary**: `#4ECDC4` (青绿色) - 用于强调和交互元素
- **secondary-dark**: `#3BA99C` (深青绿色) - 辅助色的深色变体
- **secondary-light**: `#6FFFF6` (浅青绿色) - 辅助色的浅色变体

### 中性色
- **background**: `#F7F7F7` (浅灰色) - 页面背景色
- **card-bg**: `#FFFFFF` (白色) - 卡片背景色
- **text-primary**: `#333333` (深灰色) - 主要文本色
- **text-secondary**: `#666666` (中灰色) - 次要文本色
- **text-tertiary**: `#999999` (浅灰色) -  tertiary文本色
- **border**: `#EEEEEE` (极浅灰色) - 边框色
- **divider**: `#DDDDDD` (分割线色)

### 功能色
- **success**: `#52C41A` (成功绿)
- **warning**: `#FAAD14` (警告黄)
- **error**: `#FF4D4F` (错误红)
- **info**: `#1890FF` (信息蓝)

## 排版规范

### 字体
- **主要字体**: `'Noto Sans SC', sans-serif` - 无衬线字体，清晰易读
- **标题字体**: `'Bangers', cursive` - 活泼有特色的标题字体
- **代码字体**: `'Fira Code', monospace` - 等宽字体

### 字体大小
- **h1**: `36rpx` - 页面大标题
- **h2**: `32rpx` - 章节标题
- **h3**: `28rpx` - 小标题
- **body**: `28rpx` - 正文文本
- **small**: `24rpx` - 小文本
- **caption**: `22rpx` - 说明文字

### 行高
- **标题**: `1.2`
- **正文**: `1.6`
- **按钮文本**: `1.4`

### 字重
- **normal**: `400`
- **medium**: `500`
- **bold**: `700`

## 组件样式

### 按钮
```scss
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 16rpx 32rpx;
  border-radius: 8rpx;
  font-size: 28rpx;
  font-weight: 500;
  transition: all 0.3s ease;
}

.button-primary {
  background-color: $primary;
  color: white;
  &:hover {
    background-color: $primary-dark;
  }
}

.button-secondary {
  background-color: $secondary;
  color: white;
  &:hover {
    background-color: $secondary-dark;
  }
}

.button-outline {
  border: 2rpx solid $primary;
  color: $primary;
  background-color: transparent;
  &:hover {
    background-color: $primary-light;
  }
}

.button-text {
  color: $primary;
  background-color: transparent;
  &:hover {
    background-color: $primary-light;
  }
}
```

### 卡片
```scss
.card {
  background-color: $card-bg;
  border-radius: 16rpx;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.05);
  padding: 24rpx;
  margin-bottom: 24rpx;
  transition: transform 0.3s ease, box-shadow 0.3s ease;

  &:hover {
    transform: translateY(-4rpx);
    box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.1);
  }
}

.card-header {
  margin-bottom: 16rpx;
  padding-bottom: 16rpx;
  border-bottom: 2rpx solid $border;
}

.card-body {
  // 内容样式
}

.card-footer {
  margin-top: 16rpx;
  padding-top: 16rpx;
  border-top: 2rpx solid $border;
}
```

### 表单元素
```scss
.input {
  width: 100%;
  padding: 16rpx 20rpx;
  border: 2rpx solid $border;
  border-radius: 8rpx;
  font-size: 28rpx;
  transition: border-color 0.3s ease;

  &:focus {
    border-color: $primary;
    outline: none;
  }
}

.select {
  width: 100%;
  padding: 16rpx 20rpx;
  border: 2rpx solid $border;
  border-radius: 8rpx;
  font-size: 28rpx;
  background-color: white;
  appearance: none;
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="%23666" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 9 12 15 18 9"></polyline></svg>');
  background-repeat: no-repeat;
  background-position: right 20rpx center;
}

.checkbox {
  width: 28rpx;
  height: 28rpx;
  border: 2rpx solid $border;
  border-radius: 6rpx;
  appearance: none;
  cursor: pointer;

  &:checked {
    background-color: $primary;
    border-color: $primary;
    background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>');
    background-repeat: no-repeat;
    background-position: center;
  }
}
```

## 页面布局

### 首页布局
```scss
.home-container {
  padding: 32rpx;
  display: flex;
  flex-direction: column;
  gap: 32rpx;
}

.home-header {
  text-align: center;
  margin-bottom: 40rpx;
}

.home-title {
  font-size: 48rpx;
  font-weight: bold;
  color: $primary;
  margin-bottom: 16rpx;
}

.home-subtitle {
  font-size: 28rpx;
  color: $text-secondary;
}

.interaction-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300rpx, 1fr));
  gap: 24rpx;
}

.interaction-card {
  @extend .card;
  height: 240rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 24rpx;
  cursor: pointer;
}

.interaction-icon {
  width: 80rpx;
  height: 80rpx;
  margin-bottom: 16rpx;
  color: $primary;
}
```

### 推荐页布局
```scss
.recommend-container {
  padding: 32rpx;
}

.recommend-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24rpx;
}

.recommend-title {
  font-size: 32rpx;
  font-weight: bold;
}

.sort-selector {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.game-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280rpx, 1fr));
  gap: 24rpx;
}

.game-card {
  @extend .card;
  display: flex;
  flex-direction: column;
  height: 400rpx;
}

.game-image {
  width: 100%;
  height: 200rpx;
  object-fit: cover;
  border-radius: 8rpx;
  margin-bottom: 16rpx;
}

.game-title {
  font-size: 28rpx;
  font-weight: bold;
  margin-bottom: 8rpx;
}

.game-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8rpx;
  margin-bottom: 16rpx;
}

.game-tag {
  padding: 4rpx 12rpx;
  background-color: $primary-light;
  color: $primary-dark;
  border-radius: 16rpx;
  font-size: 22rpx;
}
```

### 个人页布局
```scss
.profile-container {
  padding: 32rpx;
}

.profile-header {
  display: flex;
  align-items: center;
  gap: 24rpx;
  margin-bottom: 32rpx;
}

.profile-avatar {
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
  object-fit: cover;
}

.profile-info {
  flex: 1;
}

.profile-name {
  font-size: 32rpx;
  font-weight: bold;
  margin-bottom: 8rpx;
}

.profile-desc {
  font-size: 24rpx;
  color: $text-secondary;
}

.profile-tabs {
  display: flex;
  border-bottom: 2rpx solid $border;
  margin-bottom: 24rpx;
}

.profile-tab {
  padding: 16rpx 24rpx;
  font-size: 28rpx;
  font-weight: 500;
  color: $text-secondary;
  cursor: pointer;
  position: relative;

  &.active {
    color: $primary;
  }

  &.active::after {
    content: '';
    position: absolute;
    bottom: -2rpx;
    left: 0;
    width: 100%;
    height: 4rpx;
    background-color: $primary;
  }
}
```

## 动画效果
```scss
// 淡入动画
.fade-in {
  animation: fadeIn 0.5s ease-in-out;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

// 滑动动画
.slide-in {
  animation: slideIn 0.5s ease-in-out;
}

@keyframes slideIn {
  from { transform: translateY(20rpx); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

// 按钮悬停效果
.button-hover {
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-4rpx);
    box-shadow: 0 8rpx 16rpx rgba(0, 0, 0, 0.1);
  }
}
```

## 响应式设计
```scss
// 移动端适配
@media screen and (max-width: 768px) {
  .home-title {
    font-size: 40rpx;
  }

  .interaction-cards {
    grid-template-columns: 1fr;
  }

  .game-list {
    grid-template-columns: 1fr;
  }
}

// 平板适配
@media screen and (min-width: 769px) and (max-width: 1024px) {
  .interaction-cards {
    grid-template-columns: repeat(2, 1fr);
  }

  .game-list {
    grid-template-columns: repeat(2, 1fr);
  }
}

// 桌面端适配
@media screen and (min-width: 1025px) {
  .interaction-cards {
    grid-template-columns: repeat(3, 1fr);
  }

  .game-list {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

## 图标规范
- 使用 uni-ui 提供的图标库
- 自定义图标使用 SVG 格式
- 图标大小统一使用 40rpx (主要图标)、32rpx (次要图标)、24rpx (小图标)
- 图标颜色与文本颜色保持一致

## 图片规范
- 游戏封面图比例为 3:4
- 用户头像使用圆形，比例为 1:1
- 图片格式优先使用 WebP 格式
- 图片加载时使用骨架屏或占位图
- 实现图片懒加载

## UI组件使用规范
1. 使用 uni-ui 组件库作为基础
2. 自定义组件遵循 Vue 组件规范
3. 组件命名采用 PascalCase 命名法
4. 组件 props 定义清晰，类型明确
5. 组件事件使用 kebab-case 命名法
6. 组件文档完善，包含使用示例

## 实施建议
1. 创建主题变量文件，统一管理颜色和字体
2. 使用 SCSS 预处理器，提高样式代码的复用性和可维护性
3. 实现全局样式重置，确保跨平台一致性
4. 使用 CSS Modules 或 BEM 命名法避免样式冲突
5. 定期进行 UI 评审，确保视觉一致性
6. 关注用户反馈，持续优化视觉体验