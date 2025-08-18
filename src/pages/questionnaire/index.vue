<template>
  <view class="questionnaire-container">
    <!-- 头部 -->
    <view class="qn-header">
      <view class="anime-title">偏好问卷</view>
      <view class="subtitle">通过简短问卷，快速了解你的喜好并生成个性化推荐</view>
    </view>

    <!-- 进度条 -->
    <view class="progress">
      <view class="progress-bg"></view>
      <view class="progress-bar" :style="{ width: progressPercent + '%' }"></view>
    </view>

    <!-- 问题区域 -->
    <view class="qn-area">
      <view class="qn-card anime-card">
        <view class="qn-index">问题 {{ currentIndex + 1 }} / {{ questions.length }}</view>
        <view class="qn-title">{{ currentQuestion.title }}</view>

        <view class="qn-options">
          <view 
            v-for="(opt, idx) in currentQuestion.options"
            :key="idx"
            class="qn-option anime-card"
            :class="{ active: answers[currentIndex] === opt.value }"
            @click="selectOption(opt.value)"
          >
            <view class="opt-label">{{ opt.label }}</view>
            <view class="opt-desc" v-if="opt.desc">{{ opt.desc }}</view>
          </view>
        </view>
      </view>

      <!-- 操作按钮 -->
      <view class="qn-actions">
        <button class="anime-btn secondary" :disabled="currentIndex === 0" @click="prev">上一步</button>
        <button class="anime-btn" :disabled="!answers[currentIndex]" @click="next">
          {{ currentIndex === questions.length - 1 ? '完成' : '下一题' }}
        </button>
      </view>
    </view>

    <!-- 完成弹层 -->
    <view v-if="isFinished" class="qn-result-overlay">
      <view class="qn-result anime-card">
        <view class="result-title">完成！🎉</view>
        <view class="result-desc">已根据你的回答生成个性化推荐。</view>
        <view class="result-actions">
          <button class="anime-btn" @click="goRecommend">查看推荐</button>
          <button class="anime-btn secondary" @click="restart">重新填写</button>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
    title: '你更偏好的题材是？',
    options: [
      { label: '校园治愈', value: 'school', desc: '温暖、日常、成长' },
      { label: '科幻悬疑', value: 'sci', desc: '设定、推理、反转' },
      { label: '奇幻冒险', value: 'fantasy' },
      { label: '都市恋爱', value: 'love' },
    ]
  },
  {
    title: '你喜欢的叙事节奏？',
    options: [
      { label: '慢热铺垫', value: 'slow' },
      { label: '高潮迭起', value: 'fast' },
      { label: '日常向', value: 'slice' },
    ]
  },
  {
    title: '你更看重的要素？',
    options: [
      { label: '剧情深度', value: 'story' },
      { label: '角色塑造', value: 'role' },
      { label: '演出/配乐', value: 'av' },
    ]
  }
])

const currentIndex = ref(0)
const answers = ref({})
const isFinished = ref(false)

const currentQuestion = computed(() => questions.value[currentIndex.value])
const progressPercent = computed(() => Math.round(((currentIndex.value) / questions.value.length) * 100))

function selectOption(val) {
  answers.value[currentIndex.value] = val
}

function prev() {
  if (currentIndex.value > 0) currentIndex.value -= 1
}

function next() {
  if (!answers.value[currentIndex.value]) return
  if (currentIndex.value < questions.value.length - 1) {
    currentIndex.value += 1
  } else {
    isFinished.value = true
  }
}

function restart() {
  answers.value = {}
  currentIndex.value = 0
  isFinished.value = false
}

function goRecommend() {
  uni.navigateTo({ url: '/pages/recommend/index' })
}
</script>

<style scoped>
.questionnaire-container {
  min-height: 100vh;
  background-color: var(--color-bg-page);
  padding-bottom: 120rpx;
}

.qn-header {
  padding: var(--spacing-lg);
  text-align: center;
}

.subtitle {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  margin-top: var(--spacing-sm);
}

.progress {
  height: 12rpx;
  margin: 0 var(--spacing-lg);
  position: relative;
  border-radius: var(--radius-sm);
  overflow: hidden;
}

.progress-bg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(248, 46, 138, 0.1);
  border: 1rpx solid rgba(248, 46, 138, 0.2);
}

.progress-bar {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  background: linear-gradient(90deg, var(--color-primary), var(--color-secondary));
  border-radius: var(--radius-sm);
  transition: width 0.3s ease;
}

.qn-area {
  padding: var(--spacing-lg);
}

.qn-card {
  padding: var(--spacing-lg);
}

.qn-index {
  font-size: var(--font-size-xs);
  color: var(--color-text-secondary);
  margin-bottom: var(--spacing-sm);
}

.qn-title {
  font-size: var(--font-size-lg);
  font-weight: 700;
  margin-bottom: var(--spacing-md);
  color: var(--color-text-primary);
  letter-spacing: 1rpx;
}

.qn-options {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--spacing-sm);
}

.qn-option {
  padding: var(--spacing-md);
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1rpx solid var(--color-border);
}

.qn-option:active {
  transform: translateY(2rpx);
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.05);
}

.qn-option.active {
  background: white;
  border-color: var(--color-primary);
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.2);
}

.opt-label { 
  font-weight: 700; 
  color: var(--color-text-primary);
}

.opt-desc { 
  font-size: var(--font-size-sm); 
  color: var(--color-text-secondary);
  margin-top: var(--spacing-xs);
}

.qn-actions {
  display: flex;
  justify-content: space-between;
  gap: var(--spacing-sm);
  margin-top: var(--spacing-lg);
}

.qn-result-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.qn-result {
  width: 80%;
  max-width: 640rpx;
  padding: var(--spacing-lg);
  animation: slideUp 0.3s ease;
}

@keyframes slideUp {
  from {
    transform: translateY(20rpx);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.result-title { 
  font-size: var(--font-size-xl); 
  font-weight: 700; 
  margin-bottom: var(--spacing-sm); 
  color: var(--color-text-primary);
  text-align: center;
}

.result-desc { 
  color: var(--color-text-secondary); 
  margin-bottom: var(--spacing-md); 
  text-align: center;
}

.result-actions { 
  display: flex; 
  gap: var(--spacing-sm); 
  justify-content: center;
}
</style>