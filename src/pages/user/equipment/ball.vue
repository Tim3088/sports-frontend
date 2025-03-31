<template>
  <view class="page">
    <view class="header">
      <text class="title">木球</text>
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
          name: "标准木球",
          price: 100,
          image: "/static/images/ball1.jpg",
        },
        {
          id: 2,
          name: "高级木球",
          price: 200,
          image: "/static/images/ball2.jpg",
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
  background: linear-gradient(90deg, #00bcd4, #0288d1);
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
  color: #0288d1;
}
</style>
