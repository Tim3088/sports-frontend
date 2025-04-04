<template>
  <view class="page">
    <view class="header">
      <text class="title">我的报名</text>
    </view>
    <view v-if="registrations.length" class="registrations">
      <view
        class="registration"
        v-for="(registration, index) in registrations"
        :key="registration.courseId"
      >
        <text class="course-title">{{ registration.title }}\n</text>
        <text class="course-info">时间: {{ registration.time }}\n</text>
        <text class="course-info">地点: {{ registration.location }}</text>
        <button
          class="cancel-button"
          @click="cancelRegistration(registration.courseId, index)"
        >
          取消报名
        </button>
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
      }
    },
    async cancelRegistration(courseId, index) {
      try {
        const token = uni.getStorageSync("user_token");
        const response = await uni.request({
          url: `https://sports.ziven.site/api/user/courses/${courseId}`, // 更新后的后端接口地址
          method: "DELETE",
          header: {
            Authorization: `Bearer ${token}`, // 使用 Bearer Token
          },
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          uni.showToast({ title: "取消报名成功", icon: "success" });
          this.registrations.splice(index, 1); // 从列表中移除已取消的报名记录
        } else {
          console.error("取消报名失败:", response);
          uni.showToast({ title: "取消报名失败", icon: "none" });
        }
      } catch (error) {
        console.error("取消报名请求出错:", error);
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
  background: linear-gradient(90deg, #4caf50, #388e3c); /* 修改为绿色渐变 */
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
  position: relative;
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
.cancel-button {
  position: absolute;
  right: 20rpx;
  top: 50%;
  transform: translateY(-50%);
  background-color: #f44336;
  color: white;
  font-size: 24rpx;
  padding: 10rpx 20rpx;
  border-radius: 5rpx;
  border: none;
}
</style>
