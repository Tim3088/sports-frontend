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
    <view v-if="loading" class="loading-overlay">
      <text class="loading-text">登录中...</text>
    </view>
  </view>
</template>

<script>
export default {
  onLoad(options) {
    this.redirect = options.redirect || "/pages/profile/index"; // 登录成功后跳转的页面
  },
  data() {
    return {
      redirect: "",
      loading: false, // 添加 loading 状态
    };
  },
  methods: {
    async handleLogin() {
      if (uni.getStorageSync("user_token")) {
        uni.showToast({
          title: "您已登录，请勿重复登录",
          icon: "none",
        });
        return;
      }
      this.loading = true; // 开始显示加载状态
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

              // 检查用户是否已设置昵称
              const userInfoResponse = await uni.request({
                url: "https://sports.ziven.site/api/user/info",
                method: "GET",
                header: {
                  Authorization: `Bearer ${token}`,
                },
              });

              if (
                userInfoResponse.statusCode === 200 &&
                userInfoResponse.data.code === 200
              ) {
                const userInfo = userInfoResponse.data.data.userInfo;
                if (!userInfo.nickName) {
                  console.log("用户未设置昵称，提示用户输入昵称...");
                  const setNickname = async () => {
                    uni.showModal({
                      title: "首次登陆请设置昵称",
                      editable: true, // 允许用户输入
                      showCancel: false, // 禁用取消按钮
                      success: async (modalRes) => {
                        if (modalRes.confirm && modalRes.content) {
                          const nickname = modalRes.content.trim();
                          console.log("用户输入的昵称:", nickname);
                          if (nickname) {
                            const setNicknameResponse = await uni.request({
                              url: "https://sports.ziven.site/api/user/info/nickName",
                              method: "POST",
                              header: {
                                Authorization: `Bearer ${token}`,
                              },
                              data: {
                                nickName: nickname, // 使用用户输入的昵称
                              },
                            });

                            if (
                              setNicknameResponse.statusCode === 200 &&
                              setNicknameResponse.data.code === 200
                            ) {
                              console.log("昵称设置成功");
                              uni.showToast({
                                title: "昵称设置成功",
                                icon: "success",
                              });
                              uni.setStorageSync("user_nickname", nickname); // 保存昵称到本地缓存
                            } else if (
                              setNicknameResponse.data.code === 4000 &&
                              setNicknameResponse.data.message === "昵称已存在"
                            ) {
                              console.error("昵称已存在:", setNicknameResponse);
                              uni.showModal({
                                title: "提示",
                                content: "昵称已存在，请重新输入",
                                showCancel: false, // 禁用取消按钮
                                success: () => {
                                  setNickname(); // 用户点击确定后重新提示输入昵称
                                },
                              });
                            } else {
                              console.error(
                                "设置昵称失败:",
                                setNicknameResponse
                              );
                              uni.showToast({
                                title: "设置昵称失败",
                                icon: "none",
                              });
                              setNickname(); // 重新提示用户输入昵称
                            }
                          } else {
                            uni.showModal({
                              title: "提示",
                              content: "昵称不能为空，请重新输入",
                              showCancel: false, // 禁用取消按钮
                              success: () => {
                                setNickname(); // 用户点击确定后重新提示输入昵称
                              },
                            });
                          }
                        }
                      },
                    });
                  };
                  setNickname(); // 调用设置昵称逻辑
                }
              } else {
                console.error("获取用户信息失败:", userInfoResponse);
              }
            } catch (storageError) {
              console.error("存储 token 失败:", storageError);
            }
            uni.showToast({ title: "登录成功", icon: "success" });
            setTimeout(() => {
              uni.redirectTo({ url: this.redirect }); // 登录成功后跳转到指定页面
            }, 1000);
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
      } finally {
        this.loading = false; // 隐藏加载状态
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
.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}
.loading-text {
  color: #fff;
  font-size: 28rpx;
}
</style>
