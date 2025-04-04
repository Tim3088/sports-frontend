<template>
  <view class="page">
    <view class="header">
      <text class="title">球杆</text>
    </view>
    <view class="products">
      <view
        class="product"
        v-for="(product, index) in products"
        :key="index"
        @click="navigateToProduct(product.id)"
      >
        <image :src="product.image" class="product-image" />
        <text class="product-name">{{ product.name }}</text>
        <text class="product-price">￥{{ product.price }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      products: [
        {
          id: 1,
          name: "高级球杆",
          price: 500,
          image: "/static/images/club1.jpg",
        },
        {
          id: 2,
          name: "专业球杆",
          price: 800,
          image: "/static/images/club2.jpg",
        },
      ],
    };
  },
  methods: {
    navigateToProduct(productId) {
      uni.navigateTo({ url: `/pages/user/equipment/product?id=${productId}` });
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
.products {
  display: flex;
  flex-wrap: wrap;
  gap: 20rpx;
}
.product {
  width: 45%;
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
  text-align: center;
}
.product-image {
  width: 100rpx;
  height: 100rpx;
  margin-bottom: 10rpx;
}
.product-name {
  font-size: 28rpx;
  color: #333;
}
.product-price {
  font-size: 24rpx;
  color: #4caf50; /* 修改为绿色 */
}
</style>
