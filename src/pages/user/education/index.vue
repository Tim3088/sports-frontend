<template>
  <view class="page">
    <!-- 页面标题 -->
    <view class="header">
      <text class="title">木球教育培训</text>
    </view>

    <!-- 培训项目分类 -->
    <view class="section">
      <text class="section-title">培训项目分类</text>
      <view class="categories">
        <view class="category" v-for="(item, index) in categories" :key="index">
          <text class="category-name">{{ item }}</text>
        </view>
      </view>
    </view>

    <!-- 培训课程详情 -->
    <view class="section">
      <text class="section-title">培训课程详情</text>
      <view class="course" v-for="(course, index) in courses" :key="index">
        <text class="course-title">{{ course.title }}</text>
        <view class="course-details">
          <text class="course-info">时间: {{ course.time }}\n</text>
          <text class="course-info">地点: {{ course.location }}\n</text>
          <text class="course-info">内容: {{ course.content }}\n</text>
          <text class="course-info">收费: {{ course.fee }}</text>
        </view>
        <button class="signup-button" @click="submitSignup(course.id)">
          立即报名
        </button>
      </view>
    </view>

    <!-- 师资团队介绍 -->
    <view class="section">
      <text class="section-title">师资团队</text>
      <view class="team-member" v-for="(member, index) in team" :key="index">
        <image :src="member.photo" class="member-photo" />
        <text class="member-name">{{ member.name }}</text>
        <text class="member-title">{{ member.title }}</text>
      </view>
    </view>

    <!-- 学员风采展示 -->
    <view class="section">
      <text class="section-title">学员风采</text>
      <view class="gallery">
        <image
          v-for="(image, index) in gallery"
          :key="index"
          :src="image"
          class="gallery-image"
        />
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      categories: ["初级培训", "中级培训", "高级培训", "教练员培训"],
      courses: [], // 初始化为空数组
      team: [], // 初始化为空数组
      gallery: [
        "/static/images/student1.jpg",
        "/static/images/student2.jpg",
        "/static/images/student3.jpg",
      ],
    };
  },
  methods: {
    async fetchCourses() {
      try {
        const response = await uni.request({
          url: `https://sports.ziven.site/api/education/courses`, // 确保使用 HTTP 或 HTTPS
          method: "GET",
        });

        if (response.statusCode === 200 && response.data.code === 200) {
          this.courses = response.data.data.courses; // 适配后端返回的嵌套结构
          console.log("课程数据获取成功:", this.courses);
        } else {
          console.error("获取课程失败:", response);
          uni.showToast({ title: "获取课程失败", icon: "none" });
        }
      } catch (error) {
        console.error("获取课程请求出错:", error);
        uni.showToast({ title: "网络错误", icon: "none" });
      }
    },
    async fetchTeam() {
      try {
        const response = await uni.request({
          url: `https://sports.ziven.site/api/education/team`, // 获取师资团队数据的接口地址
          method: "GET",
        });

        if (response.statusCode === 200 && response.data.code === 200) {
          this.team = response.data.data.team.map((member) => ({
            id: member.id,
            name: member.name,
            title: member.title,
            photo: member.img, // 将 img 字段映射为 photo
          }));
          console.log("师资团队数据获取成功:", this.team);
        } else {
          console.error("获取师资团队失败:", response);
          uni.showToast({ title: "获取师资团队失败", icon: "none" });
        }
      } catch (error) {
        console.error("获取师资团队请求出错:", error);
        uni.showToast({ title: "网络错误", icon: "none" });
      }
    },
    async submitSignup(courseId) {
      try {
        const token = uni.getStorageSync("user_token"); // 从本地存储获取 Bearer Token
        if (!token) {
          uni.showToast({ title: "请先登录", icon: "none" });
          return;
        }
        const response = await uni.request({
          url: "https://sports.ziven.site/api/education/signup", // 报名接口地址
          method: "POST",
          header: {
            Authorization: `Bearer ${token}`, // 使用 Bearer Token
            "Content-Type": "application/json",
          },
          data: {
            courseId: courseId, // 传递当前课程的 ID
          },
        });
        if (response.statusCode === 200) {
          uni.showToast({ title: "报名成功", icon: "success" });
        } else {
          console.error("报名失败:", response);
          uni.showToast({ title: "报名失败", icon: "none" });
        }
      } catch (error) {
        console.error("报名请求出错:", error);
        uni.showToast({ title: "报名失败", icon: "none" });
      }
    },
  },
  mounted() {
    this.fetchCourses(); // 页面加载时获取课程数据
    this.fetchTeam(); // 页面加载时获取师资团队数据
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
.section {
  margin-bottom: 30rpx;
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
.categories {
  display: flex;
  flex-wrap: wrap;
}
.category {
  background-color: #e0f7fa;
  padding: 10rpx 20rpx;
  border-radius: 20rpx;
  margin-right: 10rpx;
  margin-bottom: 10rpx;
}
.category-name {
  font-size: 28rpx;
  color: #00796b;
}
.course {
  margin-bottom: 20rpx;
}
.course-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx; /* 增加标题与详情之间的间距 */
}
.course-details {
  margin-left: 20rpx; /* 增加缩进以区分详情部分 */
}
.course-info {
  font-size: 24rpx;
  color: #666;
  margin-bottom: 10rpx; /* 确保每行内容有间距 */
}
.signup-button {
  width: 100%;
  height: 80rpx;
  background-color: #0288d1; /* 修改为主题蓝色 */
  color: white;
  font-size: 28rpx;
  text-align: center;
  line-height: 80rpx;
  border-radius: 10rpx;
}
.team-member {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
}
.member-photo {
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
  margin-right: 20rpx;
}
.member-name {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
}
.member-title {
  font-size: 24rpx;
  color: #666;
}
.gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 10rpx;
}
.gallery-image {
  width: 100rpx;
  height: 100rpx;
  border-radius: 10rpx;
}
</style>
