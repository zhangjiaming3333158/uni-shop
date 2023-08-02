<template>
  <view>
    <!-- 使用自定义的搜索组件 -->
    <view class="search-box">
      <my-search @click="gotoSearch"></my-search>
    </view>
    <!-- 使用自定义的分类组件 -->
    <view class="scroll-view-container">
      <!-- 左侧的滚动视图区域 -->
      <scroll-view class="left-scroll-view" scroll-y :style="{height: wh + 'px'}">
        <!-- <view class="left-scroll-view-item active">xxx</view> -->
        <view :class="['left-scroll-view-item', i === activeIndex ? 'active' : '']" v-for="(item,i) in cateList" :key="i" @click="changeActive(i)">{{ item.cat_name }}</view>
      </scroll-view>
      <!-- 右侧的滚动视图区域 -->
      <scroll-view class="right-scroll-view" scroll-y :style="{height: wh + 'px'}" :scroll-top="scrollTop">
        <!-- <view class="left-scroll-view-item active">zzz</view> -->
        <view class="cate-lv2" v-for="(item2, i2) in cateList2" :key="i2">
          <view class="cate-lv2-title">/ {{item2.cat_name}} /</view>
          <!-- 动态渲染三级分类的列表数据 -->
          <view class="cate-lv3-list">
            <!-- 三级分类 Item 项 -->
            <view class="cate-lv3-item" v-for="(item3, i3) in item2.children" :key="i3" @click="gotoGoodsList(item3)">
              <!-- 图片 -->
              <image :src="item3.cat_icon"></image>
              <!-- 文本 -->
              <text>{{item3.cat_name}}</text>
            </view>
          </view>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      wh: '', //屏幕高度
      cateList: [], //分类数据
      activeIndex: 0, //激活项索引
      cateList2: [], //二级分类数据
      cateList3: [], //三级分类数据
      scrollTop: 0, //滚动条距离顶部的距离
    }
  },
  onLoad() {
    //获取屏幕高度
    const info = uni.getSystemInfoSync()
    //获取屏幕高度-搜索栏高度
    this.wh = info.windowHeight - 50
    //获取分类数据
    this.getCateList()
  },
  methods: {
    //获取分类数据
    async getCateList() {
      const { data: res } = await uni.$http.get('/api/public/v1/categories')
      // console.log(res)
      if (res.meta.status !== 200)
        return uni.$showMsg({
          title: '获取分类数据失败',
        })
      this.cateList = res.message
      // 为二级分类赋值
      this.cateList2 = res.message[0].children
      // console.log(this.cateList)
    },
    //点击左侧分类项，激活当前项
    changeActive(i) {
      this.activeIndex = i
      this.cateList2 = this.cateList[i].children
      // 可以简化为如下的代码：
      this.scrollTop = this.scrollTop ? 0 : 1
    },
    gotoGoodsList(item3) {
      uni.navigateTo({
        url: '/subpkg/goods_list/goods_list?cid=' + item3.cat_id,
      })
    },
    gotoSearch() {
      uni.navigateTo({
        url: '/subpkg/search/search',
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.scroll-view-container {
  display: flex;
  flex-direction: row;

  .left-scroll-view {
    width: 25%;
    background-color: #ccc;

    .left-scroll-view-item {
      height: 100rpx;
      line-height: 100rpx;
      text-align: center;
      font-size: 30rpx;

      // 激活项的样式
      &.active {
        background-color: #ffffff;
        position: relative;

        // 渲染激活项左侧的红色指示边线
        &::before {
          content: ' ';
          display: block;
          width: 3px;
          height: 30px;
          background-color: #c00000;
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
        }
      }
    }
  }
  .right-scroll-view {
    width: 75%;
    background-color: #ffffff;
    .cate-lv2 {
      padding: 20rpx;
      .cate-lv2-title {
        font-size: 12px;
        font-weight: bold;
        text-align: center;
        padding: 15px 0;
      }
      .cate-lv3-list {
        display: flex;
        flex-wrap: wrap;
        .cate-lv3-item {
          width: 29%;
          padding: 10rpx;
          display: flex;
          flex-direction: column;
          align-items: center;
          image {
            width: 60px;
            height: 60px;
          }

          text {
            font-size: 12px;
          }
        }
      }
    }
  }
}
.search-box {
  // 设置定位效果为“吸顶”
  position: sticky;
  // 吸顶的“位置”
  top: 0;
  // 提高层级，防止被轮播图覆盖
  z-index: 999;
}
</style>