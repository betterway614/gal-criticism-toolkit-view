<template>
  <view class="bingo-container">
    <!-- 头部信息 -->
    <view class="bingo-header">
      <view class="pixel-title">GAL 宾果挑战</view>
      <view class="subtitle">选择符合你偏好的特征，凑成一条线即可获胜</view>
    </view>

    <!-- 分数与控制 -->
    <view class="bingo-controls">
      <view class="score pixel-card">已选：{{ selectedCount }} / 12</view>
      <button class="pixel-btn" @click="reset">重置</button>
    </view>

    <!-- 宾果网格 5x5 -->
    <view class="bingo-grid pixel-card">
      <view 
        class="bingo-cell"
        v-for="(cell, idx) in grid"
        :key="idx"
        :class="{ active: cell.checked, win: winLinesMap[idx] }"
        @click="toggle(idx)"
      >
        <text class="cell-text">{{ cell.label }}</text>
      </view>
    </view>

    <!-- 结束弹层 -->
    <view v-if="hasWin" class="win-overlay">
      <view class="win-card pixel-card">
        <view class="result-title">宾果！🎉</view>
        <view class="result-desc">你完成了 {{ winLineCount }} 条连线</view>
        <view class="result-actions">
          <button class="pixel-btn" @click="goRecommend">查看推荐</button>
          <button class="pixel-btn secondary" @click="reset">再来一局</button>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref, computed } from 'vue'

// 25 个候选特征（示例）
const candidates = [
  '校园', '治愈', '科幻', '悬疑', '奇幻',
  '恋爱', '日常', '剧情', '轻松', '热血',
  '反转', '长篇', '短篇', '原创新作', '经典',
  '强演出', '高配乐', '全语音', '多结局', 'CG丰富',
  '角色群像', '黑暗系', '成长', '冒险', '搞笑'
]

const grid = ref(candidates.map(l => ({ label: l, checked: false })))

const selectedCount = computed(() => grid.value.filter(c => c.checked).length)

function reset() {
  grid.value = candidates
    .map(l => ({ label: l, checked: false }))
    .sort(() => Math.random() - 0.5)
}

function toggle(idx) {
  grid.value[idx].checked = !grid.value[idx].checked
}

// 计算所有可能的连线（5 行、5 列、2 斜线）
const lines = [
  // 行
  [0,1,2,3,4], [5,6,7,8,9], [10,11,12,13,14], [15,16,17,18,19], [20,21,22,23,24],
  // 列
  [0,5,10,15,20], [1,6,11,16,21], [2,7,12,17,22], [3,8,13,18,23], [4,9,14,19,24],
  // 斜线
  [0,6,12,18,24], [4,8,12,16,20]
]

const winLines = computed(() => {
  return lines.filter(line => line.every(i => grid.value[i].checked))
})

const winLineCount = computed(() => winLines.value.length)
const hasWin = computed(() => winLineCount.value > 0)

const winLinesMap = computed(() => {
  const map = {}
  winLines.value.forEach(line => line.forEach(i => map[i] = true))
  return map
})

function goRecommend() {
  uni.navigateTo({ url: '/pages/recommend/index' })
}
</script>

<style scoped>
.bingo-container {
  min-height: 100vh;
  background: var(--color-bg-page);
}

.bingo-header {
  padding: var(--spacing-lg);
  text-align: center;
}
.subtitle {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
}

.bingo-controls {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--spacing-lg);
  margin-bottom: var(--spacing-md);
}

.score {
  padding: var(--spacing-sm) var(--spacing-md);
}

.bingo-grid {
  margin: 0 var(--spacing-lg);
  padding: var(--spacing-md);
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: var(--spacing-sm);
}

.bingo-cell {
  height: 120rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: var(--spacing-sm);
  border: 2px solid var(--color-text-primary);
  background: var(--color-bg-card);
  box-shadow: 2px 2px 0 var(--color-secondary);
  user-select: none;
  transition: transform 0.12s ease;
}

.bingo-cell:active { transform: scale(0.98); }
.bingo-cell.active {
  background: var(--color-primary);
  color: white;
}
.bingo-cell.win {
  outline: 3px solid var(--color-warning);
}
.cell-text { font-size: var(--font-size-sm); }

.win-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.4);
  display: flex;
  align-items: center;
  justify-content: center;
}
.win-card { width: 80%; max-width: 640rpx; padding: var(--spacing-lg); }
.result-title { font-size: var(--font-size-xl); font-weight: 700; margin-bottom: var(--spacing-sm); }
.result-desc { color: var(--color-text-secondary); margin-bottom: var(--spacing-md); }
.result-actions { display: flex; gap: var(--spacing-sm); justify-content: center; }
</style>