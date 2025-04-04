<template>
  <view class="page">
    <!-- 页面标题 -->
    <view class="header">
      <text class="title">木球赛事</text>
    </view>

    <!-- 最新赛事资讯 -->
    <view class="section">
      <text class="section-title">最新赛事资讯</text>
      <view class="news" v-for="(news, index) in newsList" :key="index">
        <text class="news-title">{{ news.title }}</text>
        <text class="news-date">{{ news.date }}</text>
        <text class="news-content">{{ news.content }}</text>
      </view>
    </view>

    <!-- 赛事报名通道 -->
    <view class="section">
      <text class="section-title">赛事报名通道</text>
      <button class="signup-button" @click="navigateToSignup">立即报名</button>
    </view>

    <!-- 赛事直播与回放专区 -->
    <view class="section">
      <text class="section-title">赛事直播与回放专区</text>
      <view class="video" v-for="(video, index) in videos" :key="index">
        <text class="video-title">{{ video.title }}</text>
        <video :src="video.url" controls class="video-player"></video>
      </view>
    </view>

    <!-- 选手风采展示 -->
    <view class="section">
      <text class="section-title">选手风采展示</text>
      <view class="gallery">
        <image
          v-for="(image, index) in playerGallery"
          :key="index"
          :src="image"
          class="gallery-image"
        />
      </view>
    </view>

    <!-- 赛事规则及裁判指南 -->
    <view class="section">
      <text class="section-title">赛事规则及裁判指南</text>
      <view class="rules">
        <text v-for="(rule, index) in rules" :key="index" class="rule-item">
          {{ rule }}
        </text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      newsList: [
        {
          title: "木球锦标赛即将开赛\n",
          date: "2025-04-01\n",
          content: "敬请期待！",
        },
        {
          title: "木球公开赛赛程公布\n",
          date: "2025-03-25\n",
          content: "查看详情",
        },
      ],
      videos: [
        { title: "木球锦标赛直播", url: "/static/videos/live.mp4" },
        { title: "木球公开赛回放", url: "/static/videos/replay.mp4" },
      ],
      playerGallery: [
        "/static/images/player1.jpg",
        "/static/images/player2.jpg",
        "/static/images/player3.jpg",
      ],
      rules: [
        "1. 比赛采用国际木球联合会最新规则。\n",
        "2. 裁判需持有国家级裁判证书。\n",
        "3. 参赛选手需提前30分钟到场签到。",
      ],
    };
  },
  methods: {
    navigateToSignup() {
      uni.navigateTo({ url: "/pages/signup/index" });
    },
  },
};
</script>

<style scoped>
.page {
  padding: 20rpx;
  background-color: #f5f5f5;
}
.header {
  text-align: center;
  margin-bottom: 20rpx;
  background: linear-gradient(90deg, #4caf50, #388e3c); /* 修改为绿色渐变 */
  padding: 20rpx;
  color: white;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.section {
  margin-bottom: 30rpx;
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}
.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}
.news {
  margin-bottom: 20rpx;
}
.news-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
}
.news-date {
  font-size: 24rpx;
  color: #999;
}
.news-content {
  font-size: 24rpx;
  color: #666;
}
.signup-button {
  width: 100%;
  height: 80rpx;
  background-color: #4caf50; /* 修改为绿色 */
  color: white;
  font-size: 28rpx;
  text-align: center;
  line-height: 80rpx;
  border-radius: 10rpx;
}
.video {
  margin-bottom: 20rpx;
}
.video-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}
.video-player {
  width: 100%;
  height: 200rpx;
  border-radius: 10rpx;
}
.gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 10rpx;
}
.gallery-image {
  width: 100rpx;
  height: 100rpx;
  border-radius: 10rpx;
}
.rules {
  margin-top: 10rpx;
}
.rule-item {
  font-size: 24rpx;
  color: #666;
  margin-bottom: 10rpx;
}
</style>
