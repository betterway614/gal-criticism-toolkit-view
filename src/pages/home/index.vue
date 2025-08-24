<template>
  <view class="home-container fade-in" :style="{ 'background-image': 'linear-gradient(135deg, var(--color-bg-page) 0%, #FFE6F0 100%)' }">
    <!-- 装饰性元素 -->
    <view class="anime-decoration top-left"></view>
    <view class="anime-decoration top-right"></view>
    <view class="anime-decoration bottom-left"></view>
    <view class="anime-decoration bottom-right"></view>

    <!-- 页面头部 -->
    <view class="home-header" :style="{ 'background-color': 'rgba(255, 255, 255, 0.8)', 'backdrop-filter': 'blur(10rpx)' }">
      <image src="/static/logo.png" class="home-logo bounce-in" />
      <view class="home-title anime-title glow-text">Galgame 批评工具箱</view>
      <view class="home-subtitle slide-up">探索、分析、发现你的 Galgame 宇宙</view>
    </view>

    <!-- 三大功能入口 -->
    <view class="interaction-section">
      <view class="section-title slide-up" :style="{ 'animation-delay': '0.1s' }">开始你的探索之旅</view>
      <view class="interaction-cards">
        <view 
          class="interaction-card anime-card pink slide-in" 
          :style="{ 'animation-delay': '0.1s' }"
          @click="navigateTo('/pages/dialogue/index')"
        >
          <view class="interaction-icon">💬</view>
          <view class="interaction-title">智能对话</view>
          <view class="interaction-desc">与AI助手深度交流，发现你的游戏偏好</view>
        </view>
        
        <view 
          class="interaction-card anime-card green slide-in"
          :style="{ 'animation-delay': '0.2s' }"
          @click="navigateTo('/pages/questionnaire/index')"
        >
          <view class="interaction-icon">📝</view>
          <view class="interaction-title">偏好问卷</view>
          <view class="interaction-desc">通过科学问卷快速构建个人画像</view>
        </view>
        
        <view 
          class="interaction-card anime-card yellow slide-in"
          :style="{ 'animation-delay': '0.3s' }"
          @click="navigateTo('/pages/game/index')"
        >
          <view class="interaction-icon">🎮</view>
          <view class="interaction-title">趣味游戏</view>
          <view class="interaction-desc">在游戏中探索隐藏的喜好倾向</view>
        </view>
      </view>
    </view>

    <!-- 最新推荐区 -->
    <view class="recommend-section">
      <view class="section-header">
        <view class="section-title">热门推荐</view>
        <view class="section-more" @click="navigateTo('/pages/recommend/index')">
          查看更多 →
        </view>
      </view>
      
      <!-- 加载状态 -->
      <view v-if="recommendState.loading" class="anime-scroll-x">
        <view class="anime-scroll-item" v-for="i in 3" :key="i">
          <view class="anime-skeleton card"></view>
        </view>
      </view>
      
      <!-- 错误状态 -->
      <view v-else-if="recommendState.error" class="anime-error">
        <view class="anime-error-icon">⚠️</view>
        <view class="anime-error-text">{{ recommendState.error }}</view>
        <button class="anime-btn sm" @click="loadRecommendations">重试</button>
      </view>
      
      <!-- 空状态 -->
      <view v-else-if="!recommendState.data.length" class="anime-empty">
        <view class="anime-empty-icon">📚</view>
        <view class="anime-empty-text">暂无推荐内容</view>
        <button class="anime-btn sm" @click="navigateTo('/pages/questionnaire/index')">
          去填写问卷
        </button>
      </view>
      
      <!-- 推荐内容 -->
      <view v-else class="anime-scroll-x">
        <view 
          class="anime-scroll-item recommend-card"
          v-for="(item, index) in recommendState.data" 
          :key="item.id"
          @click="handleRecommendClick(item)"
        >
          <view class="recommend-media">
            <image class="recommend-cover" :src="item.cover" :alt="item.title" />
            <view class="recommend-badge">★ {{ item.rating }}</view>
          </view>
          <view class="recommend-info">
            <view class="recommend-title">{{ item.title }}</view>
            <view class="recommend-tags">
              <view class="anime-tag" v-for="tag in item.tags.slice(0, 2)" :key="tag">
                {{ tag }}
              </view>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// 状态管理
