<template>
  <view class="page">
    <view class="header">
      <text class="title">木球新闻</text>
    </view>
    <view class="content">
      <view class="news-item" v-for="(news, index) in newsList" :key="index">
        <image :src="news.image" class="news-image" />
        <view class="news-text">
          <text class="news-title">{{ news.title }}</text>
          <text class="news-date">{{ news.date }}</text>
        </view>
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
          title: "木球赛事圆满落幕",
          date: "2025-03-20",
          image: "/static/images/news1.jpg",
        },
        {
          title: "木球培训活动启动",
          date: "2025-03-18",
          image: "/static/images/news2.jpg",
        },
        {
          title: "木球文化推广计划",
          date: "2025-03-15",
          image: "/static/images/news3.jpg",
        },
      ],
    };
  },
};
</script>

<style scoped>
.page {
  background-color: #f5f5f5;
  padding: 20rpx;
}
.header {
  background: linear-gradient(90deg, #00bcd4, #0288d1);
  color: white;
  padding: 20rpx;
  text-align: center;
  border-radius: 10rpx;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.content {
  margin-top: 20rpx;
}
.news-item {
  display: flex;
  background-color: #fff;
  margin-bottom: 20rpx;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
.news-image {
  width: 100rpx;
  height: 100rpx;
  margin-right: 20rpx;
  border-radius: 10rpx;
}
.news-text {
  flex: 1;
}
.news-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}
.news-date {
  font-size: 24rpx;
  color: #666;
}
</style>
