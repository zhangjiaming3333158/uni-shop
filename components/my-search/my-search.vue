<template>
  <view class="my-search-container" @click="handler">
    <!-- 使用 view 组件模拟 input 输入框的样式 -->
    <view class="my-search-box">
      <uni-icons type="search" size="17"></uni-icons>
      <text class="placeholder">搜索</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    handler() {
      // 跳转到搜索页面
      // uni.navigateTo({
      // 	url: '/pages/search/search',
      // })
      this.$emit('click') //触发父组件的事件
    },
  },
}
</script>

<style lang="scss" scoped>
.my-search-container {
  height: 50px;
  background-color: #c00000;
  padding: 0 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  .my-search-box {
    height: 36px;
    width: 100%;
    background-color: #f5f5f5;
    border-radius: 15px;
    display: flex;
    align-items: center;
    justify-content: center;
    uni-icons {
      margin-right: 5px;
      color: #999;
    }
    text {
      font-size: 14px;
      color: #999;
    }
  }
}
</style>
