<template>
  <view class="dialogue-container">
    <!-- 页面头部 -->
    <view class="dialogue-header">
      <view class="anime-title">智能对话助手</view>
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
          <view class="message-bubble anime-card" :class="msg.role">
            <view class="message-content">{{ msg.content }}</view>
            <view class="message-time">{{ formatTime(msg.timestamp) }}</view>
          </view>
        </view>
        
        <!-- 加载状态 -->
        <view v-if="isLoading" class="message-item assistant">
          <view class="message-avatar">🤖</view>
          <view class="message-bubble anime-card assistant">
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
        <uni-input 
          class="message-input"
          v-model="inputMessage"
          placeholder="说出你的想法..."
          :disabled="isLoading"
          @confirm="sendMessage"
          confirm-type="send"
        />
        <button 
          class="send-btn anime-btn" 
          :disabled="!inputMessage.trim() || isLoading"
          @click="sendMessage"
          style="color: white;"
        >
          发送
        </button>
      </view>
      
      <!-- 快捷回复 -->
      <view class="quick-replies" v-if="quickReplies.length > 0">
        <view 
          class="quick-reply anime-tag"
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

<style scoped lang="scss">
.dialogue-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #FFF0F5;
}

.dialogue-header {
  padding: 30rpx;
  text-align: center;
  background-color: #F82E8A;
  position: relative;
  box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
}

.dialogue-header::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 8rpx;
  background: linear-gradient(90deg, #F82E8A, #FF79B0);
}

.subtitle {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.9);
  margin-top: 10rpx;
}

.dialogue-area {
  flex: 1;
  overflow: hidden;
}

.message-list {
  height: 100%;
  padding: 20rpx;
}

.message-item {
  display: flex;
  margin-bottom: 24rpx;
  align-items: flex-start;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10rpx);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.message-item.user {
  justify-content: flex-end;
}

.message-avatar {
  width: 60rpx;
  height: 60rpx;
  border-radius: 50%;
  background-color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32rpx;
  margin: 0 16rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
  z-index: 1;
}

.message-bubble {
  max-width: 70%;
  padding: 20rpx 24rpx;
  position: relative;
  word-wrap: break-word;
  word-break: break-all;
}

.message-bubble.assistant {
  background-color: white;
  color: #333;
  border-radius: 0 20rpx 20rpx 20rpx;
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.1);
  border: 1rpx solid rgba(248, 46, 138, 0.2);
}

.message-bubble.user {
  background-color: #F82E8A;
  color: white;
  border-radius: 20rpx 0 20rpx 20rpx;
  box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.2);
}

.message-bubble.assistant::before {
  content: '';
  position: absolute;
  left: -14rpx;
  top: 16rpx;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 10rpx 16rpx 10rpx 0;
  border-color: transparent rgba(248, 46, 138, 0.2) transparent transparent;
}

.message-bubble.user::before {
  content: '';
  position: absolute;
  right: -14rpx;
  top: 16rpx;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 10rpx 0 10rpx 16rpx;
  border-color: transparent transparent transparent #F82E8A;
}

.message-content {
  font-size: 28rpx;
  line-height: 1.5;
  margin-bottom: 8rpx;
}

.message-time {
  font-size: 20rpx;
  opacity: 0.7;
  text-align: right;
}

.typing-indicator {
  display: flex;
  gap: 12rpx;
  align-items: center;
  padding: 10rpx 0;
}

.typing-dot {
  width: 12rpx;
  height: 12rpx;
  border-radius: 50%;
  background-color: #F82E8A;
  animation: typing 1.4s infinite ease-in-out;
}

.typing-dot:nth-child(1) { animation-delay: -0.32s; }
  .typing-dot:nth-child(2) { animation-delay: -0.16s; }
  .typing-dot:nth-child(3) { animation-delay: 0s; }
  
  @keyframes typing {
    0%, 60%, 100% { 
      transform: translateY(0);
      opacity: 0.7;
    }
    30% { 
      transform: translateY(-5px);
      opacity: 1;
    }
  }
  
  .input-area {
    padding: 20rpx;
    border-top: 1px solid rgba(248, 46, 138, 0.2);
    background-color: white;
    box-shadow: 0 -2rpx 8rpx rgba(248, 46, 138, 0.05);
    position: relative;
    z-index: 10;
  }
  
  .input-container {
    display: flex;
    align-items: center;
    gap: 20rpx;
  }
  
  .message-input {
    flex: 1;
    padding: 20rpx 24rpx;
    border: 1px solid rgba(248, 46, 138, 0.3);
    background-color: white;
    color: #333;
    font-size: 28rpx;
    border-radius: 30rpx;
    box-shadow: inset 0 2rpx 4rpx rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
  }
  
  .message-input:focus {
    border-color: #F82E8A;
    box-shadow: inset 0 2rpx 4rpx rgba(0, 0, 0, 0.05), 0 0 0 4rpx rgba(248, 46, 138, 0.1);
  }
  
  .message-input::placeholder {
    color: rgba(0, 0, 0, 0.4);
  }
  
  .quick-replies {
    display: flex;
    flex-wrap: wrap;
    gap: 12rpx;
    margin-top: 20rpx;
  }
  
  .quick-reply {
    padding: 12rpx 20rpx;
    border: 1px solid rgba(248, 46, 138, 0.3);
    background-color: white;
    color: #F82E8A;
    font-size: 24rpx;
    border-radius: 20rpx;
    transition: all 0.3s ease;
    box-shadow: 0 2rpx 6rpx rgba(248, 46, 138, 0.05);
  }
  
  .quick-reply:active {
    background-color: #F82E8A;
    color: white;
    box-shadow: 0 2rpx 10rpx rgba(248, 46, 138, 0.2);
    transform: translateY(2rpx);
  }
  
  /* anime-btn 样式 - 来自全局样式的副本 */
  .anime-btn {
    background-color: #F82E8A;
    color: white;
    border: none;
    padding: 18rpx 36rpx;
    font-size: 28rpx;
    border-radius: 30rpx;
    box-shadow: 0 4rpx 12rpx rgba(248, 46, 138, 0.3);
    transition: all 0.3s ease;
    font-weight: 500;
    letter-spacing: 1rpx;
  }
  
  .anime-btn:active {
    transform: scale(0.95);
    box-shadow: 0 2rpx 6rpx rgba(248, 46, 138, 0.2);
  }
  
  .anime-btn:disabled {
    background-color: #FFD6E5;
    color: #FFA4C5;
    box-shadow: none;
  }
  
  /* anime-title 样式 - 来自全局样式的副本 */
  .anime-title {
    font-size: 44rpx;
    font-weight: bold;
    color: white;
    text-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.1);
    letter-spacing: 2rpx;
    font-family: 'Arial Rounded MT Bold', sans-serif;
  }
  
  /* anime-card 样式 */
  .anime-card {
    background-color: white;
    border-radius: 24rpx;
    box-shadow: 0 4rpx 16rpx rgba(248, 46, 138, 0.1);
    border: 1rpx solid rgba(248, 46, 138, 0.2);
    transition: all 0.3s ease;
  }
  
  /* anime-tag 样式 */
  .anime-tag {
    background-color: rgba(248, 46, 138, 0.05);
    border: 1rpx solid rgba(248, 46, 138, 0.3);
    color: #F82E8A;
    border-radius: 20rpx;
    padding: 12rpx 20rpx;
    font-size: 24rpx;
    transition: all 0.3s ease;
  }
  
  .anime-tag:active {
    background-color: #F82E8A;
    color: white;
    box-shadow: 0 2rpx 8rpx rgba(248, 46, 138, 0.2);
  }
</style>