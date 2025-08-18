<template>
  <view class="page-container">
    <!-- 顶部导航栏 -->
    <view class="navbar">
      <view class="navbar-title">
        <text class="icon"><i class="fas fa-gamepad"></i></text>
        <text>二次元游戏推荐</text>
      </view>
    </view>

    <!-- 功能入口区 -->
    <view class="function-area">
      <view class="function-title">
        <text class="icon"><i class="fas fa-bolt"></i></text>
        <text>快速入口</text>
      </view>
      <view class="function-buttons">
        <button class="function-btn" @tap="navigateToGame">
          <text class="btn-icon"><i class="fas fa-gamepad"></i></text>
          <text class="btn-text">趣味游戏</text>
          <text class="btn-desc">探索你的游戏偏好</text>
        </button>
        <button class="function-btn" @tap="navigateToDialogue">
          <text class="btn-icon"><i class="fas fa-comment-alt"></i></text>
          <text class="btn-text">开始对话</text>
          <text class="btn-desc">获取个性化推荐</text>
        </button>
        <button class="function-btn" @tap="navigateToQuestionnaire">
          <text class="btn-icon"><i class="fas fa-clipboard-list"></i></text>
          <text class="btn-text">问卷测试</text>
          <text class="btn-desc">精准匹配喜好</text>
        </button>
      </view>
    </view>

    <!-- 热门游戏推荐区 -->
    <view class="recommend-area">
      <view class="recommend-title">
        <text class="icon"><i class="fas fa-fire"></i></text>
        <text>热门游戏推荐</text>
      </view>
      <view class="game-cards">
        <view class="game-card" v-for="game in hotGames" :key="game.id" @tap="navigateToGameDetail(game.id)">
          <image :src="game.image" class="game-image"></image>
          <view class="game-info">
            <view class="game-header">
              <text class="game-name">{{game.name}}</text>
              <view class="game-rating">
                <text class="star-icon"><i class="fas fa-star"></i></text>
                <text class="rating-text">{{game.rating}}</text>
              </view>
            </view>
            <view class="game-tags">
              <text class="tag" v-for="tag in game.tags" :key="tag">{{tag}}</text>
            </view>
          </view>
        </view>
      </view>
    </view>

    <!-- 底部导航 -->
    <tabbar class="tabbar">
      <tabbar-item icon="home" text="首页" selected @tap="navigateToHome"></tabbar-item>
      <tabbar-item icon="message-circle" text="对话" @tap="navigateToDialogue"></tabbar-item>
      <tabbar-item icon="user" text="我的" @tap="navigateToProfile"></tabbar-item>
    </tabbar>
  </view>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

// 初始化路由
const router = useRouter();

// 热门游戏数据
const hotGames = ref([
  {
    id: 1,
    name: '原神',
    image: 'https://s.coze.cn/image/6kOzwBA0gCg/',
    rating: 4.8,
    tags: ['开放世界', '角色扮演']
  },
  {
    id: 2,
    name: '明日方舟',
    image: 'https://s.coze.cn/image/gDiVTnTLJrI/',
    rating: 4.6,
    tags: ['策略', '塔防']
  },
  {
    id: 3,
    name: '崩坏：星穹铁道',
    image: 'https://s.coze.cn/image/12XaZ092gX8/',
    rating: 4.7,
    tags: ['回合制', '科幻']
  },
  {
    id: 4,
    name: '光与夜之恋',
    image: 'https://s.coze.cn/image/9U5mXt69oQK/',
    rating: 4.5,
    tags: ['乙女', '恋爱']
  },
  {
    id: 5,
    name: '恋与制作人',
    image: 'https://s.coze.cn/image/02bRwG36MhL/',
    rating: 4.4,
    tags: ['乙女', '养成']
  },
  {
    id: 6,
    name: '公主连结Re:Dive',
    image: 'https://s.coze.cn/image/07iG709xOaM/',
    rating: 4.3,
    tags: ['角色扮演', '养成']
  }
]);

