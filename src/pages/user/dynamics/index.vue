<template>
  <view class="page">
    <view class="header">
      <text class="title">球友动态</text>
    </view>
    <view class="content">
      <view class="dynamic" v-for="(dynamic, index) in dynamics" :key="index">
        <text class="dynamic-author">{{ dynamic.author }}</text>
        <text class="dynamic-content">{{ dynamic.content }}</text>
        <text class="dynamic-time">{{ dynamic.time }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      dynamics: [
        {
          author: "球友A",
          content: "刚刚完成了一场精彩的木球比赛！",
          time: "10分钟前",
        },
        // ...other dynamics...
      ],
    };
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
  background: linear-gradient(90deg, #00bcd4, #0288d1);
  padding: 20rpx;
  color: white;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.content {
  margin-top: 20rpx;
}
.dynamic {
  background-color: #fff;
  margin-bottom: 20rpx;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
.dynamic-author {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
}
.dynamic-content {
  font-size: 24rpx;
  color: #666;
  margin-top: 10rpx;
}
.dynamic-time {
  font-size: 20rpx;
  color: #999;
  margin-top: 5rpx;
}
</style>
