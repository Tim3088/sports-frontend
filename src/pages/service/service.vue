<template>
  <view class="page">
    <!-- 顶部标题栏 -->
    <view class="header">
      <text class="title">服务</text>
    </view>

    <!-- 功能区 -->
    <view class="features">
      <view
        class="feature"
        v-for="(item, index) in featureList"
        :key="index"
        @click="navigateToFeature(item.path)"
      >
        <view class="feature-icon-wrapper">
          <image class="feature-icon" :src="item.icon" mode="aspectFit"></image>
        </view>
        <text class="feature-text">{{ item.text }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      featureList: [
        {
          icon: "/static/icons/education.png",
          text: "木球教育培训",
          path: "https://mp.weixin.qq.com/s/iWkAFLancveZg2BoIABLlg", 
        },
        {
          icon: "/static/icons/equipment.png",
          text: "木球器材器械",
          path: "/pages/equipment/index",
        },
        {
          icon: "/static/icons/health.png",
          text: "木球健康旅居",
          path: "/pages/health/index",
        },
        {
          icon: "/static/icons/events.png",
          text: "木球赛事",
          path: "/pages/events/index",
        },
      ],
    };
  },
  methods: {
    navigateToFeature(path) {
      if (path.startsWith("http")) {
        // 外部链接跳转
        uni.navigateTo({
          url: `/pages/webview/index?url=${encodeURIComponent(path)}`,
        });
      } else {
        // 内部页面跳转
        uni.navigateTo({ url: path });
      }
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
.features {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  margin: 20rpx;
  padding: 20rpx 0;
  background-color: #fff;
  border-radius: 15rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.08);
}
.feature {
  width: 22%;
  text-align: center;
  margin-bottom: 30rpx;
  transition: transform 0.2s;
}
.feature:active {
  transform: scale(0.95);
}
.feature-icon-wrapper {
  width: 120rpx;
  height: 120rpx;
  margin: 0 auto 15rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #e0f7fa, #b2ebf2);
  border-radius: 50%;
  box-shadow: 0 4rpx 10rpx rgba(0, 188, 212, 0.2);
  position: relative;
  overflow: hidden;
}
.feature-icon {
  width: 70rpx;
  height: 70rpx;
}
.feature-text {
  font-size: 28rpx;
  color: #333;
  font-weight: 500;
}
</style>
