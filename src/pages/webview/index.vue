<template>
  <view class="webview-container">
    <web-view :src="url" @error="onWebViewError"></web-view>
    <view v-if="error" class="error-message">
      <text>加载失败，请检查链接或网络。</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      url: "",
      error: false,
    };
  },
  onLoad(options) {
    if (options.url) {
      this.url = decodeURIComponent(options.url);
    }
  },
  methods: {
    onWebViewError() {
      this.error = true;
    },
  },
};
</script>

<style scoped>
.webview-container {
  width: 100%;
  height: 100%;
}
.error-message {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  color: red;
  font-size: 16px;
}
</style>
