<template>
  <div class="p3-container">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="global-title">风控反欺诈实时监控</div>
      <div class="right-time">
        <weather-com />
      </div>
    </div>

    <!-- 内容 -->
    <div class="frame-content">
      <div class="left-box">
        <div class="content-title"><span>风险交易金额趋势</span></div>
        <amount-trends-chart />
        <div class="content-title"><span>风险交易拦截金额类型</span></div>
        <amount-type-chart />
        <microfinance-chart />
      </div>
      <div class="center-box">
        <!-- 地图 -->
        <div class="strategy-box">
          <real-time-strategy title="19年累计调用策略" :num='amount' :blank="1"/>
          <real-time-strategy title="当日调用策略" num='4913' :blank="2"/>
        </div>
        <map-chart class="map-charts" />
        <pie-chart />
        <div class="map-panel"></div>
      </div>
      <div class="right-box">
        <div class="content-title"><span>实时调用决策变量239个  规则106条</span></div> 
        <rule-chart />
        <div class="content-title"><span>近24小时放款监测</span></div> 
        <loan-monitor-chart />
        <div class="content-title"><span>进件客户质量分布（人行评分）</span></div>
        <cust-distribute-chart />
        <div class="content-title"><span>Top5拒绝原因</span></div>
        <top5-refuse-chart />
      </div>
    </div>
  </div>
</template>

<script>

import amountTrendsChart from '@/components/three-screen/amountTrendsChart'
import amountTypeChart from '@/components/three-screen/amountTypeChart'
import realTimeStrategy from '@/components/three-screen/realTimeStrategy'
import MapChart from "@/components/three-screen/mapChart.vue"
import PieChart from "@/components/three-screen/pieChart.vue"
import LoanMonitorChart from "@/components/three-screen/loanMonitorChart.vue"
import CustDistributeChart from "@/components/three-screen/custDistributeChart.vue"
import Top5RefuseChart from "@/components/three-screen/top5RefuseChart.vue"
import WeatherCom from "@/components/weather.vue"
import MicrofinanceChart from "@/components/three-screen/microfinanceChart.vue"
import RuleChart from "@/components/three-screen/ruleChart.vue"
import moment from 'moment'
export default {
  data() {
    return {
      amount: 0
    }
  },
  mounted(){
    this.getAccRiskAll()
    this.numFun(200, 10000)
  },
  methods: {
    getAccRiskAll(){
      this.axios.get('/api/p3/accRiskAll',{
        params: {
          type: 'y'
        }
      })
      .then(function (response) {
          console.log(response);
      })
      .catch(function (error) {
          console.log(error);
      })
    },
    numFun(startNum,maxNum) {
      var that = this
      let numText = startNum;
      let golb; // 为了清除requestAnimationFrame
      function numSlideFun(){ // 数字动画
          numText+=55; // 速度的计算可以为小数 。数字越大，滚动越快
          if(numText >= maxNum){
              numText = maxNum;
              cancelAnimationFrame(golb);
          }else {
              golb = requestAnimationFrame(numSlideFun);
          }
        that.amount=numText
      }
       numSlideFun(); // 调用数字动画
    }
  },
  components: {
    amountTrendsChart,
    amountTypeChart,
    realTimeStrategy,
    PieChart,
    MapChart,
    LoanMonitorChart,
    CustDistributeChart,
    Top5RefuseChart,
    WeatherCom,
    MicrofinanceChart,
    RuleChart
  }
};
</script>

<style lang="less">
.p3-container {
  width: 4224px;
  height: 1536px;
  // width: 100%;
  // height: 100%;
  background: url("../../assets/images/p3/bg-three.png") no-repeat;
  background-size: 100% 100%;
  font-size: 28px;
  color: #fff;
  overflow: hidden;

  // 标题样式
  .title-frame {
    width: 100%;
    // height: 5%;
    height: 110px;
    background: url("../../assets/images/header.png") no-repeat;
    background-size: 100% 100%;
    display: flex;
    justify-content: flex-end;
    .global-title {
      width: 33%;
      text-align: center;
      font-size: 40px;
      font-weight: bold;
    }
    .right-time {
      width: 33%;
      text-align: right;
    }
    .time-span {
      display: inline-block;
      margin-left: 5%;
    }
  }

  // 内容样式
  .frame-content {
    display: flex;
    justify-content: space-between;
    // height: 80%;
    padding: 0 40px;
    .left-box,
    .right-box {
      width: 960px;
      height: 1440px;
      background: url("../../assets/images/p3/bg-cont.png") no-repeat;
    }
    .center-box {
      position: relative;
      width: 50%;
      height: 100%;
      .strategy-box{
        width: 100%;
      }
      .map-charts {
        width: 100%;
        height: 1160px;
      }
      .map-panel{
        position: absolute;
        right: 0;
        bottom: 50px;
        width: 140px;
        height: 140px;
        background: url('../../assets/images/p3/panel.png') no-repeat;
      }
    }
    .content-title {
      width: 880px;
      height: 60px;
      background: url("../../assets/images/p3/title-three.png") no-repeat;
      background-size: 100% 100%;
      margin: 16px auto 0;
      span{
        display: inline-block;
        font-size: 30px;
        padding: 17px 0 0 50px;

      }
    }
  }
}
</style>