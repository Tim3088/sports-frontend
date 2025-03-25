<template>
  <view class="page">
    <!-- 顶部背景 -->
    <view class="header">
      <text class="header-title">我的</text>
    </view>

    <!-- 登录状态 -->
    <view v-if="isLoggedIn" class="login-section">
      <text class="login-text">{{ userName }}已登录</text>
    </view>

    <!-- 未登录提示 -->
    <view v-else class="login-section">
      <text class="login-text">您还没有登录</text>
      <view class="login-subtext-wrapper">
        <text class="login-subtext">点击立即登录，获取更多服务！</text>
      </view>
      <view class="login-button-wrapper">
        <button class="login-button" @click="navigateToLogin">立即登录</button>
      </view>
    </view>

    <!-- 功能列表 -->
    <view class="features">
      <view
        class="feature-item"
        v-for="(item, index) in featureList"
        :key="index"
        @click="navigateToFeature(item.path)"
      >
        <text class="feature-text">{{ item.text }}</text>
        <text class="feature-version" v-if="item.version">{{
          item.version
        }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      isLoggedIn: false,
      userName: "",
      featureList: [
        { text: "关于我们", path: "/pages/about/index" },
        { text: "版本号", version: "1.0.0" },
      ],
    };
  },
  onShow() {
    console.log("页面显示");
    this.checkLoginStatus(); // 执行页面显示时的逻辑
  },
  methods: {
    navigateToLogin() {
      uni.navigateTo({ url: "/pages/login/index" });
    },
    navigateToFeature(path) {
      if (path) {
        uni.navigateTo({ url: path });
      }
    },
    checkLoginStatus() {
      const token = uni.getStorageSync("user_token");
      console.log("token:", token);
      if (token) {
        this.isLoggedIn = true;
        this.fetchUserName();
      } else {
        this.isLoggedIn = false;
      }
    },
    fetchUserName() {
      uni.getUserProfile({
        success: (res) => {
          console.log("获取用户信息成功:", res);
          this.userName = res.userInfo.nickName || "用户";
        },
        fail: (err) => {
          console.error("获取用户信息失败:", err);
          this.userName = "用户";
        },
      });
    },
  },
};
</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  background-color: #f5f5f5;
  height: 100%;
}
.header {
  background: linear-gradient(90deg, #00bcd4, #0288d1);
  padding: 20rpx;
  text-align: center;
}
.header-title {
  color: white;
  font-size: 36rpx;
  font-weight: bold;
}
.login-section {
  background-color: #fff;
  margin: 20rpx;
  padding: 30rpx;
  border-radius: 15rpx;
  text-align: center;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}
.login-text {
  font-size: 32rpx;
  color: #333;
  margin-bottom: 10rpx;
}
.login-subtext-wrapper {
  margin-top: 10rpx;
}
.login-subtext {
  font-size: 28rpx;
  color: #999;
}
.login-button-wrapper {
  margin-top: 20rpx;
  text-align: center;
}
.login-button {
  background-color: #00bcd4;
  color: white;
  font-size: 28rpx;
  padding: 10rpx 20rpx;
  border-radius: 5rpx;
  border: none;
}
.features {
  margin: 20rpx;
  background-color: #fff;
  border-radius: 15rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}
.feature-item {
  display: flex;
  justify-content: space-between;
  padding: 20rpx;
  border-bottom: 1rpx solid #f0f0f0;
}
.feature-item:last-child {
  border-bottom: none;
}
.feature-text {
  font-size: 28rpx;
  color: #333;
}
.feature-version {
  font-size: 28rpx;
  color: #999;
}
</style>
