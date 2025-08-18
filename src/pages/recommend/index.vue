<template>
  <view class="anime-container">
    <view class="anime-header">
      <text class="anime-title">GAL 游戏推荐</text>
    </view>
    
    <view class="anime-search-bar">
      <view class="anime-search-input">
        <text class="anime-search-icon">🔍</text>
        <input type="text" v-model="searchKeyword" placeholder="搜索游戏名称或标签" placeholder-class="anime-search-placeholder" />
      </view>
      <button class="anime-search-btn" @click="searchGame">搜索</button>
    </view>
    
    <view class="anime-filter-section">
      <view class="anime-filter-tabs">
        <view class="anime-filter-tab" :class="{ 'active': currentTab === 'recommend' }" @click="switchTab('recommend')">
          <text>推荐</text>
        </view>
        <view class="anime-filter-tab" :class="{ 'active': currentTab === 'popular' }" @click="switchTab('popular')">
          <text>热门</text>
        </view>
        <view class="anime-filter-tab" :class="{ 'active': currentTab === 'new' }" @click="switchTab('new')">
          <text>最新</text>
        </view>
      </view>
    </view>
    
    <scroll-view class="anime-game-list" scroll-y>
      <view class="anime-game-item" v-for="game in gameList" :key="game.id" @click="viewGameDetail(game.id)">
        <image class="anime-game-cover" :src="game.coverUrl" mode="aspectFill"></image>
        <view class="anime-game-info">
          <text class="anime-game-title">{{ game.title }}</text>
          <text class="anime-game-description">{{ game.description }}</text>
          <view class="anime-game-tags">
            <text class="anime-game-tag" v-for="tag in game.tags" :key="tag">{{ tag }}</text>
          </view>
          <view class="anime-game-score">
            <text class="anime-score-label">评分:</text>
            <text class="anime-score-value">{{ game.score }}</text>
          </view>
        </view>
      </view>
    </scroll-view>
    
    <view class="anime-footer">
      <text class="anime-footer-text">GAL 批评工具箱 © 2023</text>
    </view>
  </view>
</template>

<script>
import { mapState, mapMutations } from 'vuex';

export default {
  data() {
    return {
      searchKeyword: '',
      currentTab: 'recommend',
      gameList: [
        {
          id: 1,
          title: '樱花飞舞的季节',
          description: '一个关于青春与回忆的感人故事',
          coverUrl: '/static/images/game1.jpg',
          tags: ['恋爱', '校园', '催泪'],
          score: 4.8
        },
        {
          id: 2,
          title: '星空下的约定',
          description: '跨越时空的浪漫恋曲',
          coverUrl: '/static/images/game2.jpg',
          tags: ['科幻', '爱情', '悬疑'],
          score: 4.6
        },
        {
          id: 3,
          title: '梦幻岛',
          description: '寻找失落记忆的奇幻冒险',
          coverUrl: '/static/images/game3.jpg',
          tags: ['奇幻', '冒险', '解谜'],
          score: 4.7
        },
        {
          id: 4,
          title: '夏日回忆',
          description: '夏日里的青春物语',
          coverUrl: '/static/images/game4.jpg',
          tags: ['日常', '青春', '治愈'],
          score: 4.5
        },
        {
          id: 5,
          title: '无限轮回',
          description: '在时间循环中寻找真相',
          coverUrl: '/static/images/game5.jpg',
          tags: ['悬疑', '推理', '时间循环'],
          score: 4.9
        }
      ]
    };
  },
  computed: {
    ...mapState(['userInfo'])
  },
  methods: {
    ...mapMutations(['setCurrentGameId']),
    
    switchTab(tab) {
      this.currentTab = tab;
      // 这里可以根据不同的标签加载不同的游戏列表
      this.loadGameList(tab);
    },
    
    loadGameList(tab) {
      // 模拟根据标签加载不同的游戏列表
      // 实际项目中应该调用API获取数据
      console.log(`加载${tab}游戏列表`);
      // 这里可以根据不同的标签过滤游戏列表
    },
    
    searchGame() {
      if (this.searchKeyword.trim()) {
        // 这里可以根据搜索关键词过滤游戏列表
        console.log(`搜索游戏: ${this.searchKeyword}`);
        // 实际项目中应该调用搜索API
      }
    },
    
    viewGameDetail(gameId) {
      this.setCurrentGameId(gameId);
      uni.navigateTo({
        url: `/pages/game/index?id=${gameId}`
      });
    }
  },
  onLoad() {
    this.loadGameList(this.currentTab);
  }
};
</script>

