<template>
  <view class="page">
    <view class="header">
      <text class="title">球友互动</text>
    </view>
    <view class="content">
      <view class="post" v-for="(post, index) in posts" :key="index">
        <text class="post-title">{{ post.title }}</text>
        <text class="post-author">作者: {{ post.author }}</text>
        <text class="post-content">{{ post.content }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      posts: [
        {
          title: "木球技巧分享",
          author: "球友A",
          content: "如何提升击球精准度？",
        },
        {
          title: "木球赛事讨论",
          author: "球友B",
          content: "近期赛事的精彩回顾。",
        },
        {
          title: "木球装备推荐",
          author: "球友C",
          content: "哪款球杆性价比最高？",
        },
      ],
    };
  },
};
</script>

<style scoped>
.page {
  background-color: #f5f5f5;
  padding: 20rpx;
}
.header {
  background: linear-gradient(90deg, #00bcd4, #0288d1);
  color: white;
  padding: 20rpx;
  text-align: center;
  border-radius: 10rpx;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.content {
  margin-top: 20rpx;
}
.post {
  background-color: #fff;
  margin-bottom: 20rpx;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
.post-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}
.post-author {
  font-size: 24rpx;
  color: #666;
  margin-bottom: 10rpx;
}
.post-content {
  font-size: 24rpx;
  color: #333;
}
</style>
