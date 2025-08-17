<template>
  <view class="dialogue-container">
    <!-- 页面头部 -->
    <view class="dialogue-header">
      <view class="pixel-title">智能对话助手</view>
      <view class="subtitle">与AI助手深度交流，发现你的游戏偏好</view>
    </view>

    <!-- 对话区域 -->
    <view class="dialogue-area">
      <scroll-view 
        class="message-list" 
        scroll-y 
        :scroll-top="scrollTop"
        scroll-with-animation
      >
        <view 
          class="message-item" 
          v-for="(msg, index) in messages" 
          :key="index"
          :class="msg.role"
        >
          <view class="message-avatar">
            <text v-if="msg.role === 'assistant'">🤖</text>
            <text v-else>👤</text>
          </view>
          <view class="message-bubble pixel-card" :class="msg.role">
            <view class="message-content">{{ msg.content }}</view>
            <view class="message-time">{{ formatTime(msg.timestamp) }}</view>
          </view>
        </view>
        
        <!-- 加载状态 -->
        <view v-if="isLoading" class="message-item assistant">
          <view class="message-avatar">🤖</view>
          <view class="message-bubble pixel-card assistant">
            <view class="typing-indicator">
              <view class="typing-dot"></view>
              <view class="typing-dot"></view>
              <view class="typing-dot"></view>
            </view>
          </view>
        </view>
      </scroll-view>
    </view>

    <!-- 输入区域 -->
    <view class="input-area">
      <view class="input-container">
        <input 
          class="message-input"
          v-model="inputMessage"
          placeholder="说出你的想法..."
          :disabled="isLoading"
          @confirm="sendMessage"
          confirm-type="send"
        />
        <button 
          class="send-btn pixel-btn" 
          :disabled="!inputMessage.trim() || isLoading"
          @click="sendMessage"
        >
          发送
        </button>
      </view>
      
      <!-- 快捷回复 -->
      <view class="quick-replies" v-if="quickReplies.length > 0">
        <view 
          class="quick-reply pixel-tag"
          v-for="(reply, index) in quickReplies"
          :key="index"
          @click="selectQuickReply(reply)"
        >
          {{ reply }}
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'

// 响应式数据
const messages = ref([])
const inputMessage = ref('')
const isLoading = ref(false)
const scrollTop = ref(0)
const quickReplies = ref([
  '推荐一些治愈系游戏',
  '我喜欢剧情丰富的作品',
  '有什么经典必玩的吗？',
  '最近有哪些新作值得关注？'
])

// 初始欢迎消息
const welcomeMessages = [
  {
    role: 'assistant',
    content: '你好！我是你的Galgame推荐助手 🎮 我可以根据你的喜好为你推荐合适的游戏。',
    timestamp: Date.now()
  },
  {
    role: 'assistant', 
    content: '你可以告诉我你喜欢什么类型的故事、角色或者游戏风格，我会为你精心挑选！',
    timestamp: Date.now() + 1000
  }
]

// 模拟AI回复数据
const aiResponses = [
  '根据你的描述，我推荐你试试《CLANNAD》，这是一部非常治愈的校园恋爱作品。',
  '如果你喜欢剧情丰富的游戏，《Steins;Gate》绝对是不二之选，科幻悬疑剧情非常精彩。',
  '《Little Busters!》也很不错，友情与成长的主题很温暖。',
  '最近的话，《Summer Pockets》和《Angel Beats! 1st beat》都值得一试。',
  '你还有什么特别的偏好吗？比如喜欢的画风、声优或者特定的剧情元素？'
]

// 发送消息
async function sendMessage() {
  if (!inputMessage.value.trim() || isLoading.value) return
  
  // 添加用户消息
  const userMessage = {
    role: 'user',
    content: inputMessage.value.trim(),
    timestamp: Date.now()
  }
  messages.value.push(userMessage)
  
  const userInput = inputMessage.value
  inputMessage.value = ''
  
  // 滚动到底部
  await scrollToBottom()
  
  // 模拟AI回复
  await simulateAIResponse(userInput)
}

// 快捷回复
function selectQuickReply(reply) {
  inputMessage.value = reply
  sendMessage()
}

