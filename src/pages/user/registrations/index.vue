<template>
  <view class="page">
    <view class="header">
      <text class="title">我的报名</text>
    </view>
    <view v-if="registrations.length" class="registrations">
      <view
        class="registration"
        v-for="(registration, index) in registrations"
        :key="index"
      >
        <text class="course-title">{{ registration.title }}\n</text>
        <text class="course-info">时间: {{ registration.time }}\n</text>
        <text class="course-info">地点: {{ registration.location }}</text>
      </view>
    </view>
    <view v-else class="empty">
      <text>暂无报名记录</text>
    </view>
  </view>
</template>

<script>
export default {
  name: "RegistrationsPage",
  data() {
    return {
      registrations: [], // 存储报名信息
    };
  },
  onShow() {
    const token = uni.getStorageSync("user_token");
    if (!token) {
      uni.showToast({ title: "还未登录", icon: "none" });
      uni.switchTab({ url: "/pages/profile/index" }); // 返回到“我的”页面
      return;
    }
    this.fetchRegistrations(); // 获取报名信息
  },
  methods: {
    async fetchRegistrations() {
      try {
        const token = uni.getStorageSync("user_token");
        const response = await uni.request({
          url: "https://sports.ziven.site/api/user/courses", // 更新后的后端接口地址
          method: "GET",
          header: {
            Authorization: `Bearer ${token}`, // 使用 Bearer Token
          },
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          this.registrations = response.data.data.userCourses; // 适配后端返回的 userCourses 字段
        } else {
          console.error("获取报名信息失败:", response);
          uni.showToast({ title: "获取报名信息失败", icon: "none" });
        }
      } catch (error) {
        console.error("获取报名信息请求出错:", error);
        uni.showToast({ title: "网络错误", icon: "none" });
      }
    },
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
  margin-bottom: 20rpx;
  background: linear-gradient(90deg, #00bcd4, #0288d1);
  padding: 20rpx;
  color: white;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.registrations {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}
.registration {
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}
.course-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}
.course-info {
  font-size: 24rpx;
  color: #666;
}
.empty {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  color: #999;
  font-size: 28rpx;
}
</style>