// 页面导航函数
const navigateToGame = () => {
  router.push('/pages/game/index');
};

const navigateToDialogue = () => {
  router.push('/pages/dialogue/index');
};

const navigateToQuestionnaire = () => {
  router.push('/pages/questionnaire/index');
};

const navigateToGameDetail = (id) => {
  router.push(`/pages/game/index?id=${id}`);
};

const navigateToHome = () => {
  router.push('/pages/home/index');
};

const navigateToProfile = () => {
  router.push('/pages/personal/index');
};
</script>

<style scoped>
/* 页面容器 */
.page-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #FFB6C1 0%, #FFA07A 100%);
  padding-bottom: 100rpx;
}

/* 顶部导航栏 */
.navbar {
  background: rgba(255, 255, 255, 0.95);
  padding: 20rpx 0;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.navbar-title {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 36rpx;
  font-weight: bold;
  color: #333;
}

.navbar-title .icon {
  margin-right: 10rpx;
  color: #FF69B4;
}

/* 功能入口区 */
.function-area {
  margin: 30rpx;
  padding: 20rpx;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 20rpx;
  box-shadow: 0 4rpx 15rpx rgba(0, 0, 0, 0.1);
}

.function-title {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
  font-size: 30rpx;
  font-weight: bold;
  color: #333;
}

.function-title .icon {
  margin-right: 10rpx;
  color: #FF69B4;
}

.function-buttons {
  display: flex;
  justify-content: space-between;
}

.function-btn {
  flex: 1;
  margin: 0 10rpx;
  padding: 20rpx 0;
  background: linear-gradient(135deg, #FF91A4 0%, #FF6B81 100%);
  border: none;
  border-radius: 15rpx;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: 0 4rpx 10rpx rgba(255, 105, 180, 0.3);
  transition: all 0.3s ease;
}

.function-btn:active {
  transform: scale(0.95);
  box-shadow: 0 2rpx 5rpx rgba(255, 105, 180, 0.2);
}

.btn-icon {
  font-size: 40rpx;
  margin-bottom: 10rpx;
}

.btn-text {
  font-size: 26rpx;
  font-weight: bold;
}

.btn-desc {
  font-size: 20rpx;
  opacity: 0.9;
  margin-top: 5rpx;
}

/* 推荐区域 */
.recommend-area {
  margin: 0 30rpx 30rpx;
}

.recommend-title {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
  font-size: 30rpx;
  font-weight: bold;
  color: #333;
}

.recommend-title .icon {
  margin-right: 10rpx;
  color: #FF69B4;
}

.game-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.game-card {
  width: 340rpx;
  margin-bottom: 20rpx;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 20rpx;
  overflow: hidden;
  box-shadow: 0 4rpx 15rpx rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.game-card:active {
  transform: translateY(-5rpx);
  box-shadow: 0 6rpx 20rpx rgba(0, 0, 0, 0.15);
}

.game-image {
  width: 100%;
  height: 200rpx;
  object-fit: cover;
}

.game-info {
  padding: 20rpx;
}

.game-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10rpx;
}

.game-name {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.game-rating {
  display: flex;
  align-items: center;
}

.star-icon {
  color: #FFD700;
  margin-right: 5rpx;
}

.rating-text {
  font-size: 24rpx;
  color: #666;
}

.game-tags {
  display: flex;
  flex-wrap: wrap;
}

.tag {
  font-size: 20rpx;
  background: #FFB6C1;
  color: white;
  padding: 5rpx 15rpx;
  border-radius: 15rpx;
  margin-right: 10rpx;
  margin-bottom: 10rpx;
}

/* 底部导航 */
.tabbar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(255, 255, 255, 0.95);
  display: flex;
  justify-content: space-around;
  padding: 10rpx 0;
  box-shadow: 0 -2rpx 10rpx rgba(0, 0, 0, 0.1);
}

.tabbar-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #999;
}

.tabbar-item.selected {
  color: #FF69B4;
}

/* 动画效果 */
.anime-fade-in {
  animation: fadeIn 0.5s ease-in;
}

.anime-slide-up {
  animation: slideUp 0.5s ease-out;
}

.anime-bounce {
  animation: bounce 0.6s ease-in-out;
}

.anime-pulse {
  animation: pulse 2s infinite;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideUp {
  from {
    transform: translateY(50rpx);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes bounce {
  0%, 20%, 53%, 80%, 100% {
    transform: translate3d(0, 0, 0);
  }
  40%, 43% {
    transform: translate3d(0, -30rpx, 0);
  }
  70% {
    transform: translate3d(0, -15rpx, 0);
  }
  90% {
    transform: translate3d(0, -4rpx, 0);
  }
}

@keyframes pulse {
  0% {
    transform: scale3d(1, 1, 1);
  }
  50% {
    transform: scale3d(1.05, 1.05, 1.05);
  }
  100% {
    transform: scale3d(1, 1, 1);
  }
}

/* 适配不同屏幕尺寸 */
@media screen and (max-width: 768rpx) {
  .game-card {
    width: 100%;
  }
  
  .function-buttons {
    flex-direction: column;
  }
  
  .function-btn {
    margin: 10rpx 0;
  }
}

/* 夜间模式适配 */
@media (prefers-color-scheme: dark) {
  .page-container {
    background: linear-gradient(135deg, #4A1A2C 0%, #3A1A2C 100%);
  }
  
  .navbar {
    background: rgba(26, 26, 26, 0.95);
  }
  
  .navbar-title {
    color: #fff;
  }
  
  .function-area {
    background: rgba(26, 26, 26, 0.9);
  }
  
  .function-title {
    color: #fff;
  }
  
  .game-card {
    background: rgba(26, 26, 26, 0.9);
  }
  
  .game-name {
    color: #fff;
  }
  
  .rating-text {
    color: #ccc;
  }
  
  .tabbar {
    background: rgba(26, 26, 26, 0.95);
  }
}

/* 增强的卡片样式 */
.anime-card {
  position: relative;
  overflow: hidden;
}

.anime-card::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(
    45deg,
    rgba(255, 182, 193, 0) 0%,
    rgba(255, 182, 193, 0.1) 50%,
    rgba(255, 182, 193, 0) 100%
  );
  transform: rotate(30deg);
  animation: shine 6s infinite;
}

@keyframes shine {
  0% {
    transform: translateX(-100%) rotate(30deg);
  }
  20%, 100% {
    transform: translateX(100%) rotate(30deg);
  }
}

/* 标签云动画 */
.tag-cloud {
  display: flex;
  flex-wrap: wrap;
  gap: 10rpx;
}

.tag-cloud .tag {
  animation: float 3s ease-in-out infinite;
}

.tag-cloud .tag:nth-child(2n) {
  animation-delay: 0.5s;
}

.tag-cloud .tag:nth-child(3n) {
  animation-delay: 1s;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5rpx);
  }
}

/* 数字动画 */
.anime-number {
  font-weight: bold;
  background: linear-gradient(135deg, #FF69B4 0%, #FF8DC7 100%);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

/* 评分星星动画 */
.anime-score {
  display: flex;
  align-items: center;
}

.anime-score-star {
  color: #FFD700;
  margin-right: 5rpx;
  animation: starPulse 2s infinite;
}

.anime-score-star:nth-child(2n) {
  animation-delay: 0.3s;
}

@keyframes starPulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
}

.anime-score-value {
  font-size: 30rpx;
  font-weight: bold;
  color: #FF91A4;
}

.anime-footer {
  padding: 30rpx;
  text-align: center;
}

.anime-footer-text {
  font-size: 24rpx;
  color: #999999;
}
</style>