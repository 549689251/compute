<template>
  <div @click="clickHandle" class="home-wapper">


    <div class="title-wapper">
      <span>全年累计计算个人所得税</span>
      <br>
      <label>输入相关数据计算出本年每月的税收详情，供大家参考</label>
    </div>

    <div>
      <div class="header-wapper">
        <div class="item_list_style"></div>
        <span>请输入你的工资</span>
      </div>
      <div class="divier-line"></div>
      <div class="content-wapper">
        <span>工资</span>
        <label>
          <input type="text" v-model="param.salary" placeholder="请输入你的工资" pattern="[0-9]"/>
        </label>
        <div class="unit">元</div>
      </div>
    </div>


    <div style="margin-top: 0.3rem">
      <div class="header-wapper">
        <div class="item_list_style"></div>
        <span>请输入社保和公积金的总额</span>
      </div>
      <div class="divier-line"></div>
      <div class="content-wapper">
        <span>总额</span>
        <label>
          <input type="text" v-model="param.social_insurance" placeholder="请输入社保和公积金的总额" pattern="[0-9]"/>
        </label>
        <div class="unit">元</div>
      </div>
    </div>

    <div style="margin-top: 0.3rem">
      <div class="header-wapper">
        <div class="item_list_style"></div>
        <span>请输入专项扣除总额（租房，房贷等）</span>
      </div>
      <div class="divier-line"></div>
      <div class="content-wapper">
        <span>总额</span>
        <label>
          <input type="text" v-model="param.social_plus" placeholder="请输入专项扣除总额" pattern="[0-9]"/>
        </label>
        <div class="unit">元</div>
      </div>
    </div>

    <div style="margin-top: 20px">
      <div class="tips-wapper">
        <span style="color: #fb883c;font-size: 12px;margin-left: .38rem">
                  注意：本次计算个税起点5000元
        </span>
      </div>
    </div>
    <div>
      <div class="submit-wapper">
        <button @click="submit">提交</button>
      </div>
    </div>
  </div>
</template>

<script>
  import store from '../counter/store'

  export default {
    data() {
      return {
        check: {
          numberOfHouses: false,
          remainingAssets: false
        },
        param: {
          salary: '',
          social_insurance: '',
          social_plus: '',
          entryCustDto: ''
        },
        computeResult: []
      }
    },

    components: {},

    methods: {
      submit() {
        store.state.handleResult = [];
        this.computeResult = [];
        //之前月份税收总金额
        let oldMonthSum = 0;
        for (let i = 1; i <= 12; i++) {
          // 10000 是工资********
          //500 是社保、公积金总额********
          //800是专项扣除，比如房贷 如果没有 请删除********
          //5000是税收起点********
          let adjective = (this.param.salary - this.param.social_insurance - this.param.social_plus - 5000) * i;
          let result = 0.00;
          if (adjective < 36000) {
            result = adjective * 0.03 - oldMonthSum;
            result = Math.floor(result * 100) / 100;
          } else if (adjective > 36000 && adjective < 144000) {
            result = adjective * 0.1 - 2520 - oldMonthSum;
            result = Math.floor(result * 100) / 100;
          } else if (adjective > 144000 && adjective < 300000) {
            result = adjective * 0.2 - 16920 - oldMonthSum;
            result = Math.floor(result * 100) / 100;
          } else if (adjective > 300000 && adjective < 420000) {
            result = adjective * 0.25 - 31920 - oldMonthSum;
            result = Math.floor(result * 100) / 100;
          } else if (adjective > 420000 && adjective < 660000) {
            result = adjective * 0.3 - 52920 - oldMonthSum;
            result = Math.floor(result * 100) / 100;
          } else if (adjective > 660000 && adjective < 960000) {
            result = adjective * 0.35 - 85920 - oldMonthSum;
            result = Math.floor(result * 100) / 100;
          } else if (adjective > 960000) {
            result = adjective * 0.45 - 181920 - oldMonthSum;
            result = Math.floor(result * 100) / 100;
          }
          this.computeResult.push(result);
          oldMonthSum += result;
        }
        store.state.handleResult = this.computeResult;

        console.log(store.state.handleResult.toString())
        const url = '../result/main';
        wx.navigateTo({url})
      }

    },

    created() {
    }
  }
</script>

<style lang="less">
  @import "./reset.css";

  .home-wapper {
    background: #FAFBFC;

    .title-wapper {
      width: 100%;
      height: 2rem;
      background: #4c84ff;
      display: flex;
      flex-direction: column;
      justify-content: center;

      & > span {
        font-family: PingFangSC-Semibold;
        font-size: 0.32rem;
        color: #ffffff;
        margin-left: .3rem;
      }

      & > label {
        font-family: PingFangSC-Semibold;
        font-size: 0.24rem;
        color: #8cf0ff;
        margin-left: .3rem;
      }
    }


    .header-wapper {
      background: white;
      height: 1rem;
      display: flex;
      flex-direction: row;
      align-items: center;

      .item_list_style {
        width: 0.06rem;
        height: 0.3rem;
        background-color: #4c84ff;
      }

      & > span {
        font-size: 0.3rem;
        margin-left: .24rem;
        color: #4c84ff;
        font-weight: bold;
      }
    }

    .divier-line {
      height: 0.02rem;
      width: 0.3rem;
      background: white;
    }

    .content-wapper {
      background: white;
      display: flex;
      height: 1rem;
      align-items: center;

      & > span {
        flex: 1;
        font-family: PingFangSC-Regular;
        font-size: 0.28rem;
        font-weight: normal;
        font-stretch: normal;
        letter-spacing: 0rem;
        color: #727790;
        margin-left: .3rem;
      }

      & > label {
        flex: 2;
        width: 0.67rem;
        height: 1rem;
        line-height: 1rem;
        font-family: PingFangSC-Regular;
        font-size: 0.28rem;
        font-weight: normal;
        font-stretch: normal;
        letter-spacing: 0rem;
        color: #2f3240;

        & > input {
          height: 1rem;
          text-align: right;
          margin-right: .16rem;
        }
      }

      .unit {
        font-family: PingFangSC-Regular;
        font-size: 0.28rem;
        font-weight: normal;
        font-stretch: normal;
        letter-spacing: 0rem;
        color: #727790;
        height: 1rem;
        text-align: center;
        display: block;
        padding-right: .32rem;
        line-height: 1rem;
      }
    }

    .submit-wapper {

      margin-top: 80px;

      & > button {
        width: 6.5rem;
        height: 0.9rem;
        background-color: #4c84ff;
        box-shadow: 0rem 0.08rem 0.2rem 0rem rgba(76, 132, 255, 0.3);
        border-radius: 0.04rem;
        color: white;
      }
    }
  }
</style>
