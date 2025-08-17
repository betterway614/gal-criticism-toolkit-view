<template>
  <view class="home-container fade-in">
    <!-- 页面头部 -->
    <view class="home-header">
      <image src="/static/logo.png" class="home-logo" />
      <view class="home-title pixel-title">Galgame 批评工具箱</view>
      <view class="home-subtitle">探索、分析、发现你的 Galgame 宇宙</view>
      <!-- 新增主行动按钮 -->
      <view class="header-actions">
        <button class="pixel-btn" @click="navigateTo('/pages/dialogue/index')">开始对话</button>
        <button class="pixel-btn secondary" @click="navigateTo('/pages/recommend/index')">浏览推荐</button>
      </view>
    </view>

    <!-- 三大功能入口 -->
    <view class="interaction-section">
      <view class="section-title">开始你的探索之旅</view>
      <view class="interaction-cards">
        <view 
          class="interaction-card pixel-card pink slide-in" 
          :style="{ 'animation-delay': '0.1s' }"
          @click="navigateTo('/pages/dialogue/index')"
        >
          <view class="interaction-icon">💬</view>
          <view class="interaction-title">智能对话</view>
          <view class="interaction-desc">与AI助手深度交流，发现你的游戏偏好</view>
        </view>
        
        <view 
          class="interaction-card pixel-card green slide-in"
          :style="{ 'animation-delay': '0.2s' }"
          @click="navigateTo('/pages/questionnaire/index')"
        >
          <view class="interaction-icon">📝</view>
          <view class="interaction-title">偏好问卷</view>
          <view class="interaction-desc">通过科学问卷快速构建个人画像</view>
        </view>
        
        <view 
          class="interaction-card pixel-card yellow slide-in"
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
      <view v-if="recommendState.loading" class="pixel-scroll-x">
        <view class="pixel-scroll-item" v-for="i in 3" :key="i">
          <view class="pixel-skeleton card"></view>
        </view>
      </view>
      
      <!-- 错误状态 -->
      <view v-else-if="recommendState.error" class="pixel-error">
        <view class="pixel-error-icon">⚠️</view>
        <view class="pixel-error-text">{{ recommendState.error }}</view>
        <button class="pixel-btn sm" @click="loadRecommendations">重试</button>
      </view>
      
      <!-- 空状态 -->
      <view v-else-if="!recommendState.data.length" class="pixel-empty">
        <view class="pixel-empty-icon">📚</view>
        <view class="pixel-empty-text">暂无推荐内容</view>
        <button class="pixel-btn sm" @click="navigateTo('/pages/questionnaire/index')">
          去填写问卷
        </button>
      </view>
      
      <!-- 推荐内容 -->
      <view v-else class="pixel-scroll-x">
        <view 
          class="pixel-scroll-item recommend-card"
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
              <view class="pixel-tag" v-for="tag in item.tags.slice(0, 2)" :key="tag">
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
  uni.navigateTo({ url })
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

<style scoped>
/* 页面容器 */
.home-container {
  min-height: 100vh;
  background-color: var(--color-bg-page);
  padding: var(--spacing-xl) var(--spacing-lg);
}

/* 页面头部 */
.home-header {
  text-align: center;
  margin-bottom: var(--spacing-xxl);
  padding: var(--spacing-xl) var(--spacing-lg);
  background-color: var(--color-bg-card);
  border: 3px solid var(--color-text-primary);
  box-shadow: 6px 6px 0 var(--color-secondary);
  background-image:
    linear-gradient(90deg, rgba(0,0,0,0.03) 1px, transparent 1px),
    linear-gradient(0deg, rgba(0,0,0,0.03) 1px, transparent 1px);
  background-size: 16rpx 16rpx;
}

.home-logo {
  width: 120rpx;
  height: 120rpx;
  margin-bottom: var(--spacing-md);
  border: 3px solid var(--color-text-primary);
  box-shadow: 4px 4px 0 var(--color-primary);
}

.home-title {
  font-size: var(--font-size-xxl);
  margin-bottom: var(--spacing-sm);
}

.home-subtitle {
  font-size: var(--font-size-md);
  color: var(--color-text-secondary);
  line-height: 1.5;
}

/* 功能区块 */
.interaction-section,
.recommend-section {
  margin-bottom: var(--spacing-xxl);
}

.section-title {
  font-size: var(--font-size-xl);
  font-weight: 700;
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-lg);
  text-align: center;
  font-family: 'Pixelify Sans', monospace;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--spacing-lg);
}

.section-more {
  font-size: var(--font-size-sm);
  color: var(--color-primary);
  cursor: pointer;
  padding: var(--spacing-xs) var(--spacing-sm);
  border: 2px solid var(--color-primary);
  transition: all 0.2s ease;
}

.section-more:hover {
  background-color: var(--color-primary);
  color: var(--color-text-inverse);
}

/* 交互卡片网格 */
.interaction-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280rpx, 1fr));
  gap: var(--spacing-lg);
}

.interaction-card {
  height: 280rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: var(--spacing-lg);
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
}

.interaction-card:hover {
  transform: translate(-2px, -2px);
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