<template>
  <view class="personal-container">
    <view class="anime-title">个人中心</view>
    <view class="anime-cards-grid">
      <view class="anime-card" @click="navigateTo('/pages/dialogue/index')">
        <view class="anime-card-header">
          <view class="anime-card-title">对话列表</view>
        </view>
      </view>
      <view class="anime-card" @click="navigateTo('/pages/game/index')">
        <view class="anime-card-header">
          <view class="anime-card-title">宾果详情</view>
        </view>
      </view>
      <view class="anime-card" @click="navigateTo('/pages/recommend/index')">
        <view class="anime-card-header">
          <view class="anime-card-title">卡片列表</view>
        </view>
      </view>
      <view class="anime-card">
        <view class="anime-card-header">
          <view class="anime-card-title">个人档案</view>
        </view>
      </view>
      <view class="anime-card">
        <view class="anime-card-header">
          <view class="anime-card-title">收藏标签</view>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
function navigateTo(path) {
  uni.navigateTo({
    url: path
  })
}
</script>

<style scoped>
.personal-container {
  padding: 40rpx;
  min-height: 100vh;
  background-color: var(--color-bg-page);
}

.anime-cards-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20rpx;
  margin-top: 30rpx;
}

.anime-card {
  background-color: white;
  border-radius: var(--radius-md);
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  border: 1rpx solid var(--color-border);
  transition: all 0.3s ease;
  overflow: hidden;
  height: 180rpx;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.anime-card:active {
  transform: translateY(2rpx);
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.05);
}

.anime-card-header {
  padding: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.anime-card-title {
  font-size: var(--font-size-md);
  font-weight: 600;
  color: var(--color-text-primary);
  letter-spacing: 1rpx;
}
</style>