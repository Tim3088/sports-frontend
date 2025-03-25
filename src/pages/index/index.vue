<template>
  <view class="page">
    <!-- 顶部标题栏 -->
    <view class="header">
      <text class="title">木球运动</text>
    </view>

    <!-- 横幅区域 -->
    <view class="banner">
      <image
        class="banner-image"
        src="/static/banner.png"
        mode="aspectFill"
      ></image>
      <view class="banner-text">
        <text>木球服务</text>
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
      featureList: [], // 删除功能区数据
    };
  },
  methods: {
    navigateToFeature(path) {
      // 页面导航逻辑
      uni.navigateTo({
        url: path,
      });
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
.header .location {
  font-size: 28rpx;
  margin-top: 10rpx;
}
.banner {
  position: relative;
  margin: 20rpx;
}
.banner-image {
  width: 100%;
  border-radius: 20rpx;
}
.banner-text {
  position: absolute;
  bottom: 20rpx;
  left: 20rpx;
  color: white;
  font-size: 28rpx;
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
