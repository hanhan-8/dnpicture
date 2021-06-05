<template>
  <scroll-view
    @scrolltolower="handToLower"
    class="recommend_view"
    scroll-y
    v-if="recommends.length>0"
  >
    <!-- 推荐 -->
    <view class="recommends_wrap">
      <view class="recommend_item" v-for="(item,index) in recommends" :key="index">
        <image mode="widthFix" :src="item.thumb" />
      </view>
    </view>
    <!-- 月份 -->
    <view class="months_wrap">
      <view class="months_title">
        <view class="months_title_info">
          <view class="months_info">
            <text>{{months.DD}}/</text>
            {{months.MM}}月
          </view>
          <view class="months_text">{{months.title}}</view>
        </view>
        <view class="months_title_more">更多</view>
      </view>
      <view class="months_content">
        <view class="months_item" v-for="(item,index) in months.items" :key="index">
          <image mode="aspectFill" :src="item.thumb+item.rule.replace('$<Height>',360)" />
        </view>
      </view>
    </view>
    <!-- 热门 -->
    <view class="hots_wrap">
      <view class="hots_title">
        <text>热门</text>
      </view>
      <view class="hots_content">
        <view class="hots_item" v-for="(item,index) in hots" :key="index">
          <image mode="hightFix" :src="item.thumb" />
        </view>
      </view>
    </view>
  </scroll-view>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      recommends: [],
      months: {},
      hots: [],
      params: {
        limit: 30,
        order: "hot",
        skip: 0
      }
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url: "http://service.picasso.adesk.com/v3/homepage/vertical",
        data: this.params
      }).then(result => {
        // console.log(result);

        if (this.recommends.length === 0) {
          this.recommends = result.res.homepage[1].items;
          this.months = result.res.homepage[2];
          this.months.MM = moment(this.months.atime).format("MM");
          this.months.DD = moment(this.months.atime).format("DD");
        }

        this.hots = [...this.hots, ...result.res.vertical];
      });
    },
    handToLower() {
      this.params.skip += this.params.limit;
      this.getList();
    }
  }
};
</script>

<style lang="scss" scoped>
.recommend_view {
  height: calc(100vh - 36px);
}
.recommends_wrap {
  display: flex;
  flex-wrap: wrap;
  .recommend_item {
    width: 50%;
    border: 5rpx solid #fff;
  }
}

.months_wrap {
  .months_title {
    display: flex;
    justify-content: space-between;
    padding: 20rpx;
    .months_title_info {
      display: flex;
      color: $color;
      font-weight: 600;
      font-size: 30rpx;
      .months_info {
        text {
          font-size: 36rpx;
        }
      }
      .months_text {
        font-size: 34rpx;
        color: #666;
        margin-left: 30rpx;
      }
    }
    .months_title_more {
      color: $color;
      font-size: 24rpx;
    }
  }

  .months_content {
    display: flex;
    flex-wrap: wrap;
    .months_item {
      width: 33.33%;
      border: 5rpx solid #fff;
    }
  }
}

.hots_wrap {
  .hots_title {
    color: $color;
    font-weight: 600;
    font-size: 36rpx;
    margin-left: 10rpx;
  }
  .hots_content {
    display: flex;
    flex-wrap: wrap;
    .hots_item {
      width: 33.33%;
      border: 5rpx solid #fff;
      image {
      }
    }
  }
}
</style>