const recommendState = ref({
  loading: false,
  error: null,
  data: []
})

// 模拟推荐数据
const mockRecommendData = [
  {
    id: 1,
    title: "CLANNAD",
    cover: "/static/images/game1.jpg",
    rating: 9.2,
    tags: ["校园", "治愈", "感动"]
  },
  {
    id: 2,
    title: "Steins;Gate",
    cover: "/static/images/game2.jpg", 
    rating: 9.5,
    tags: ["科幻", "悬疑", "时间旅行"]
  },
  {
    id: 3,
    title: "Little Busters!",
    cover: "/static/images/game3.jpg",
    rating: 8.8,
    tags: ["青春", "友情", "棒球"]
  }
]

// 页面跳转
function navigateTo(url) {
  try {
    console.log('尝试跳转到:', url);
    
    // 检查是否是tabBar页面（增强版判断逻辑，适应不同格式的URL）
    const tabBarPagePaths = ['pages/dialogue/index', 'pages/home/index', 'pages/personal/index'];
    
    // 标准化URL，移除可能的斜杠前缀
    let normalizedUrl = url.startsWith('/') ? url.substring(1) : url;
    console.log('标准化后的URL:', normalizedUrl);
    
    // 增加日志，显示当前URL和tabBarPages内容
    console.log('tabBar页面列表:', tabBarPagePaths);
    console.log('是否为tabBar页面:', tabBarPagePaths.includes(normalizedUrl));
    
    if (tabBarPagePaths.includes(normalizedUrl)) {
      // 对于tabBar页面，强制使用switchTab，确保路径格式正确
      console.log('确认是tabBar页面，使用switchTab跳转');
      // 确保URL格式正确，添加斜杠前缀
      const switchTabUrl = normalizedUrl.startsWith('/') ? normalizedUrl : '/' + normalizedUrl;
      console.log('最终switchTab URL:', switchTabUrl);
      
      // 使用强制的switchTab调用
      uni.switchTab({
        url: switchTabUrl,
        success: () => {
          console.log('tabBar页面跳转成功:', switchTabUrl);
        },
        fail: (err) => {
          console.error('tabBar页面跳转失败:', err);
          uni.showToast({
            title: '切换Tab失败，请重试',
            icon: 'none'
          });
        }
      });
    } else {
      // 对于非tabBar页面，使用navigateTo
      console.log('确认是非tabBar页面，使用navigateTo跳转');
      uni.navigateTo({
        url: url,
        success: () => {
          console.log('普通页面跳转成功:', url);
        },
        fail: (err) => {
          console.error('普通页面跳转失败:', err);
          uni.showToast({
            title: '跳转失败，请重试',
            icon: 'none'
          });
        }
      });
    }
  } catch (error) {
    console.error('导航函数异常:', error);
    uni.showToast({
      title: '导航异常，请重试',
      icon: 'none'
    });
  }
}

// 加载推荐内容
async function loadRecommendations() {
  recommendState.value.loading = true
  recommendState.value.error = null
  
  try {
    // 模拟API调用
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // 随机决定是否返回数据
    if (Math.random() > 0.2) {
      recommendState.value.data = mockRecommendData
    } else {
      throw new Error('网络连接失败，请重试')
    }
  } catch (error) {
    recommendState.value.error = error.message
  } finally {
    recommendState.value.loading = false
  }
}

// 处理推荐点击
function handleRecommendClick(item) {
  uni.showToast({
    title: `查看《${item.title}》详情`,
    icon: 'none'
  })
  // 实际项目中这里会跳转到游戏详情页
}

// 页面加载时获取推荐内容
onMounted(() => {
  loadRecommendations()
})
</script>

<style scoped lang="scss">
/* 页面容器 */
.home-container {
  min-height: 100vh;
  background-color: #FFF0F5;
  padding: 30rpx;
  position: relative;
}

/* 页面头部 */
.home-header {
  text-align: center;
  margin-bottom: 40rpx;
  padding: 40rpx 30rpx;
  background-color: white;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  position: relative;
  overflow: hidden;
}

.home-header::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 8rpx;
  background: linear-gradient(90deg, #F82E8A, #FF79B0);
}

.home-logo {
  width: 120rpx;
  height: 120rpx;
  margin-bottom: 20rpx;
  border-radius: 50%;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.2);
}

