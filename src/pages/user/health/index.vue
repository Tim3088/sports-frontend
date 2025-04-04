<template>
  <view class="page">
    <!-- 页面标题 -->
    <view class="header">
      <text class="title">木球健康旅居</text>
    </view>

    <!-- 推荐旅居目的地 -->
    <view class="section">
      <text class="section-title">推荐旅居目的地</text>
      <view
        class="destination"
        v-for="(item, index) in destinations"
        :key="index"
      >
        <image :src="item.image" class="destination-image" />
        <text class="destination-name">{{ item.name }}</text>
        <text class="destination-description">{{ item.description }}</text>
      </view>
    </view>

    <!-- 旅居套餐详情 -->
    <view class="section">
      <text class="section-title">旅居套餐详情</text>
      <view class="package" v-for="(pkg, index) in packages" :key="index">
        <text class="package-title">{{ pkg.title }}</text>
        <text class="package-details">{{ pkg.details }}</text>
        <button class="book-button" @click="bookPackage(pkg.id)">
          在线预订
        </button>
      </view>
    </view>

    <!-- 用户体验分享 -->
    <view class="section">
      <text class="section-title">用户体验分享</text>
      <view class="experience" v-for="(exp, index) in experiences" :key="index">
        <text class="experience-text">{{ exp.text }}</text>
        <image v-if="exp.image" :src="exp.image" class="experience-image" />
      </view>
    </view>

    <!-- 健康养生专题文章 -->
    <view class="section">
      <text class="section-title">健康养生专题文章</text>
      <view class="article" v-for="(article, index) in articles" :key="index">
        <text class="article-title">{{ article.title }}</text>
        <text class="article-content">{{ article.content }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      destinations: [
        {
          name: "生态木球基地\n",
          description: "享受自然与木球的完美结合。",
          image: "/static/images/destination1.jpg",
        },
        {
          name: "休闲度假村\n",
          description: "放松身心的理想之地。",
          image: "/static/images/destination2.jpg",
        },
        {
          name: "康养酒店\n",
          description: "健康与舒适的完美体验。",
          image: "/static/images/destination3.jpg",
        },
      ],
      packages: [
        {
          id: 1,
          title: "生态木球基地套餐\n",
          details: "包含住宿、餐饮、木球活动等。",
        },
        {
          id: 2,
          title: "休闲度假村套餐\n",
          details: "包含豪华住宿、健康餐饮等。",
        },
      ],
      experiences: [
        {
          text: "一次难忘的旅居体验！",
          image: "/static/images/experience1.jpg",
        },
        {
          text: "木球活动非常有趣！",
          image: "/static/images/experience2.jpg",
        },
      ],
      articles: [
        {
          title: "健康养生之道\n",
          content: "如何通过木球运动保持健康。",
        },
        {
          title: "旅居生活的益处\n",
          content: "放松身心，享受自然。",
        },
      ],
    };
  },
  methods: {
    bookPackage(packageId) {
      uni.navigateTo({ url: `/pages/booking/index?packageId=${packageId}` });
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
.destination,
.package,
.experience,
.article {
  margin-bottom: 20rpx;
}
.destination-image,
.experience-image {
  width: 100%;
  height: 200rpx;
  border-radius: 10rpx;
}
.destination-name,
.package-title,
.article-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
}
.destination-description,
.package-details,
.article-content {
  font-size: 24rpx;
  color: #666;
}
.book-button {
  width: 100%;
  height: 80rpx;
  background-color: #4caf50; /* 修改为绿色 */
  color: white;
  font-size: 28rpx;
  text-align: center;
  line-height: 80rpx;
  border-radius: 10rpx;
}
</style>
