<template>
  <view class="page">
    <text>我的报名页面</text>
  </view>
</template>

<script>
export default {
  name: "RegistrationsPage",
  onShow() {
    const token = uni.getStorageSync("user_token");
    if (!token) {
      uni.showToast({ title: "还未登录", icon: "none" });
      uni.switchTab({ url: "/pages/profile/index" }); // 返回到“我的”页面
    }
  },
};
</script>

<style scoped>
.page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background-color: #f8f8f8;
}
</style>