.home-title {
  font-size: 40rpx;
  margin-bottom: 15rpx;
  color: #F82E8A;
  font-weight: bold;
  letter-spacing: 2rpx;
}

.home-subtitle {
  font-size: 28rpx;
  color: #666;
  line-height: 1.5;
}

.header-actions {
  display: flex;
  justify-content: center;
  gap: 20rpx;
  margin-top: 30rpx;
}

/* 功能区块 */
.interaction-section,
.recommend-section {
  margin-bottom: 40rpx;
}

.section-title {
  font-size: 36rpx;
  font-weight: 700;
  color: #F82E8A;
  margin-bottom: 20rpx;
  text-align: center;
  letter-spacing: 1rpx;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20rpx;
}

.section-more {
  font-size: 28rpx;
  color: #F82E8A;
  cursor: pointer;
  padding: 10rpx 20rpx;
  background-color: rgba(248, 46, 138, 0.1);
  border-radius: 15rpx;
  transition: all 0.3s ease;
}

.section-more:active {
  background-color: rgba(248, 46, 138, 0.2);
}

/* 交互卡片网格 */
.interaction-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280rpx, 1fr));
  gap: 20rpx;
}

.interaction-card {
  height: 280rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 30rpx;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 20rpx;
  background-color: white;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  border: 1rpx solid rgba(248, 46, 138, 0.2);
  position: relative;
  overflow: hidden;
}

/* 悬停效果 - H5平台 */
.interaction-card:hover {
  transform: scale(1.03);
  box-shadow: 0 8rpx 24rpx rgba(248, 46, 138, 0.15);
}

/* 点击效果 - 全平台支持 */
.interaction-card:active {
  transform: scale(0.98);
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.05);
}

/* 卡片颜色主题 */
.interaction-card.pink {
  border-color: #F82E8A;
}

.interaction-card.green {
  border-color: #4CAF50;
}

.interaction-card.yellow {
  border-color: #FFC107;
}

.interaction-icon {
  font-size: 80rpx;
  margin-bottom: var(--spacing-md);
  width: 80rpx;
  height: 80rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.interaction-title {
  font-size: var(--font-size-lg);
  font-weight: 700;
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-sm);
  font-family: 'Pixelify Sans', monospace;
}

.interaction-desc {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  line-height: 1.4;
}

/* 推荐卡片 */
.recommend-card {
  border: 3px solid var(--color-text-primary);
  background-color: var(--color-bg-card);
  box-shadow: 4px 4px 0 var(--color-secondary);
  transition: all 0.3s ease;
  cursor: pointer;
}

.recommend-card:hover {
  transform: translate(-2px, -2px);
  box-shadow: 6px 6px 0 var(--color-secondary);
}

.recommend-cover {
  width: 100%;
  height: 160rpx;
  object-fit: cover;
  display: block;
}

.recommend-info {
  padding: var(--spacing-md);
}

.recommend-title {
  font-size: var(--font-size-md);
  font-weight: 700;
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-xs);
  font-family: 'Pixelify Sans', monospace;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.recommend-rating {
  font-size: var(--font-size-sm);
  color: var(--color-warning);
  margin-bottom: var(--spacing-sm);
  font-weight: 700;
}

.recommend-tags {
  display: flex;
  gap: var(--spacing-xs);
  flex-wrap: wrap;
}

/* 响应式设计 */
@media screen and (max-width: 768rpx) {
  .interaction-cards {
    grid-template-columns: 1fr;
  }
  
  .section-header {
    flex-direction: column;
    gap: var(--spacing-sm);
    text-align: center;
  }
}

/* 装饰性元素 */
.anime-decoration {
  position: absolute;
  width: 200rpx;
  height: 200rpx;
  background-color: var(--color-primary);
  opacity: 0.05;
  border-radius: 50%;
  z-index: -1;
}

.top-left {
  top: -50rpx;
  left: -50rpx;
  animation: float 8s ease-in-out infinite;
}

.top-right {
  top: -100rpx;
  right: -50rpx;
  background-color: var(--color-secondary);
  animation: float 10s ease-in-out infinite 1s;
}

.bottom-left {
  bottom: -100rpx;
  left: -50rpx;
  background-color: var(--color-accent);
  animation: float 9s ease-in-out infinite 2s;
}

