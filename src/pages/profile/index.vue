<template>
  <view class="page">
    <!-- 顶部背景 -->
    <view class="header">
      <text class="header-title">我的</text>
    </view>

    <!-- 登录状态 -->
    <view v-if="isLoggedIn" class="login-section">
      <text class="login-text">
        {{ userName ? `${userName}欢迎您！` : "未设置昵称" }}
      </text>
      <button v-if="!userName" class="set-nickname-button" @click="setNickname">
        设置昵称
      </button>
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
        v-for="(item, index) in additionalFeatures"
        :key="index"
        @click="navigateToFeature(item.path)"
      >
        <text class="feature-text">{{ item.text }}</text>
      </view>
    </view>

    <!-- 关于我们和版本号 -->
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
      additionalFeatures: [
        { text: "我的报名", path: "/pages/user/registrations/index" },
        { text: "我的订单", path: "/pages/user/orders/index" },
        { text: "我的收藏", path: "/pages/user/favorites/index" },
        { text: "我的互动记录", path: "/pages/user/interactions/index" },
      ],
    };
  },
  onShow() {
    const token = uni.getStorageSync("user_token");
    if (!token) {
      uni.showToast({ title: "还未登录", icon: "none" });
    }
    this.checkLoginStatus();
  },
  methods: {
    navigateToLogin() {
      uni.navigateTo({ url: "/pages/user/login/index" });
    },
    navigateToFeature(path) {
      if (path) {
        uni.navigateTo({ url: path });
      }
    },
    checkLoginStatus() {
      const token = uni.getStorageSync("user_token");
      if (token) {
        this.isLoggedIn = true;
        this.fetchUserName();
      } else {
        this.isLoggedIn = false;
      }
    },
    fetchUserName() {
      const token = uni.getStorageSync("user_token");
      uni.request({
        url: "https://sports.ziven.site/api/user/info",
        method: "GET",
        header: {
          Authorization: `Bearer ${token}`,
        },
        success: (res) => {
          if (res.statusCode === 200 && res.data.code === 200) {
            this.userName = res.data.data.userInfo.nickName || "";
            uni.setStorageSync("user_nickname", this.userName); // 保存昵称到本地缓存
          } else {
            console.error("获取用户信息失败:", res);
          }
        },
        fail: (err) => {
          console.error("获取用户信息请求失败:", err);
        },
      });
    },
    setNickname() {
      const token = uni.getStorageSync("user_token");
      const promptNickname = () => {
        uni.showModal({
          title: "设置昵称",
          editable: true,
          success: (res) => {
            if (res.confirm && res.content) {
              const nickname = res.content.trim();
              if (nickname) {
                uni.request({
                  url: "https://sports.ziven.site/api/user/info/nickName",
                  method: "POST",
                  header: {
                    Authorization: `Bearer ${token}`,
                  },
                  data: { nickName: nickname },
                  success: (response) => {
                    if (
                      response.statusCode === 200 &&
                      response.data.code === 200
                    ) {
                      uni.showToast({ title: "昵称设置成功", icon: "success" });
                      this.userName = nickname;
                      uni.setStorageSync("user_nickname", nickname); // 保存昵称到本地缓存
                    } else if (
                      response.data.code === 4000 &&
                      response.data.message === "昵称已存在"
                    ) {
                      uni.showModal({
                        title: "提示",
                        content: "昵称已存在，请重新输入",
                        showCancel: false,
                        success: () => {
                          promptNickname(); // 重新提示用户输入昵称
                        },
                      });
                    } else {
                      uni.showToast({ title: "昵称设置失败", icon: "none" });
                    }
                  },
                  fail: (error) => {
                    console.error("设置昵称请求失败:", error);
                    uni.showToast({ title: "网络错误", icon: "none" });
                  },
                });
              } else {
                uni.showToast({ title: "昵称不能为空", icon: "none" });
              }
            }
          },
        });
      };
      promptNickname(); // 调用设置昵称逻辑
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
  background: linear-gradient(90deg, #4caf50, #388e3c); /* 修改为绿色渐变 */
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
.set-nickname-button {
  background-color: #4caf50;
  color: white;
  font-size: 28rpx;
  padding: 10rpx 20rpx;
  border-radius: 5rpx;
  border: none;
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
  background-color: #4caf50; /* 修改为绿色 */
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
