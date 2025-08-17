<template>
  <view class="questionnaire-container">
    <!-- 头部 -->
    <view class="qn-header">
      <view class="pixel-title">偏好问卷</view>
      <view class="subtitle">通过简短问卷，快速了解你的喜好并生成推荐</view>
    </view>

    <!-- 进度条 -->
    <view class="progress">
      <view class="progress-bar" :style="{ width: progressPercent + '%' }"></view>
    </view>

    <!-- 问题区域 -->
    <view class="qn-area">
      <view class="qn-card pixel-card">
        <view class="qn-index">问题 {{ currentIndex + 1 }} / {{ questions.length }}</view>
        <view class="qn-title">{{ currentQuestion.title }}</view>

        <view class="qn-options">
          <view 
            v-for="(opt, idx) in currentQuestion.options"
            :key="idx"
            class="qn-option pixel-card"
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
        <button class="pixel-btn secondary" :disabled="currentIndex === 0" @click="prev">上一步</button>
        <button class="pixel-btn" :disabled="!answers[currentIndex]" @click="next">
          {{ currentIndex === questions.length - 1 ? '完成' : '下一题' }}
        </button>
      </view>
    </view>

    <!-- 完成弹层 -->
    <view v-if="isFinished" class="qn-result-overlay">
      <view class="qn-result pixel-card">
        <view class="result-title">完成！🎉</view>
        <view class="result-desc">已根据你的回答生成个性化推荐。</view>
        <view class="result-actions">
          <button class="pixel-btn" @click="goRecommend">查看推荐</button>
          <button class="pixel-btn secondary" @click="restart">重新填写</button>
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
}

.progress {
  height: 12rpx;
  background: rgba(0,0,0,0.06);
  margin: 0 var(--spacing-lg);
  border: 2px solid var(--color-text-primary);
}

.progress-bar {
  height: 100%;
  background: var(--color-primary);
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
}

.qn-options {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--spacing-sm);
}

.qn-option {
  padding: var(--spacing-md);
  cursor: pointer;
  transition: transform 0.15s ease;
}

.qn-option:active {
  transform: scale(0.98);
}

.qn-option.active {
  background: var(--color-bg-card);
  border-color: var(--color-primary);
  box-shadow: 4px 4px 0 var(--color-secondary);
}

.opt-label { font-weight: 700; }
.opt-desc { font-size: var(--font-size-sm); color: var(--color-text-secondary); }

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
}

.qn-result {
  width: 80%;
  max-width: 640rpx;
  padding: var(--spacing-lg);
}

.result-title { font-size: var(--font-size-xl); font-weight: 700; margin-bottom: var(--spacing-sm); }
.result-desc { color: var(--color-text-secondary); margin-bottom: var(--spacing-md); }
.result-actions { display: flex; gap: var(--spacing-sm); justify-content: center; }
</style>