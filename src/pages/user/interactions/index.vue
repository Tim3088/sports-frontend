<template>
  <view class="page">
    <view class="header">
      <text class="title">我的互动记录</text>
    </view>
    <view class="filter-options">
      <button
        :class="{ active: filterType === 'all' }"
        @click="filterPosts('all')"
      >
        全部
      </button>
      <button
        :class="{ active: filterType === 'interacted' }"
        @click="filterPosts('interacted')"
      >
        互动过的
      </button>
      <button
        :class="{ active: filterType === 'mine' }"
        @click="filterPosts('mine')"
      >
        我发布的
      </button>
    </view>
    <view v-if="posts.length" class="posts">
      <view class="post" v-for="(post, index) in filteredPosts" :key="post.id">
        <text class="post-title">{{ post.title }}\n</text>
        <text class="post-content">{{ post.content }}\n</text>
        <text class="post-likes">点赞数: {{ post.likes }}\n</text>
        <text class="post-time">发布时间: {{ post.updatedAt }}</text>
        <view class="post-comments" v-if="post.comments.length">
          <text class="comments-title">评论:</text>
          <view
            class="comment"
            v-for="(comment, cIndex) in post.comments"
            :key="cIndex"
          >
            <text class="comment-author">{{ comment.nickName }}:</text>
            <text class="comment-content">{{ comment.content }}</text>
            <text class="comment-time">{{
              formatDate(comment.createdAt)
            }}</text>
          </view>
        </view>
        <view class="post-actions-right">
          <view
            class="action-item delete-action"
            v-if="post.isMine && post.nickName === currentUser"
            @click="deletePost(post.id, index)"
          >
            <image src="/static/icons/delete.png" class="action-icon-small" />
            <text class="delete-text">删除</text>
          </view>
        </view>
      </view>
    </view>
    <view v-else class="empty">
      <text>暂无互动记录</text>
    </view>
  </view>
</template>

