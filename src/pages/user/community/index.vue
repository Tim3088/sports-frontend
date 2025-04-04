<template>
  <view class="page">
    <view class="header">
      <text class="title">球友互动</text>
    </view>
    <view class="create-post-button">
      <button @click="navigateToCreatePost">发布新帖</button>
    </view>
    <view class="content">
      <view class="post" v-for="(post, index) in posts" :key="index">
        <text class="post-title">{{ post.title }}\n</text>
        <text class="post-author">作者: {{ post.author }}\n</text>
        <text class="post-time">发布时间: {{ post.updatedAt }}\n</text>
        <text class="post-content">{{ post.content }}</text>
        <view class="post-actions">
          <view class="action-item" @click="toggleLikePost(post.id)">
            <image
              :src="
                userLikes[post.id]
                  ? '/static/icons/like-active.png'
                  : '/static/icons/like.png'
              "
              class="action-icon"
            />
            <text>{{ post.likes }}</text>
          </view>
          <view class="action-item" @click="commentPost(post.id)">
            <image src="/static/icons/comment.png" class="action-icon" />
            <text>评论</text>
          </view>
        </view>
        <view class="comments" v-if="post.comments.length">
          <view
            v-for="(comment, cIndex) in post.comments"
            :key="cIndex"
            class="comment"
          >
            <text class="comment-author">{{ comment.author }}:</text>
            <text class="comment-content">{{ comment.content }}</text>
            <text class="comment-time">{{ comment.createdAt }}</text>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      posts: [], // 帖子列表
      userLikes: {}, // 记录用户对每个帖子的点赞状态
    };
  },
  methods: {
    async fetchPosts() {
      try {
        const token = uni.getStorageSync("user_token");
        if (!token) {
          uni.showToast({ title: "请先登录", icon: "none" });
          return;
        }
        const response = await uni.request({
          url: "https://sports.ziven.site/api/community/posts",
          method: "GET",
          header: {
            Authorization: `Bearer ${token}`,
          },
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          const formatDateTime = (dateTime) => {
            const date = new Date(dateTime);
            const year = date.getFullYear();
            const month = String(date.getMonth() + 1).padStart(2, "0");
            const day = String(date.getDate()).padStart(2, "0");
            const hours = String((date.getHours() + 16) % 24).padStart(2, "0"); // 小时加16并取模24
            const minutes = String(date.getMinutes()).padStart(2, "0");
            const seconds = String(date.getSeconds()).padStart(2, "0");
            return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
          };

          this.posts = response.data.data.posts.map((post) => ({
            id: post.id,
            title: post.title,
            content: post.content,
            author: post.nickName, // 映射 nickName 为 author
            likes: post.likes,
            updatedAt: formatDateTime(post.updatedAt), // 格式化帖子更新时间
            liked: post.liked, // 后端返回用户是否已点赞
            comments: post.comments.map((comment) => ({
              author: comment.nickName, // 映射 nickName 为评论作者
              content: comment.content,
              createdAt: formatDateTime(comment.createdAt), // 格式化评论时间
            })),
          }));
          this.userLikes = this.posts.reduce((acc, post) => {
            acc[post.id] = post.liked;
            return acc;
          }, {});
        } else {
          console.error("获取帖子失败:", response);
        }
      } catch (error) {
        console.error("获取帖子请求出错:", error);
      }
    },
    async toggleLikePost(postId) {
      const token = uni.getStorageSync("user_token");
      if (!token) {
        uni.showToast({ title: "请先登录", icon: "none" });
        return;
      }
      const liked = this.userLikes[postId];
      try {
        const response = await uni.request({
          url: `https://sports.ziven.site/api/community/posts/${postId}/like`,
          method: liked ? "DELETE" : "POST", // 根据当前状态决定请求类型
          header: {
            Authorization: `Bearer ${token}`,
          },
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          this.userLikes[postId] = !liked; // 更新本地状态
          const post = this.posts.find((p) => p.id === postId);
          if (post) {
            post.likes += liked ? -1 : 1; // 更新点赞数
          }
        } else {
          console.error("点赞操作失败:", response);
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
    navigateToCreatePost() {
      uni.navigateTo({
        url: "/pages/user/community/createPost", // 跳转到新界面
      });
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
.create-post-button {
  margin: 20rpx 0;
  text-align: center;
}
button {
  background-color: #4caf50;
  color: white;
  font-size: 28rpx;
  padding: 10rpx 20rpx;
  border-radius: 5rpx;
  border: none;
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
.post-time {
  font-size: 24rpx;
  color: #999;
  margin-bottom: 10rpx;
}
.post-content {
  font-size: 28rpx; /* 增大字体 */
  color: #000; /* 更黑 */
}
.post-actions {
  display: flex;
  gap: 20rpx;
  margin-top: 10rpx;
}
.action-item {
  display: flex;
  align-items: center;
  gap: 10rpx;
}
.action-icon {
  width: 30rpx;
  height: 30rpx;
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
.comment-time {
  font-size: 20rpx;
  color: #999;
  margin-left: 10rpx;
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