<style lang="scss" scoped>
.anime-container {
  width: 100%;
  min-height: 100vh;
  background-color: #FFF0F5;
  display: flex;
  flex-direction: column;
}

.anime-header {
  padding: 30rpx;
  background: linear-gradient(135deg, #FF91A4, #FFB6C1);
  border-bottom-left-radius: 30rpx;
  border-bottom-right-radius: 30rpx;
  box-shadow: 0 4rpx 12rpx rgba(255, 145, 164, 0.3);
}

.anime-title {
  font-size: 48rpx;
  font-weight: bold;
  color: #FFFFFF;
  text-align: center;
  display: block;
  text-shadow: 2rpx 2rpx 4rpx rgba(0, 0, 0, 0.2);
}

.anime-search-bar {
  padding: 30rpx;
  display: flex;
  gap: 20rpx;
}

.anime-search-input {
  flex: 1;
  background: #FFFFFF;
  border-radius: 40rpx;
  padding: 20rpx 30rpx;
  display: flex;
  align-items: center;
  gap: 20rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}

.anime-search-icon {
  font-size: 32rpx;
  color: #FF91A4;
}

.anime-search-input input {
  flex: 1;
  font-size: 30rpx;
  color: #333333;
  background: transparent;
}

.anime-search-placeholder {
  color: #CCCCCC;
}

.anime-search-btn {
  background: #FF91A4;
  color: #FFFFFF;
  border: none;
  border-radius: 40rpx;
  padding: 0 40rpx;
  font-size: 30rpx;
  font-weight: 500;
  box-shadow: 0 2rpx 8rpx rgba(255, 145, 164, 0.3);
  transition: all 0.3s ease;
}

.anime-search-btn:active {
  transform: scale(0.95);
  box-shadow: 0 1rpx 4rpx rgba(255, 145, 164, 0.2);
}

.anime-filter-section {
  padding: 0 30rpx 20rpx;
}

.anime-filter-tabs {
  display: flex;
  gap: 30rpx;
  background: #FFFFFF;
  border-radius: 40rpx;
  padding: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}

.anime-filter-tab {
  flex: 1;
  text-align: center;
  padding: 15rpx 0;
  border-radius: 30rpx;
  transition: all 0.3s ease;
}

.anime-filter-tab.active {
  background: #FF91A4;
  color: #FFFFFF;
  font-weight: 500;
}

.anime-filter-tab text {
  font-size: 30rpx;
  color: #333333;
}

.anime-filter-tab.active text {
  color: #FFFFFF;
}

.anime-game-list {
  flex: 1;
  padding: 0 30rpx 30rpx;
}

.anime-game-item {
  margin-top: 30rpx;
  background: #FFFFFF;
  border-radius: 30rpx;
  overflow: hidden;
  display: flex;
  box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.anime-game-item:active {
  transform: translateY(4rpx);
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.05);
}

.anime-game-cover {
  width: 240rpx;
  height: 320rpx;
  border-top-left-radius: 30rpx;
  border-bottom-left-radius: 30rpx;
}

.anime-game-info {
  flex: 1;
  padding: 20rpx;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.anime-game-title {
  font-size: 34rpx;
  font-weight: bold;
  color: #333333;
  margin-bottom: 10rpx;
  line-height: 44rpx;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.anime-game-description {
  font-size: 26rpx;
  color: #666666;
  line-height: 36rpx;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  margin-bottom: 15rpx;
}

.anime-game-tags {
  display: flex;
  gap: 10rpx;
  flex-wrap: wrap;
  margin-bottom: 15rpx;
}

.anime-game-tag {
  background: #FFEFF4;
  color: #FF91A4;
  font-size: 24rpx;
  padding: 6rpx 16rpx;
  border-radius: 20rpx;
}

.anime-game-score {
  display: flex;
  align-items: center;
  gap: 10rpx;
}

.anime-score-label {
  font-size: 26rpx;
  color: #666666;
}

.anime-score-value {
  font-size: 30rpx;
  font-weight: bold;
  color: #FF91A4;
}

.anime-footer {
  padding: 30rpx;
  text-align: center;
}

.anime-footer-text {
  font-size: 24rpx;
  color: #999999;
}
</style>