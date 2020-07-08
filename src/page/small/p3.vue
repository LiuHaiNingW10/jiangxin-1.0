<template>
  <div class="p3-container">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="global-title">智能风控</div>
      <div class="right-time">
        <weather-com />
      </div>
    </div>

    <!-- 内容 -->
    <div class="frame-content">
      <div class="left-box">
        <div class="content-title">
          <span>风险交易金额趋势</span>
        </div>
        <amount-trends-chart />
        <div class="content-title">
          <span>风险交易拦截金额类型</span>
        </div>
        <amount-type-chart />
        <microfinance-chart />
      </div>
      <div class="center-box">
        <!-- 地图 -->
        <div class="strategy-box">
          <real-time-strategy v-if="showUses" title="当日累计拦截金额（元）" :num="usesTime.year" :blank="1" />
          <real-time-strategy v-if="showUses" title="当日累计拦截笔数（笔）" :num="usesTime.day" :blank="2" />
        </div>
        <map-chart class="map-charts" />
        <div class="map-panel"></div>
      </div>
      <div class="right-box">
        <div class="content-title">
          <span>实时反欺诈模型决策占比</span>
        </div>
        <div class="decisionRate">
          <decisionRate-chart :ids="idsDecision"/>
          <decisionRateB-chart :ids="idsDecisionB"/>
        </div>
        <div class="content-title">
          <span>实时加强验证</span>
        </div>
        <verification-chart :tableDatas="tableDataV"/>
        <div class="content-title">
          <span>进件客户质量分布（人行评分）</span>
        </div>
        <RiskPortraitChart :ids="idRisk" :tableDatas="tableDataR"/>
        <RiskPortraitChart :ids="idRiskM" :tableDatas="tableDataRM"/>
        <RiskPortraitChart :ids="idRiskR" :tableDatas="tableDataRR"/>
        <!-- <div class="content-title">
          <span>Top5拒绝原因</span>
        </div>
        <top5-refuse-chart /> -->
      </div>
    </div>
  </div>
</template>

