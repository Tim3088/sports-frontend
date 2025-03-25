<template>
  <view class="login-page">
    <view class="logo">
      <image src="/static/logo.png" class="logo-image" mode="aspectFit" />
    </view>
    <view class="form">
      <button class="login-button wechat-login" @click="handleLogin">
        微信登录
      </button>
    </view>
    <view class="footer">
      <text class="footer-text">© 2025 版权所有 zjut</text>
    </view>
  </view>
</template>

<script>
export default {
  methods: {
    async handleLogin() {
      try {
        const loginRes = await new Promise((resolve, reject) => {
          uni.login({
            provider: "weixin",
            success: resolve,
            fail: reject,
          });
        });

        console.log("微信登录成功:", loginRes.code);

        const response = await uni.request({
          url: `https://sports.ziven.site/api/user/wxLogin/${loginRes.code}`,
          method: "GET",
        });

        if (response.statusCode === 200 && response.data.code === 200) {
          const token = response.data.data?.token; // 确保从 data 中正确获取 token
          if (token) {
            console.log("获取 token 成功:", token);
            try {
              uni.setStorageSync("user_token", token); // 保存 token 到本地存储
              console.log("token 已成功存储到本地:", token);
            } catch (storageError) {
              console.error("存储 token 失败:", storageError);
            }
            uni.showToast({ title: "登录成功", icon: "success" });
          } else {
            console.error("响应中未找到 token:", response.data);
            uni.showToast({ title: "登录失败", icon: "none" });
          }
        } else {
          console.error("获取 token 失败:", response);
          uni.showToast({ title: "登录失败", icon: "none" });
        }
      } catch (error) {
        console.error("登录流程出错:", error);
        uni.showToast({ title: "登录失败", icon: "none" });
      }
    },
  },
};
</script>

<style scoped>
.login-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between; /* 确保内容均匀分布 */
  height: 100%;
  background-color: #f8f8f8; /* 统一背景颜色 */
}
.logo {
  margin-top: 40rpx;
}
.logo-image {
  width: 200rpx;
  height: 200rpx;
}
.form {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-grow: 1; /* 占据中间的空间 */
  width: 100%;
}
.login-button {
  width: 80%;
  height: 80rpx;
  border-radius: 10rpx;
  font-size: 28rpx;
  color: #fff;
  text-align: center;
  line-height: 80rpx;
}
.wechat-login {
  background-color: #4caf50;
}
.footer {
  width: 100%;
  padding: 20rpx;
  text-align: center;
  background-color: #f8f8f8; /* 与页面背景一致 */
}
.footer-text {
  font-size: 24rpx;
  color: #999;
}
</style>
