<template>
  <view class="page">
    <view class="header">
      <text class="title">通知公告</text>
    </view>
    <view class="content">
      <view
        class="announcement"
        v-for="(item, index) in announcements"
        :key="index"
      >
        <swiper
          class="announcement-images"
          indicator-dots
          autoplay="false"
          interval="5000"
          circular
        >
          <swiper-item v-for="(image, imgIndex) in item.images" :key="imgIndex">
            <image
              :src="image"
              class="announcement-image"
              @click="previewImage(item.images, imgIndex)"
            />
          </swiper-item>
        </swiper>
        <text class="announcement-title">{{ item.title }}</text>
        <text class="announcement-date">{{ item.date }}</text>
        <text class="announcement-description">{{ item.description }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      announcements: [
        {
          title: "木球培训活动\n",
          date: "2025年3月30日\n",
          description: "当日于浙江工业大学屏峰校区举办的木球培训活动",
          images: [
            "https://img.ziven.site/blog/miniapp/announcements/3.30-1.jpg",
            "https://img.ziven.site/blog/miniapp/announcements/3.30-2.jpg",
            "https://img.ziven.site/blog/miniapp/announcements/3.30-3.jpg",
          ],
        },
        {
          title: "木球培训活动\n",
          date: "2025年3月29日\n",
          description: "当日于浙江工业大学屏峰校区举办的木球培训活动",
          images: [
            "https://img.ziven.site/blog/miniapp/announcements/3.29-1.jpg",
            "https://img.ziven.site/blog/miniapp/announcements/3.29-2.jpg",
            "https://img.ziven.site/blog/miniapp/announcements/3.29-3.jpg",
          ],
        },
      ],
    };
  },
  methods: {
    previewImage(images, currentIndex) {
      uni.previewImage({
        urls: images, // 图片数组
        current: images[currentIndex], // 当前预览的图片
      });
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
.content {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}
.announcement {
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}
.announcement-images {
  width: 100%;
  height: 300rpx; /* 调整高度以适配图片比例 */
  margin-bottom: 10rpx;
}
.announcement-image {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 确保图片按比例填充 */
  border-radius: 10rpx;
}
.announcement-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 5rpx;
}
.announcement-date {
  font-size: 24rpx;
  color: #999;
  margin-bottom: 10rpx;
}
.announcement-description {
  font-size: 24rpx;
  color: #666;
}
</style>
