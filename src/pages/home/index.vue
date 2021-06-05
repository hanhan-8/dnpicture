<template>
  <view>
    <view class="home_tab">
      <view class="home_tab_title">
        <view class="title_inner">
          <uni-segmented-control
            :current="current"
            :values="items.map(v=>v.title)"
            @clickItem="onClickItem"
            styleType="text"
            activeColor="#d4237a"
          ></uni-segmented-control>
        </view>
        <view class="iconfont iconsearch"></view>
      </view>
      <view class="home_tab_content">
        <view v-if="current === 0">
          <home-recommend></home-recommend>
        </view>
        <view v-if="current === 1">
          <home-category></home-category>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import { homeCategory } from "./home-category";
import { homeRecommend } from "./home-recommend";
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  components: {
    homeCategory,
    homeRecommend,
    uniSegmentedControl
  },
  data() {
    return {
      items: [{ title: "推荐" }, { title: "分类" }],
      current: 0
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.home_tab {
  .home_tab_title {
    position: relative;
    .title_inner {
      padding: 0 50px;
    }
    .iconsearch {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 5px;
    }
  }
  .home_tab_content {
  }
}
</style>