.bottom-right {
  bottom: -50rpx;
  right: -50rpx;
  background-color: var(--color-warning);
  animation: float 11s ease-in-out infinite 3s;
}

/* 新增动画 */
@keyframes float {
  0% {
    transform: translateY(0px) rotate(0deg);
  }
  50% {
    transform: translateY(-20px) rotate(5deg);
  }
  100% {
    transform: translateY(0px) rotate(0deg);
  }
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(248, 46, 138, 0.7);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(248, 46, 138, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(248, 46, 138, 0);
  }
}

@keyframes glow {
  0% {
    text-shadow: 0 0 5px rgba(248, 46, 138, 0.5);
  }
  50% {
    text-shadow: 0 0 15px rgba(248, 46, 138, 0.8), 0 0 30px rgba(248, 46, 138, 0.5);
  }
  100% {
    text-shadow: 0 0 5px rgba(248, 46, 138, 0.5);
  }
}

@keyframes bounce-in {
  0% {
    transform: scale(0.8);
    opacity: 0;
  }
  60% {
    transform: scale(1.1);
    opacity: 1;
  }
  100% {
    transform: scale(1);
  }
}

@keyframes slide-up {
  from {
    transform: translateY(30rpx);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

/* 应用动画类 */
.bounce-in {
  animation: bounce-in 0.8s cubic-bezier(0.215, 0.610, 0.355, 1.000) both;
}

.slide-up {
  animation: slide-up 0.6s ease-out forwards;
  opacity: 0;
}

/* 自定义按钮样式 */
.custom-btn {
  padding: 16rpx 32rpx;
  border-radius: var(--radius-lg);
  font-size: var(--font-size-md);
  font-weight: 600;
  transition: all 0.3s ease;
  border: none;
  outline: none;
}

.custom-btn.primary {
  background-color: var(--color-primary);
  color: white;
  box-shadow: 0 4rpx 12rpx rgba(248, 46, 138, 0.3);
}

.custom-btn.primary:active {
  background-color: #E02476;
  box-shadow: 0 2rpx 6rpx rgba(248, 46, 138, 0.2);
  transform: translateY(2rpx);
}

.custom-btn.secondary {
  background-color: white;
  color: var(--color-primary);
  border: 2rpx solid var(--color-primary);
  box-shadow: 0 4rpx 12rpx rgba(248, 46, 138, 0.1);
}

.custom-btn.secondary:active {
  background-color: rgba(248, 46, 138, 0.05);
  box-shadow: 0 2rpx 6rpx rgba(248, 46, 138, 0.05);
  transform: translateY(2rpx);
}

/* 头部优化 */
.home-header {
  padding: var(--spacing-lg) var(--spacing-xl);
  border-radius: var(--radius-xl);
  margin: var(--spacing-xl) var(--spacing-lg);
  box-shadow: 0 8rpx 30rpx rgba(248, 46, 138, 0.1);
  text-align: center;
}

/* 底部样式 */
.home-footer {
  margin-top: var(--spacing-xxl);
  padding: var(--spacing-xl) 0;
  border-top: 1rpx solid var(--color-divider);
  background-color: rgba(255, 255, 255, 0.7);
}

.footer-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-md);
}

.footer-logo image {
  width: 120rpx;
  height: 120rpx;
  border-radius: 50%;
}

.footer-links {
  display: flex;
  gap: var(--spacing-lg);
  margin: var(--spacing-sm) 0;
}

.footer-links text {
  color: var(--color-text-secondary);
  font-size: var(--font-size-sm);
  cursor: pointer;
  transition: color 0.3s ease;
}

.footer-links text:hover {
  color: var(--color-primary);
}

.footer-copyright {
  font-size: var(--font-size-xs);
  color: var(--color-text-tertiary);
}

