<template>
  <view class="page">
    <view class="header">
      <text class="title">球友互动</text>
    </view>
    <view class="content">
      <view class="post" v-for="(post, index) in posts" :key="index">
        <text class="post-title">{{ post.title }}</text>
        <text class="post-author">作者: {{ post.author }}\n</text>
        <text class="post-content">{{ post.content }}</text>
        <view class="post-actions">
          <button @click="likePost(post.id)">点赞 ({{ post.likes }})</button>
          <button @click="commentPost(post.id)">评论</button>
        </view>
        <view class="comments" v-if="post.comments.length">
          <view
            v-for="(comment, cIndex) in post.comments"
            :key="cIndex"
            class="comment"
          >
            <text class="comment-author">{{ comment.author }}:</text>
            <text class="comment-content">{{ comment.content }}</text>
          </view>
        </view>
      </view>
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
      posts: [], // 帖子列表
      newPostTitle: "", // 新帖标题
      newPostContent: "", // 新帖内容
    };
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await uni.request({
          url: "https://sports.ziven.site/api/community/posts",
          method: "GET",
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          this.posts = response.data.data.posts.map((post) => ({
            id: post.id,
            title: post.title,
            content: post.content,
            author: post.nickName, // 映射 nickName 为 author
            likes: post.likes,
            updatedAt: post.updatedAt,
            comments: post.comments.map((comment) => ({
              author: comment.nickName, // 映射 nickName 为评论作者
              content: comment.content,
              createdAt: comment.createdAt,
            })), // 映射评论数据
          }));
        } else {
          console.error("获取帖子失败:", response);
        }
      } catch (error) {
        console.error("获取帖子请求出错:", error);
      }
    },
    async likePost(postId) {
      try {
        const token = uni.getStorageSync("user_token"); // 从本地存储获取 Token
        if (!token) {
          uni.showToast({ title: "请先登录", icon: "none" });
          return;
        }
        const response = await uni.request({
          url: `https://sports.ziven.site/api/community/posts/${postId}/like`,
          method: "POST",
          header: {
            Authorization: `Bearer ${token}`, // 添加 Authorization 头部
          },
        });
        if (response.statusCode === 200) {
          if (response.data.code === 200) {
            this.fetchPosts(); // 重新获取帖子列表
          } else if (response.data.code === 4000) {
            uni.showToast({ title: "帖子不存在", icon: "none" });
          } else {
            console.error("点赞失败:", response);
          }
        } else {
          console.error("点赞失败:", response);
        }
      } catch (error) {
        console.error("点赞请求出错:", error);
      }
    },
    async commentPost(postId) {
      const userName = uni.getStorageSync("user_nickname");
      const token = uni.getStorageSync("user_token"); // 从本地存储获取 Token
      if (!userName) {
        uni.showToast({ title: "请先设置用户名", icon: "none" });
        return;
      }
      if (!token) {
        uni.showToast({ title: "请先登录", icon: "none" });
        return;
      }
      uni.showModal({
        title: "发表评论",
        placeholderText: "请输入评论内容",
        editable: true,
        success: async (res) => {
          if (res.confirm && res.content) {
            try {
              const response = await uni.request({
                url: `https://sports.ziven.site/api/community/posts/${postId}/comment`,
                method: "POST",
                header: {
                  Authorization: `Bearer ${token}`, // 添加 Authorization 头部
                  "Content-Type": "application/json", // 设置 Content-Type
                },
                data: { content: res.content, author: userName },
              });
              if (response.statusCode === 200) {
                if (response.data.code === 200) {
                  this.fetchPosts(); // 重新获取帖子列表
                } else if (response.data.code === 4000) {
                  uni.showToast({ title: "帖子不存在", icon: "none" });
                } else {
                  console.error("评论失败:", response);
                }
              } else {
                console.error("评论失败:", response);
              }
            } catch (error) {
              console.error("评论请求出错:", error);
            }
          }
        },
      });
    },
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
          data: { title: this.newPostTitle, content: this.newPostContent, author: userName },
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          this.newPostTitle = ""; // 清空标题输入框
          this.newPostContent = ""; // 清空内容输入框
          this.fetchPosts(); // 重新获取帖子列表
        } else {
          console.error("发帖失败:", response);
        }
      } catch (error) {
        console.error("发帖请求出错:", error);
      }
    },
  },
  mounted() {
    this.fetchPosts(); // 页面加载时获取帖子列表
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
  font-size: 32rpx; /* 增大字体 */
  font-weight: bold;
  color: #000; /* 更黑 */
}
.post-author {
  font-size: 24rpx;
  color: #666;
  margin-bottom: 10rpx;
}
.post-content {
  font-size: 28rpx; /* 增大字体 */
  color: #000; /* 更黑 */
}
.post-actions {
  display: flex;
  gap: 10rpx;
  margin-top: 10rpx;
}
.comments {
  margin-top: 10rpx;
  padding-left: 20rpx;
  border-left: 2rpx solid #ddd;
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
.comment {
  margin-top: 5rpx;
  padding-left: 20rpx;
  display: flex;
  gap: 5rpx;
}
.comment-author {
  font-size: 24rpx;
  font-weight: bold;
  color: #333;
}
.comment-content {
  font-size: 24rpx;
  color: #666;
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