<script>
import amountTrendsChart from "@/components/three-screen/amountTrendsChart";
import amountTypeChart from "@/components/three-screen/amountTypeChart";
import realTimeStrategy from "@/components/three-screen/realTimeStrategy";
import MapChart from "@/components/three-screen/mapChart.vue";
import PieChart from "@/components/three-screen/pieChart.vue";
import LoanMonitorChart from "@/components/three-screen/loanMonitorChart.vue";
import CustDistributeChart from "@/components/three-screen/custDistributeChart.vue";
import Top5RefuseChart from "@/components/three-screen/top5RefuseChart.vue";
import WeatherCom from "@/components/weather.vue";
import MicrofinanceChart from "@/components/three-screen/microfinanceChart.vue";
import RuleChart from "@/components/three-screen/ruleChart.vue";
import decisionRateChart from "@/components/three-screen/decisionRateChart.vue";
import decisionRateBChart from "@/components/three-screen/decisionRateBChart.vue";
import verificationChart from "@/components/three-screen/verificationChart.vue";
import * as base64 from "@/assets/base64.js"
import RiskPortraitChart from "@/components/p4/line-toRight.vue"
import moment from "moment";
export default {
  data() {
    return {
      timer: null,
      amount: 0,
      showUses: false,
      usesTime: {
        year: 0,
        day: 0
      },
      variableNum: 0,
      ruleNum: 0,
      idsDecision:{
        id:'echarts01',
        style: 'width:44%'
      },
      idsDecisionB:{
        id:'echarts02',
        style: 'width:55%'
      },
      tableDataV: [
        {
          title: '生物识别',
          img: base64.confirm_d.value,
          times: '234',
        },
        {
          title: '人工外呼',
          img: base64.confirm_c.value,
          times: '23',
        },
        {
          title: '短信加验',
          img: base64.confirm_b.value,
          times: '45',
        },
        {
          title: '密码加验',
          img: base64.confirm_d.value,
          times: '345',
        },
      ],
      tableDataR: [
        {
          area: '南京', percent: 0.4
        },
        {
          area: '福州', percent: 0.3
        },
        {
          area: '杭州', percent: 0.2
        },
        {
          area: '贵阳', percent: 0.08
        },
        {
          area: '厦门', percent: 0.02
        },
      ],
      tableDataRM: [
        {
          area: '华为荣耀', percent: 0.4
        },
        {
          area: '华为mate', percent: 0.3
        },
        {
          area: '小米米3', percent: 0.2
        },
        {
          area: 'Oppo11', percent: 0.08
        },
        {
          area: 'Apple8 ', percent: 0.02
        },
      ],
      tableDataRR: [
        {
          area: '刷单', percent: 0.4
        },
        {
          area: '卡片被盗', percent: 0.3
        },
        {
          area: '账户接管', percent: 0.2
        },
        {
          area: '电信诈骗', percent: 0.08
        },
        {
          area: '羊毛党', percent: 0.02
        },
      ],
      idRisk: {
        id: 'echarts03',
        style: "height: 15%",
        title: '实时欺诈风险画像'
      },
      idRiskM: {
        id: 'echarts04',
        style: "height: 15%",
        title: '风险设备占比'
      },
      idRiskR: {
        id: 'echarts05',
        style: "height: 15%",
        title: '风险类型占比'
      },
    };
  },
  mounted() {
    // this.getAccRiskAll();
    this.timer = setInterval(() => {
      setTimeout(this.getAccRiskAll, 0);
    }, 3000);
    this.getVariable();
  },
  methods: {
    getAccRiskAll() {
      this.axios
        .all([
          this.axios.get("/api/p3/accRiskAll", {
            params: {
              type: "y"
            }
          }),
          this.axios.get("/api/p3/accRiskAll", {
            params: {
              type: "h"
            }
          })
        ])
        .then(
          this.axios.spread((...obj) => {
            // this.usesTime = {
            //   year: obj[0].data.data,
            //   day: obj[1].data.data
            // }
            this.numFun(this.usesTime.year, obj[0].data.data, "year");
            this.numFun(this.usesTime.year, obj[1].data.data, "day");
            this.$nextTick(() => {
              this.showUses = true;
            });
          })
        )
        .catch(function(error) {
          console.log(error);
        });
    },
    numFun(startNum, maxNum, name, speed = 5000) {
      var that = this;
      let numText = startNum;
      let dis = parseInt(maxNum) - parseInt(startNum);
      let golb; // 为了清除requestAnimationFrame
      function numSlideFun() {
        // 数字动画
        numText += parseInt(dis / (speed / 1000)); // 速度的计算可以为小数 。数字越大，滚动越快
        if (numText >= maxNum) {
          numText = maxNum;
          cancelAnimationFrame(golb);
        } else {
          golb = requestAnimationFrame(numSlideFun);
        }
        that.usesTime = { ...that.usesTime, [name]: numText };
        that.amount = numText;
      }
      numSlideFun(); // 调用数字动画
    },
    getVariable() {
      this.axios
        .get("/api/p3/variable", {
          params: {
            indexname: "bldy"
          }
        })
        .then(res => {
          const { data } = res.data;
          this.variableNum = data;
        })
        .catch(function(error) {
          console.log(error);
        });
      this.axios
        .get("/api/p3/variable", {
          params: {
            indexname: "gzdy"
          }
        })
        .then(res => {
          const { data } = res.data;
          this.ruleNum = data;
        })
        .catch(function(error) {
          console.log(error);
        });
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
    decisionRateChart,
    decisionRateBChart,
    verificationChart,
    RuleChart,
    RiskPortraitChart
  },
  beforeDestroy() {
    clearInterval(this.timer);
    this.timer = null;
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
  padding-top: 1%;
  // 标题样式
  .title-frame {
    width: 100%;
    height: 5%;
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
      width: 27.2%;;
      height: 1440px;
      background: url("../../assets/images/p3/bg-cont.png") no-repeat;
      background-size: 100% 97%;
      .decisionRate {
        height: 18%;
        div {
          display: inline-block;
        }
      }
    }
    .center-box {
      position: relative;
      width: 45.4%;
      height: 100%;
      .strategy-box {
        width: 100%;
      }
      .map-charts {
        width: 100%;
        height: 1160px;
      }
      .map-panel {
        position: absolute;
        right: 0;
        bottom: 50px;
        width: 140px;
        height: 140px;
        background: url("../../assets/images/p3/panel.png") no-repeat;
      }
    }
    .content-title {
      width: 88%;
      height: 60px;
      background: url("../../assets/images/p3/title-three.png") no-repeat;
      background-size: 100% 100%;
      margin: 16px auto 0;
      span {
        display: inline-block;
        font-size: 30px;
        padding: 17px 0 0 50px;
      }
    }
  }
}
</style>