<template>
  <view>
    <my-login v-if="!token"></my-login>
    <my-userinfo v-else></my-userinfo>
    <!-- <my-userinfo></my-userinfo> -->
  </view>
</template>

<script>
import badgeMix from '@/mixins/tabbar-badge.js'
// 1. 从 vuex 中按需导入 mapState 辅助函数
import { mapState } from 'vuex'
export default {
  mixins: [badgeMix],
  data() {
    return {}
  },
  computed: {
    // 2. 从 m_user 模块中导入需要的 token 字符串
    ...mapState('m_user', ['token']),
  },
}
</script>

<style lang="scss" scoped>
page,
.my-container {
  height: 100%;
}
</style>
