<template>
  <view class="page">
    <view class="header">
      <text class="title">木球器材器械</text>
    </view>
    <view class="categories">
      <view
        class="category"
        v-for="(category, index) in categories"
        :key="index"
        @click="navigateToCategory(category.path)"
      >
        <image :src="category.icon" class="category-icon" />
        <text class="category-name">{{ category.name }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      categories: [
        {
          name: "球杆",
          icon: "/static/icons/club.png",
          path: "/pages/user/equipment/club",
        },
        {
          name: "木球",
          icon: "/static/icons/ball.png",
          path: "/pages/user/equipment/ball",
        },
        {
          name: "场地配件",
          icon: "/static/icons/accessories.png",
          path: "/pages/user/equipment/accessories",
        },
        {
          name: "比赛专用装备",
          icon: "/static/icons/gear.png",
          path: "/pages/user/equipment/gear",
        },
      ],
    };
  },
  methods: {
    navigateToCategory(path) {
      uni.navigateTo({ url: path });
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
.categories {
  display: flex;
  flex-wrap: wrap;
  gap: 20rpx;
}
.category {
  width: 45%;
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
  text-align: center;
}
.category-icon {
  width: 80rpx;
  height: 80rpx;
  margin-bottom: 10rpx;
}
.category-name {
  font-size: 28rpx;
  color: #333;
}
</style>
