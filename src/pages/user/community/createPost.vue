<template>
  <view class="page">
    <view class="header">
      <text class="title">发布新帖</text>
    </view>
    <view class="new-post">
      <input v-model="newPostTitle" placeholder="请输入标题" />
      <textarea v-model="newPostContent" placeholder="写点什么吧..." />
      <button @click="createPost">发布</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      newPostTitle: "", // 新帖标题
      newPostContent: "", // 新帖内容
    };
  },
  methods: {
    async createPost() {
      const userName = uni.getStorageSync("user_nickname");
      const token = uni.getStorageSync("user_token"); // 从本地存储获取 Token
      if (!token) {
        uni.showToast({ title: "请先登录", icon: "none" });
        return;
      }
      if (!userName) {
        uni.showToast({ title: "请先设置用户名", icon: "none" });
        return;
      }
      if (!this.newPostTitle.trim()) {
        uni.showToast({ title: "标题不能为空", icon: "none" });
        return;
      }
      if (!this.newPostContent.trim()) {
        uni.showToast({ title: "内容不能为空", icon: "none" });
        return;
      }
      try {
        const response = await uni.request({
          url: "https://sports.ziven.site/api/community/posts",
          method: "POST",
          header: {
            Authorization: `Bearer ${token}`, // 添加 Authorization 头部
            "Content-Type": "application/json", // 设置 Content-Type
          },
          data: {
            title: this.newPostTitle,
            content: this.newPostContent,
            author: userName,
          },
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          uni.showToast({ title: "发布成功", icon: "success" });
          // 通知上一个页面刷新帖子列表
          const pages = getCurrentPages();
          const previousPage = pages[pages.length - 2];
          if (previousPage && previousPage.onShow) {
            previousPage.onShow(); // 调用上一个页面的 onShow 方法
          }
          uni.navigateBack();
        } else {
          console.error("发帖失败:", response);
        }
      } catch (error) {
        console.error("发帖请求出错:", error);
      }
    },
  },
};
</script>

<style scoped>
.page {
  background-color: #f5f5f5;
  padding: 20rpx;
}
.header {
  background: linear-gradient(90deg, #4caf50, #388e3c);
  color: white;
  padding: 20rpx;
  text-align: center;
  border-radius: 10rpx;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.new-post {
  margin-top: 20rpx;
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
textarea {
  width: 100%;
  height: 100rpx;
  margin-bottom: 10rpx;
  padding: 10rpx;
  border: 1rpx solid #ddd;
  border-radius: 5rpx;
}
button {
  background-color: #4caf50;
  color: white;
  font-size: 28rpx;
  padding: 10rpx 20rpx;
  border-radius: 5rpx;
  border: none;
}
.new-post input {
  width: 100%;
  height: 60rpx;
  margin-bottom: 10rpx;
  padding: 10rpx;
  border: 1rpx solid #ddd;
  border-radius: 5rpx;
  font-size: 28rpx;
}
</style>
