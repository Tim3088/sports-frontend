<template>
  <view class="page">
    <view class="header">
      <text class="title">{{ product.name }}</text>
    </view>
    <view class="product-details">
      <image :src="product.image" class="product-image" />
      <text class="product-price">价格: ￥{{ product.price }}</text>
      <text class="product-specs">规格: {{ product.specs }}</text>
      <text class="product-material">材质: {{ product.material }}</text>
      <text class="product-usage">使用方法: {{ product.usage }}</text>
    </view>
    <button class="buy-button" @click="buyProduct">立即购买</button>
    <view class="reviews">
      <text class="reviews-title">用户评价</text>
      <view class="review" v-for="(review, index) in reviews" :key="index">
        <text class="review-text">{{ review }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      product: {
        name: "高级球杆",
        price: 500,
        image: "/static/images/club1.jpg",
        specs: "长度: 90cm, 重量: 1.2kg",
        material: "碳纤维",
        usage: "适用于专业比赛",
      },
      reviews: ["非常好用！", "性价比高，推荐购买！"],
    };
  },
  methods: {
    buyProduct() {
      uni.showToast({ title: "购买功能待实现", icon: "none" });
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
.product-details {
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
  margin-bottom: 20rpx;
}
.product-image {
  width: 100%;
  height: 200rpx;
  margin-bottom: 10rpx;
}
.product-price,
.product-specs,
.product-material,
.product-usage {
  font-size: 24rpx;
  color: #333;
  margin-bottom: 10rpx;
}
.buy-button {
  width: 100%;
  height: 80rpx;
  background-color: #4caf50; /* 修改为绿色 */
  color: white;
  font-size: 28rpx;
  text-align: center;
  line-height: 80rpx;
  border-radius: 10rpx;
  margin-bottom: 20rpx;
}
.reviews {
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}
.reviews-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}
.review {
  font-size: 24rpx;
  color: #666;
  margin-bottom: 10rpx;
}
</style>