<script>
export default {
  name: "InteractionsPage",
  data() {
    return {
      posts: [], // 存储用户的互动记录
      filterType: "all", // 筛选类型
      currentUser: "", // 当前用户
    };
  },
  computed: {
    filteredPosts() {
      if (this.filterType === "all") {
        return this.posts;
      } else if (this.filterType === "interacted") {
        return this.posts.filter((post) => post.nickName !== this.currentUser);
      } else if (this.filterType === "mine") {
        return this.posts.filter((post) => post.isMine);
      }
      return this.posts;
    },
  },
  onShow() {
    const token = uni.getStorageSync("user_token");
    if (!token) {
      uni.showToast({ title: "还未登录", icon: "none" });
      uni.switchTab({ url: "/pages/profile/index" }); // 返回到“我的”页面
      return;
    }
    this.fetchInteractions(); // 获取用户互动记录
  },
  methods: {
    async fetchInteractions() {
      try {
        const token = uni.getStorageSync("user_token");
        this.currentUser = uni.getStorageSync("user_nickname");
        const response = await uni.request({
          url: "https://sports.ziven.site/api/user/posts", // 获取用户发布的帖子列表
          method: "GET",
          header: {
            Authorization: `Bearer ${token}`, // 使用 Bearer Token
          },
        });
        if (response.statusCode === 200 && response.data.code === 200) {
          const formatDateTime = (dateTime) => {
            try {
              // 替换空格为 'T'，以兼容 iOS 的日期格式
              const formattedDateTime = dateTime.replace(" ", "T");
              const date = new Date(formattedDateTime);
              if (isNaN(date.getTime())) {
                throw new Error("Invalid date format");
              }
              const year = date.getFullYear();
              const month = String(date.getMonth() + 1).padStart(2, "0");
              const day = String(date.getDate()).padStart(2, "0");
              const hours = String(date.getHours()).padStart(2, "0");
              const minutes = String(date.getMinutes()).padStart(2, "0");
              return `${year}-${month}-${day} ${hours}:${minutes}`;
            } catch (error) {
              console.error("Error formatting date:", error);
              return "Invalid Date";
            }
          };

          this.posts = response.data.data.posts.map((post) => ({
            id: post.id,
            title: post.title,
            content: post.content,
            nickName: post.nickName, // 作者昵称
            likes: post.likes,
            comments: post.comments.map((comment) => ({
              nickName: comment.nickName,
              content: comment.content,
              createdAt: formatDateTime(comment.createdAt), // 评论时间
            })),
            updatedAt: formatDateTime(post.updatedAt), // 帖子发布时间
            isMine: post.nickName === this.currentUser, // 判断是否为当前用户发布
          }));
        } else {
          console.error("获取互动记录失败:", response);
          uni.showToast({ title: "获取互动记录失败", icon: "none" });
        }
      } catch (error) {
        console.error("获取互动记录请求出错:", error);
      }
    },
    async deletePost(postId, index) {
      try {
        const token = uni.getStorageSync("user_token");

        const response = await uni.request({
          url: `http://sports.ziven.site/api/user/posts/${postId}`, // 删除帖子接口地址
          method: "DELETE",
          header: {
            Authorization: `Bearer ${token}`, // 使用 Bearer Token
          },
        });

        if (response.statusCode === 200 && response.data.code === 200) {
          uni.showToast({ title: "删除成功", icon: "success" });
          this.posts.splice(index, 1); // 从列表中移除已删除的帖子
        } else {
          console.error("删除帖子失败:", response);
          uni.showToast({ title: "删除帖子失败", icon: "none" });
        }
      } catch (error) {
        console.error("删除帖子请求出错:", error);
        uni.showToast({ title: "网络错误", icon: "none" });
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(
        2,
        "0"
      )}-${String(date.getDate()).padStart(2, "0")} ${String(
        date.getHours()
      ).padStart(2, "0")}:${String(date.getMinutes()).padStart(2, "0")}`;
    },
    filterPosts(type) {
      this.filterType = type;
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
  background: linear-gradient(90deg, #4caf50, #388e3c);
  padding: 20rpx;
  color: white;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.filter-options {
  display: flex;
  justify-content: center;
  margin-bottom: 20rpx;
}
/* 美化筛选按钮样式 */
.filter-options button {
  background-color: #fff;
  border: 2rpx solid #ccc;
  padding: 10rpx 30rpx; /* 统一按钮内边距 */
  margin: 0 10rpx;
  border-radius: 50rpx; /* 统一圆角 */
  font-size: 28rpx; /* 统一字体大小 */
  color: #333;
  transition: all 0.3s ease; /* 添加过渡效果 */
}

.filter-options button.active {
  background-color: #4caf50; /* 选中状态背景色 */
  color: white; /* 选中状态字体颜色 */
  border-color: #4caf50; /* 选中状态边框颜色 */
}

.filter-options button:hover {
  box-shadow: 0 4rpx 8rpx rgba(0, 0, 0, 0.2); /* 鼠标悬停阴影效果 */
}
.posts {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}
.post {
  background-color: #fff;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}
.post-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
}
.post-content {
  font-size: 24rpx;
  color: #666;
}
.post-likes {
  font-size: 24rpx;
  color: #999;
  margin-top: 10rpx;
}
.post-time {
  font-size: 24rpx;
  color: #999;
  margin-top: 10rpx;
}
.post-comments {
  margin-top: 10rpx;
}
.comments-title {
  font-size: 24rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 5rpx;
}
.comment {
  margin-bottom: 5rpx;
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
}
.post-actions {
  margin-top: 10rpx;
  text-align: left;
}
.post-actions-right {
  margin-top: 10rpx;
  text-align: right;
}
.delete-text-large {
  font-size: 36rpx;
}
.empty {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  color: #999;
  font-size: 28rpx;
}
.action-icon-small {
  width: 25rpx;
  height: 25rpx;
}
.delete-text {
  font-size: 24rpx;
  vertical-align: middle;
}
</style>
