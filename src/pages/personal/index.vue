<template>
  <view class="personal-container">
    <!-- 顶部个人档案区域 -->
    <view class="profile-section">
      <view class="profile-avatar">
        <image class="avatar-img" src="/static/avatar-default.png" mode="aspectFill"></image>
      </view>
      <view class="profile-info">
        <view class="profile-name">用户名</view>
      </view>
      <view class="profile-more">
        <image src="/static/icons/next-icon.png" mode="aspectFit"></image>
      </view>
    </view>

    <!-- 功能菜单区域 -->
    <view class="function-menu">
      <view class="menu-grid">
        <view class="menu-item" @click="navigateTo('/pages/dialogue/index')">
          <view class="menu-icon">
            <image src="/static/icons/dialogue.png" mode="aspectFit"></image>
          </view>
          <view class="menu-text">对话列表</view>
        </view>
        <view class="menu-item" @click="navigateTo('/pages/game/index')">
          <view class="menu-icon">
            <image src="/static/icons/game.png" mode="aspectFit"></image>
          </view>
          <view class="menu-text">宾果详情</view>
        </view>
        <view class="menu-item" @click="navigateTo('/pages/recommend/index')">
          <view class="menu-icon">
            <image src="/static/icons/card.png" mode="aspectFit"></image>
          </view>
          <view class="menu-text">卡片列表</view>
        </view>
        <view class="menu-item" @click="navigateTo('/pages/profile-detail/index')">
          <view class="menu-icon">
            <image src="/static/icons/profile.png" mode="aspectFit"></image>
          </view>
          <view class="menu-text">个人档案</view>
        </view>
        <view class="menu-item" @click="navigateTo('/pages/tags/index')">
          <view class="menu-icon">
            <image src="/static/icons/star.png" mode="aspectFit"></image>
          </view>
          <view class="menu-text">收藏标签</view>
        </view>
        <view class="menu-item" @click="navigateTo('/pages/settings/index')">
          <view class="menu-icon">
            <image src="/static/icons/setting.png" mode="aspectFit"></image>
          </view>
          <view class="menu-text">设置</view>
        </view>
      </view>
    </view>

    <!-- 底部退出登录按钮 -->
    <view class="logout-section">
      <button class="logout-btn" @click="logout">退出登录</button>
    </view>
  </view>
</template>

<script setup>
function navigateTo(path) {
  // 检查页面是否存在
  uni.navigateTo({
    url: path,
    fail: (err) => {
      console.log('页面不存在:', err);
      uni.showToast({
        title: '功能开发中',
        icon: 'none'
      });
    }
  })
}

function logout() {
  uni.showModal({
    title: '提示',
    content: '确定要退出登录吗？',
    success: (res) => {
      if (res.confirm) {
        // 清除登录状态
        uni.removeStorageSync('userInfo');
        uni.removeStorageSync('token');
        // 跳转到登录页
        uni.reLaunch({
          url: '/pages/login/index'
        });
      }
    }
  });
}
</script>

<style scoped>
.personal-container {
  min-height: 100vh;
  background-color: var(--color-bg-page);
  display: flex;
  flex-direction: column;
}

/* 顶部个人档案样式 */
.profile-section {
  background-color: var(--color-primary);
  padding: 40rpx 30rpx;
  display: flex;
  align-items: center;
  position: relative;
}

.profile-more {
  margin-left: auto;
  padding: 20rpx;
}

.profile-more image {
  width: 24rpx;
  height: 36rpx;
  filter: brightness(0) invert(1);
}

.level-badge {
  position: absolute;
  bottom: 0;
  right: 0;
  background-color: var(--color-secondary);
  color: white;
  font-size: 20rpx;
  padding: 4rpx 10rpx;
  border-radius: 10rpx 0 0 0;
  font-weight: bold;
}

.profile-avatar {
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  overflow: hidden;
  border: 4rpx solid rgba(255, 255, 255, 0.8);
  margin-right: 30rpx;
}

.avatar-img {
  width: 100%;
  height: 100%;
}

.profile-info {
  color: white;
}

.profile-name {
  font-size: 36rpx;
  font-weight: 600;
  margin-bottom: 10rpx;
}

.profile-level {
  font-size: 24rpx;
  opacity: 0.9;
  margin-bottom: 8rpx;
}

.profile-score {
  font-size: 28rpx;
  opacity: 0.9;
}

/* 功能菜单样式 */
.function-menu {
  flex: 1;
  padding: 30rpx;
}

.menu-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 30rpx;
}

.menu-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20rpx 0;
}

.menu-icon {
  width: 80rpx;
  height: 80rpx;
  border: 2px solid var(--color-border);
  border-radius: 2px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 15rpx;
  background-color: rgba(255, 255, 255, 0.8);
}

.menu-icon image {
  width: 50rpx;
  height: 50rpx;
  opacity: 0.8;
}

.menu-text {
  font-size: 24rpx;
  color: var(--color-text-primary);
  text-align: center;
}

/* 底部退出按钮样式 */
.logout-section {
  padding: 30rpx;
  margin-bottom: 30rpx;
}

.logout-btn {
  width: 100%;
  height: 90rpx;
  line-height: 90rpx;
  background-color: white;
  color: var(--color-danger);
  font-size: 32rpx;
  border-radius: 45rpx;
  border: 1rpx solid var(--color-danger);
  transition: all 0.3s ease;
}

.logout-btn:active {
  background-color: var(--color-danger);
  color: white;
}
</style>