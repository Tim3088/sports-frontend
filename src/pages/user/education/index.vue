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
      </view>
    </view>

    <!-- 在线报名入口 -->
    <view class="section">
      <text class="section-title">在线报名</text>
      <button class="signup-button" @click="navigateToSignup">立即报名</button>
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
      courses: [
        {
          title: "初级培训班",
          time: "2025-04-01 至 2025-04-15",
          location: "浙工大屏峰校区木球馆",
          content: "基础规则与技巧",
          fee: "￥500",
        },
        {
          title: "中级培训班",
          time: "2025-05-01 至 2025-05-15",
          location: "浙工大屏峰校区木球馆",
          content: "进阶战术与实战",
          fee: "￥800",
        },
        {
          title: "高级培训班",
          time: "2025-06-01 至 2025-06-15",
          location: "浙工大屏峰校区木球馆",
          content: "高级战术与团队协作",
          fee: "￥1200",
        },
        {
          title: "教练员培训班",
          time: "2025-07-01 至 2025-07-15",
          location: "浙工大屏峰校区木球馆",
          content: "教学方法与赛事组织",
          fee: "￥1500",
        },
      ],
      team: [
        {
          name: "张教练",
          title: "高级教练",
          photo: "/static/images/coach.jpg",
        },
        {
          name: "李教练",
          title: "国家级教练",
          photo: "/static/images/coach.jpg",
        },
      ],
      gallery: [
        "/static/images/student1.jpg",
        "/static/images/student2.jpg",
        "/static/images/student3.jpg",
      ],
    };
  },
  methods: {
    navigateToSignup() {
      uni.navigateTo({ url: "/pages/signup/index" });
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
