<template>
  <view class="page">
    <view class="header">
      <text class="title">球友活动</text>
    </view>
    <view class="content">
      <view
        class="activity"
        v-for="(activity, index) in activities"
        :key="index"
      >
        <text class="activity-title">{{ activity.title }}</text>
        <text class="activity-date">时间: {{ activity.date }}</text>
        <text class="activity-location">地点: {{ activity.location }}</text>
        <button @click="joinActivity(index)">报名</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      activities: [
        {
          title: "木球友谊赛",
          date: "2025-04-15",
          location: "浙工大屏峰校区",
        },
        // ...other activities...
      ],
    };
  },
  methods: {
    joinActivity(index) {
      alert(`报名成功: ${this.activities[index].title}`);
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
  background: linear-gradient(90deg, #00bcd4, #0288d1);
  padding: 20rpx;
  color: white;
}
.title {
  font-size: 36rpx;
  font-weight: bold;
}
.content {
  margin-top: 20rpx;
}
.activity {
  background-color: #fff;
  margin-bottom: 20rpx;
  padding: 20rpx;
  border-radius: 10rpx;
  box-shadow: 0 2rpx 5rpx rgba(0, 0, 0, 0.1);
}
.activity-title {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
}
.activity-date,
.activity-location {
  font-size: 24rpx;
  color: #666;
}
</style>