// 模拟AI响应
async function simulateAIResponse(userInput) {
  isLoading.value = true
  
  // 模拟网络延迟
  await new Promise(resolve => setTimeout(resolve, 1000 + Math.random() * 2000))
  
  // 选择回复内容
  let response = aiResponses[Math.floor(Math.random() * aiResponses.length)]
  
  // 根据用户输入调整回复
  if (userInput.includes('治愈') || userInput.includes('温暖')) {
    response = '治愈系的话，我强烈推荐《CLANNAD》和《Little Busters!》，都是非常温暖的作品。'
  } else if (userInput.includes('剧情') || userInput.includes('故事')) {
    response = '剧情向的游戏推荐《Steins;Gate》和《Fate/stay night》，剧情深度很棒。'
  } else if (userInput.includes('新作') || userInput.includes('最近')) {
    response = '最近比较火的新作有《Summer Pockets》，Key社的最新作品质量很高。'
  }
  
  const aiMessage = {
    role: 'assistant',
    content: response,
    timestamp: Date.now()
  }
  
  messages.value.push(aiMessage)
  isLoading.value = false
  
  await scrollToBottom()
}

// 滚动到底部
async function scrollToBottom() {
  await nextTick()
  scrollTop.value = 999999
}

// 格式化时间
function formatTime(timestamp) {
  const date = new Date(timestamp)
  return `${date.getHours().toString().padStart(2, '0')}:${date.getMinutes().toString().padStart(2, '0')}`
}

// 页面加载时显示欢迎消息
onMounted(async () => {
  for (const msg of welcomeMessages) {
    messages.value.push(msg)
    await new Promise(resolve => setTimeout(resolve, 500))
    await scrollToBottom()
  }
})
</script>

<style scoped>
.dialogue-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: var(--color-bg-page);
}

.dialogue-header {
  padding: var(--spacing-lg);
  text-align: center;
  background-color: var(--color-bg-card);
  border-bottom: 3px solid var(--color-text-primary);
}

.subtitle {
  font-size: var(--font-size-sm);
  color: var(--color-text-secondary);
  margin-top: var(--spacing-xs);
}

.dialogue-area {
  flex: 1;
  overflow: hidden;
}

.message-list {
  height: 100%;
  padding: var(--spacing-md);
}

.message-item {
  display: flex;
  margin-bottom: var(--spacing-md);
  align-items: flex-start;
}

.message-item.user {
  justify-content: flex-end;
}

.message-avatar {
  width: 60rpx;
  height: 60rpx;
  border-radius: 50%;
  background-color: var(--color-bg-card);
  border: 2px solid var(--color-text-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24rpx;
  margin: 0 var(--spacing-sm);
  box-shadow: 2px 2px 0 var(--color-secondary);
}

.message-bubble {
  max-width: 70%;
  padding: var(--spacing-md);
  position: relative;
}

.message-bubble.assistant {
  background-color: var(--color-bg-card);
  border-color: var(--color-primary);
}

.message-bubble.user {
  background-color: var(--color-primary);
  color: var(--color-text-inverse);
  border-color: var(--color-primary);
}

.message-content {
  font-size: var(--font-size-md);
  line-height: 1.5;
  margin-bottom: var(--spacing-xs);
}

.message-time {
  font-size: var(--font-size-xs);
  opacity: 0.7;
  text-align: right;
}

.typing-indicator {
  display: flex;
  gap: var(--spacing-xs);
  align-items: center;
}

.typing-dot {
  width: 8rpx;
  height: 8rpx;
  border-radius: 50%;
  background-color: var(--color-text-secondary);
  animation: typing 1.4s infinite ease-in-out;
}

.typing-dot:nth-child(1) { animation-delay: -0.32s; }
.typing-dot:nth-child(2) { animation-delay: -0.16s; }
.typing-dot:nth-child(3) { animation-delay: 0s; }

@keyframes typing {
  0%, 80%, 100% {
    transform: scale(0);
    opacity: 0.5;
  }
  40% {
    transform: scale(1);
    opacity: 1;
  }
}

.input-area {
  background-color: var(--color-bg-card);
  border-top: 3px solid var(--color-text-primary);
  padding: var(--spacing-md);
}

.input-container {
  display: flex;
  gap: var(--spacing-sm);
  margin-bottom: var(--spacing-sm);
}

.message-input {
  flex: 1;
  padding: var(--spacing-md);
  border: 2px solid var(--color-text-primary);
  background-color: var(--color-bg-page);
  font-size: var(--font-size-md);
  color: var(--color-text-primary);
}

.send-btn {
  padding: var(--spacing-md) var(--spacing-lg);
}

.send-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.quick-replies {
  display: flex;
  flex-wrap: wrap;
  gap: var(--spacing-xs);
}

.quick-reply {
  cursor: pointer;
  transition: all 0.2s ease;
}

.quick-reply:active {
  transform: scale(0.95);
}
</style>