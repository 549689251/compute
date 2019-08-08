<template>
  <div class="home-wapper">
    <view class="table">
      <view class="tr bg-w">
        <view class="th">月份</view>
        <view class="th">应该缴纳金额</view>
      </view>
      <div class="item-wapper" v-for="(item, index) in result" :key="index">
        <view class="td">第{{item.index}}月应缴税收</view>
        <view class="td">{{item.content}} 元</view>
      </div>
    </view>
  </div>
</template>

<script>
  import store from '../counter/store'

  export default {

    data() {
      return {
        result: []
      }
    },

    created() {

    },
    onLoad() {
      let index = 1;
      this.result=[];
      let results = store.state.handleResult;
      results.forEach(item => {
        let handleItem = {};
        handleItem.index = index++;
        handleItem.content = item;
        this.result.push(handleItem);
      });
    },
    methods: {}
  }
</script>

<style lang="less" scoped>

  .home-wapper {
    width: 100%;
    height: 100%;
    background: white;

    .table {
      border: 0px solid darkgray;
    }

    .tr {
      display: flex;
      width: 100%;
      justify-content: center;
      height: 1rem;
      width: 100%;
      align-items: center;
    }

    .td {
      width: 100%;
      height: 0.9rem;
      line-height: 1rem;
      justify-content: center;
      text-align: center;
      font-size: 0.28rem;
      font-weight: normal;
      font-stretch: normal;
      letter-spacing: 0rem;
      color: #2f3240;
    }

    .bg-w {
      background: snow;
    }

    .bg-g {
      background: #E6F3F9;
    }

    .th {
      width: 100%;
      justify-content: center;
      background: #3366FF;
      color: #fff;
      display: flex;
      height: 1rem;
      font-size: 16px;
      align-items: center;
    }

    .item-wapper {
      margin-top: .03rem;
      display: flex;

    }
  }
</style>
