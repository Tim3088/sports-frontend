<template>
  <view class="page">
    <view class="header">
      <text class="title">关于我们</text>
    </view>
    <view class="content">
      <text class="section-title">【团队简介】</text>
      <text class="paragraph"> 木球文化宣传团队 </text>
    </view>
  </view>
</template>

<script>
export default {
  name: "AboutPage",
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
  background: linear-gradient(
    90deg,
    #00bcd4,
    #0288d1
  ); /* 修改为与其他页面一致 */
  padding: 20rpx; /* 修改为与其他页面一致 */
  color: white; /* 设置文字颜色为白色 */
}
.title {
  font-size: 36rpx;
  font-weight: bold;
  color: white; /* 修改为白色 */
}
.content {
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
.paragraph {
  font-size: 28rpx;
  color: #666;
  line-height: 1.5;
  margin-bottom: 20rpx;
}
</style>
