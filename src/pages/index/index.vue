<template>
  <view class="page">
    <!-- 顶部标题栏 -->
    <view class="header">
      <text class="title">木球运动</text>
    </view>

    <!-- 木球运动简介 -->
    <view class="intro">
      <text class="intro-text">
        木球运动是一项集健身、娱乐、竞技于一体的体育活动，倡导全民参与，传递健康生活理念。
      </text>
    </view>

    <!-- 最新动态轮播图 -->
    <view class="swiper-container">
      <swiper autoplay="true" interval="3000" circular="true" class="swiper">
        <swiper-item v-for="(item, index) in dynamicList" :key="index">
          <image :src="item.image" class="swiper-image" />
          <text class="swiper-text">{{ item.text }}</text>
        </swiper-item>
      </swiper>
    </view>

    <!-- 快捷导航入口 -->
    <view class="quick-nav">
      <view
        class="nav-item"
        v-for="(item, index) in navList"
        :key="index"
        @click="navigateToFeature(item.path)"
      >
        <image :src="item.icon" class="nav-icon" />
        <text class="nav-text">{{ item.text }}</text>
      </view>
    </view>

    <!-- 通知公告 -->
    <view class="announcement">
      <text class="announcement-title">通知公告</text>
      <view class="announcement-content">
        <text>倡导全民木球</text>
        <text class="announcement-date">2025年3月25日</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      dynamicList: [
        { image: "/static/images/event1.jpg", text: "近期赛事报名" },
        { image: "/static/images/training.jpg", text: "木球培训优惠活动" },
        { image: "/static/images/discount.jpg", text: "器材器械限时优惠" },
      ],
      navList: [
        {
          icon: "/static/icons/education.png",
          text: "教育培训",
          path: "/pages/user/education/index",
        },
        {
          icon: "/static/icons/equipment.png",
          text: "器材器械",
          path: "/pages/user/equipment/index",
        },
        {
          icon: "/static/icons/health.png",
          text: "健康旅居",
          path: "/pages/user/health/index", // 更新路径
        },
        {
          icon: "/static/icons/events.png",
          text: "木球赛事",
          path: "/pages/user/events/index",
        },
        {
          icon: "/static/icons/community.png",
          text: "球友互动",
          path: "/pages/user/community/index",
        },
        {
          icon: "/static/icons/news.png",
          text: "木球新闻",
          path: "/pages/user/news/index",
        },
      ],
    };
  },
  methods: {
    navigateToFeature(path) {
      uni.navigateTo({ url: path });
    },
  },
};
</script>

<style scoped>
.page {
  background-color: #f5f5f5;
  display: flex;
  flex-direction: column;
  height: 100%;
}
.header {
  background: linear-gradient(90deg, #00bcd4, #0288d1);
  color: white;
  padding: 20rpx;
  text-align: center;
}
.header .title {
  font-size: 36rpx;
}
.intro {
  padding: 20rpx;
  background-color: #fff;
  margin: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
.intro-text {
  font-size: 28rpx;
  color: #333;
  line-height: 1.5;
}
.swiper-container {
  margin: 20rpx;
}
.swiper {
  height: 300rpx;
  border-radius: 10rpx;
  overflow: hidden;
}
.swiper-image {
  width: 100%;
  height: 100%;
}
.swiper-text {
  position: absolute;
  bottom: 10rpx;
  left: 20rpx;
  color: white;
  font-size: 24rpx;
  text-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.5);
}
.quick-nav {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 20rpx;
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
.nav-item {
  width: 30%;
  text-align: center;
  margin-bottom: 20rpx;
}
.nav-icon {
  width: 80rpx;
  height: 80rpx;
  margin-bottom: 10rpx;
}
.nav-text {
  font-size: 24rpx;
  color: #333;
}
.announcement {
  background-color: #fff;
  margin: 20rpx;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
.announcement-title {
  color: #0288d1;
  font-weight: bold;
  margin-bottom: 10rpx;
}
.announcement-content {
  display: flex;
  justify-content: space-between;
  font-size: 28rpx;
}
.announcement-date {
  color: #999;
}
</style>
