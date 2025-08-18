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
    rating: 4.7,
    tags: ['策略', '塔防']
  },
  {
    id: 3,
    name: '崩坏：星穹铁道',
    image: 'https://s.coze.cn/image/6kOzwBA0gCg/',
    rating: 4.9,
    tags: ['回合制', '角色扮演']
  },
  {
    id: 4,
    name: '英雄联盟手游',
    image: 'https://s.coze.cn/image/gDiVTnTLJrI/',
    rating: 4.6,
    tags: ['MOBA', '竞技']
  }
]);

// 导航函数
function navigateToHome() {
  router.push('/pages/home/index');
}

function navigateToGame() {
  router.push('/pages/game/index');
}

function navigateToDialogue() {
  router.push('/pages/dialogue/index');
}

function navigateToQuestionnaire() {
  router.push('/pages/questionnaire/index');
}

function navigateToProfile() {
  router.push('/pages/personal/index');
}

function navigateToGameDetail(id) {
  router.push(`/pages/game/detail?id=${id}`);
}
</script>

<style scoped lang="scss">
// 全局样式
.page-container {
  min-height: 100vh;
  background-color: #FFF0F5;
  padding-bottom: 140rpx; // 为底部导航留出空间
}

// 导航栏样式
.navbar {
  height: 100rpx;
  background-color: #F82E8A;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  z-index: 10;
}

.navbar-title {
  color: white;
  font-size: 36rpx;
  font-weight: bold;
  display: flex;
  align-items: center;
  letter-spacing: 1rpx;
}

.icon {
  margin-right: 10rpx;
  font-size: 40rpx;
  color: white;
}

// 功能区域样式
.function-area {
  padding: 30rpx;
}

.function-title {
  color: #F82E8A;
  font-size: 32rpx;
  font-weight: bold;
  margin-bottom: 20rpx;
  display: flex;
  align-items: center;
  letter-spacing: 1rpx;
}

.function-buttons {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.function-btn {
  width: 220rpx;
  height: 220rpx;
  border-radius: 20rpx;
  background-color: white;
  border: 1rpx solid rgba(248, 46, 138, 0.2);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 30rpx;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  transition: all 0.3s ease;
}

.function-btn:active {
  transform: translateY(2rpx);
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.05);
}

.btn-icon {
  font-size: 60rpx;
  color: #F82E8A;
  margin-bottom: 15rpx;
}

.btn-text {
  font-size: 28rpx;
  color: #F82E8A;
  font-weight: bold;
  margin-bottom: 10rpx;
}

.btn-desc {
  font-size: 20rpx;
  color: #666;
  text-align: center;
  width: 80%;
}

// 游戏推荐区域样式
.recommend-area {
  padding: 30rpx;
}

.recommend-title {
  color: #F82E8A;
  font-size: 32rpx;
  font-weight: bold;
  margin-bottom: 20rpx;
  display: flex;
  align-items: center;
  letter-spacing: 1rpx;
}

.game-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 20rpx;
}

.game-card {
  width: 48%;
  background-color: white;
  border-radius: 20rpx;
  overflow: hidden;
  margin-bottom: 20rpx;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  transition: all 0.3s ease;
  border: 1rpx solid rgba(248, 46, 138, 0.2);
}

.game-card:active {
  transform: translateY(2rpx);
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.05);
}

.game-image {
  width: 100%;
  height: 240rpx;
  object-fit: cover;
  border-radius: 20rpx 20rpx 0 0;
}

.game-info {
  padding: 20rpx;
}

.game-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15rpx;
}

.game-name {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.game-rating {
  display: flex;
  align-items: center;
  background-color: rgba(248, 46, 138, 0.1);
  padding: 5rpx 10rpx;
  border-radius: 15rpx;
}

.star-icon {
  font-size: 20rpx;
  color: #FBBF24;
  margin-right: 5rpx;
}

.rating-text {
  font-size: 20rpx;
  font-weight: bold;
  color: #333;
}

.game-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10rpx;
}

.tag {
  font-size: 20rpx;
  padding: 5rpx 15rpx;
  border-radius: 15rpx;
  background-color: rgba(248, 46, 138, 0.1);
  color: #F82E8A;
  border: 1rpx solid rgba(248, 46, 138, 0.2);
}

// 底部导航样式
.tabbar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 120rpx;
  background-color: white;
  display: flex;
  justify-content: space-around;
  align-items: center;
  box-shadow: 0 -2rpx 16rpx rgba(248, 46, 138, 0.1);
  border-top: 1rpx solid rgba(248, 46, 138, 0.2);
  z-index: 10;
}

.tabbar-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #999;
  transition: color 0.3s ease;
}

.tabbar-item.selected {
  color: #F82E8A;
}

.tabbar-item text {
  font-size: 24rpx;
  margin-top: 5rpx;
}
</style>