/* 底部区域 -->
    <view class="home-footer slide-up" :style="{ 'animation-delay': '0.5s' }">
      <view class="footer-content">
        <view class="footer-logo">
          <image src="/static/logo.png" />        
        </view>
        <view class="footer-links">
          <text @click="navigateTo('/pages/personal/index')">关于我们</text>
          <text @click="navigateTo('/pages/personal/index')">使用条款</text>
          <text @click="navigateTo('/pages/personal/index')">隐私政策</text>
        </view>
        <view class="footer-copyright">
          © 2023 Galgame 批评工具箱. 保留所有权利.
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  methods: {
    scaleUp(e) {
      e.currentTarget.style.transform = 'scale(1.03)';
      e.currentTarget.style.transition = 'transform 0.3s ease';
    },
    scaleDown(e) {
      e.currentTarget.style.transform = 'scale(1)';
      e.currentTarget.style.transition = 'transform 0.3s ease';
    }
  }
}
</script>

<style scoped>
/* 页面容器 */
.home-container {
  min-height: 100vh;
  background-color: #FFF0F5;
  padding: 30rpx;
  position: relative;
}

/* 页面头部 */
.home-header {
  text-align: center;
  margin-bottom: 40rpx;
  padding: 40rpx 30rpx;
  background-color: white;
  border-radius: 20rpx;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  position: relative;
  overflow: hidden;
}

.home-header::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 8rpx;
  background: linear-gradient(90deg, #F82E8A, #FF79B0);
}

.home-logo {
  width: 120rpx;
  height: 120rpx;
  margin-bottom: 20rpx;
  border-radius: 50%;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.2);
}

.home-title {
  font-size: 40rpx;
  margin-bottom: 15rpx;
  color: #F82E8A;
  font-weight: bold;
  letter-spacing: 2rpx;
}

.home-subtitle {
  font-size: 28rpx;
  color: #666;
  line-height: 1.5;
}

.header-actions {
  display: flex;
  justify-content: center;
  gap: 20rpx;
  margin-top: 30rpx;
}

/* 功能区块 */
.interaction-section,
.recommend-section {
  margin-bottom: 40rpx;
}

.section-title {
  font-size: 36rpx;
  font-weight: 700;
  color: #F82E8A;
  margin-bottom: 20rpx;
  text-align: center;
  letter-spacing: 1rpx;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20rpx;
}

.section-more {
  font-size: 28rpx;
  color: #F82E8A;
  cursor: pointer;
  padding: 10rpx 20rpx;
  background-color: rgba(248, 46, 138, 0.1);
  border-radius: 15rpx;
  transition: all 0.3s ease;
}

.section-more:active {
  background-color: rgba(248, 46, 138, 0.2);
}

/* 交互卡片网格 */
.interaction-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280rpx, 1fr));
  gap: 20rpx;
}

.interaction-card {
  height: 280rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 30rpx;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 20rpx;
  background-color: white;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
  border: 1rpx solid rgba(248, 46, 138, 0.2);
}

.interaction-card:active {
  transform: translateY(2rpx);
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.05);
}

.interaction-icon {
  font-size: 80rpx;
  margin-bottom: var(--spacing-md);
  width: 80rpx;
  height: 80rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.interaction-title {
  font-size: var(--font-size-lg);
  font-weight: 700;
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-sm);
  font-family: 'Pixelify Sans', monospace;
}

.interaction-desc {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  line-height: 1.4;
}

/* 推荐卡片 */
.recommend-card {
  border: 3px solid var(--color-text-primary);
  background-color: var(--color-bg-card);
  box-shadow: 4px 4px 0 var(--color-secondary);
  transition: all 0.3s ease;
  cursor: pointer;
}

.recommend-card:hover {
  transform: translate(-2px, -2px);
  box-shadow: 6px 6px 0 var(--color-secondary);
}

.recommend-cover {
  width: 100%;
  height: 160rpx;
  object-fit: cover;
  display: block;
}

.recommend-info {
  padding: var(--spacing-md);
}

.recommend-title {
  font-size: var(--font-size-md);
  font-weight: 700;
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-xs);
  font-family: 'Pixelify Sans', monospace;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.recommend-rating {
  font-size: var(--font-size-sm);
  color: var(--color-warning);
  margin-bottom: var(--spacing-sm);
  font-weight: 700;
}

.recommend-tags {
  display: flex;
  gap: var(--spacing-xs);
  flex-wrap: wrap;
}

/* 响应式设计 */
@media screen and (max-width: 768rpx) {
  .interaction-cards {
    grid-template-columns: 1fr;
  }
  
  .section-header {
    flex-direction: column;
    gap: var(--spacing-sm);
    text-align: center;
  }
}
</style>