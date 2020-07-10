<template>
  <div class="second-screen">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="title-left"></div>
      <div class="global-title">小微金融</div>
      <div class="right-time">
        <weather-com />
      </div>
    </div>

    <!-- 内容 -->
    <div class="frame-content">
      <div class="content-left">
        <div class="credit-properties-div">
          <credit-properties
            class="credit-properties"
            :typeData="propertyLoans"
            :indicatorData="propertyData"
            :showCredit="showPropertyCredit"
            :propertyIds="propertyIds"
          />
          <credit-properties
            class="credit-properties"
            :typeData="personLoans"
            :indicatorData="personData"
            :showCredit="showPropertyCredit"
            :propertyIds="propertyPersonIds"
          />
        </div>

        <!-- <credit-properties-chart
          class="credit-properties-chart"
          :typeData="propertyLoans"
          :indicatorData="propertyData"
          :showCredit="showPropertyCredit"
        />-->
        <portrayal-exp v-if="showPortrayal" class="portrayal-exp" :tableDatas="OpperiodAndFinance" />
        <credit-properties-chart
          class="credit-properties-chart"
          :typeData="personLoans"
          :indicatorData="personData"
          :showCredit="showPropertyCredit"
        />
      </div>
      <div class="content-middle">
        <server-money class="server-money" />

        <sec-map-chart :chartData="mapData" class="sec-map-charts" />
        <!-- <portrayal-exp v-if="showPortrayal" class="portrayal-exp" :tableDatas="OpperiodAndFinance" />
        <portrayal-server v-if="showPortrayal" class="portrayal-server" :tableDatas="serverData" />-->
      </div>
      <div class="content-right">
        <!-- <enterprise-loan class="enterprise-loan" /> -->
      </div>
    </div>
  </div>
</template>

<script>
import WeatherCom from "../../components/weather.vue";
import CreditProperties from "../../components/second-screen/creditProperties.vue";
import CreditPropertiesChart from "../../components/second-screen/creditPropertiesChart.vue";
import EnterpriseLoan from "../../components/second-screen/enterpriseLoan.vue";
import ServerMoney from "../../components/second-screen/serverMoney.vue";
import SecMapChart from "../../components/second-screen/secondMapChart.vue";
import PortrayalExp from "../../components/second-screen/portrayalExp.vue";
// import PortrayalServer from "../../components/second-screen/portrayalServer.vue";
export default {
  mounted() {},
  data() {
    return {
      propertyLoans: {
        first: "小微属性信贷",
        littleTitle: ["分类占比", "收入水平"],
        type: 1
      },
      personLoans: {
        first: "个人经营贷",
        littleTitle: ["行业", "企业规模"],
        type: 2
      },
      showPortrayal: false,
      showPropertyCredit: false,
      propertyData: {},
      personData: {},
      OpperiodAndFinance: {},
      serverData: [],
      mapData: [],

      propertyIds: [
        {
          name: "授信金额"
        },
        {
          name: "用信企业数量"
        },
        {
          name: "时点余额"
        }
      ],
      propertyPersonIds: [
        {
          name: "笔均"
        },
        {
          name: "户均"
        },
        {
          name: "平均用信次数"
        }
      ],

      // 三农
      agriculture: [
        {
          name: "累计放款金额",
          value: 711745603.61
        },
        {
          name: "累计放款企业数",
          value: 3782.0
        },
        {
          name: "累计授信额度",
          value: 680112980.61
        }
      ],
      // 外贸贷
      foreignTrade: [
        {
          name: "中烟"
        },
        {
          name: "国际"
        },
        {
          name: "其他"
        }
      ]
    };
  },
  computed: {},
  created() {
    this.getOpperiodAndFinance();
    this.getPropertyCredit();
  },
  methods: {
    getOpperiodAndFinance() {
      this.axios
        .all([
          this.axios.get("/api/p2/opperiodAndFinance?dcode=finance"),
          this.axios.get("/api/p2/opperiodAndFinance?dcode=op_period"),
          this.axios.get("/api/p2/area")
        ])
        .then(
          this.axios.spread((...obj) => {
            let financeData = [];
            let opPeriodData = [];
            // financeData = obj[0].data.data
            //   ? obj[0].data.data.map(item => {
            //       return {
            //         name: item.xid,
            //         value: parseInt(item.val)
            //       };
            //     })
            //   : [];
            // opPeriodData = obj[1].data.data
            //   ? obj[1].data.data.map(item => {
            //       return {
            //         name: item.xid,
            //         value: parseInt(item.val)
            //       };
            //     })
            //   : [];
            financeData = [
              {
                name: "1年",
                value: 2158
              },
              {
                name: "2年",
                value: 1896
              },
              {
                name: "3年",
                value: 1543
              },
              {
                name: "4年",
                value: 4405
              }
            ];
            opPeriodData = [
              {
                name: "首贷",
                value: 5475
              },
              {
                name: "两次",
                value: 3947
              },
              {
                name: "三次及以上",
                value: 578
              }
            ];
            this.OpperiodAndFinance = Object.assign(
              {},
              {
                finance: financeData,
                op_period: opPeriodData,
                financeName: "经营年限",
                opPeriodName: "借款状态"
              }
            );
            // this.OpperiodAndFinance["finance"] = obj[0].data.data;
            // this.OpperiodAndFinance["op_period"] = obj[1].data.data;
            this.serverData = obj[2].data.data;
            this.$nextTick(() => {
              this.showPortrayal = true;
            });
          })
        );
    },

    getPropertyCredit() {
      this.axios
        .all([
          this.axios.get("/api/p2/smallCreditInfo?flag=xwsx"),
          this.axios.get("/api/p2/smallCreditInfo?flag=grjy")
        ])
        .then(
          this.axios.spread((...obj) => {
            this.propertyData = obj[0].data.data || 0;
            this.personData = obj[1].data.data || 0;
            // 假数据
            this.propertyData.applyperson = 28.16;
            this.propertyData.applyvalue = 1100774;
            this.propertyData.applynum = 52.25;
            this.personData.applyperson = 10163;
            this.personData.applyvalue = 56509;
            this.personData.applynum = 6;
            this.$nextTick(() => {
              this.showPropertyCredit = true;
            });
          })
        );
    }
  },
  components: {
    "weather-com": WeatherCom,
    "credit-properties": CreditProperties,
    "credit-properties-chart": CreditPropertiesChart,
    "enterprise-loan": EnterpriseLoan,
    "server-money": ServerMoney,
    "portrayal-exp": PortrayalExp,
    // "portrayal-server": PortrayalServe,
    "sec-map-chart": SecMapChart
  }
};
</script>

<style lang="less" scoped>
.second-screen {
  width: 4224px;
  height: 1536px;
  overflow: hidden;
  // width: 100%;
  // height: 100%;
  background: url("../../assets/images/background-second.png") no-repeat;
  background-size: 100% 100%;
  padding-top: 1%;
  font-size: 28px;
  color: #fff;

  // 标题样式
  .title-frame {
    width: 100%;
    height: 5.7%;
    background: url("../../assets/images/header.png") no-repeat;
    background-size: 100% 100%;
    display: flex;
    justify-content: space-between;
    padding: 0 1%;
    .title-left {
      width: 33%;
    }
    .global-title {
      width: 33%;
      text-align: center;
      font-size: 36px;
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
    height: 92%;
    width: 100%;
    padding: 0 0.94%;
    .content-left,
    .content-right {
      width: 29.5%;
      height: 100%;
    }

    // 中部
    .content-middle {
      width: 45%;
      height: 100%;
      padding: 0.6% 1%;
      .server-money {
        width: 100%;
        // height: 33.59%;
        height: 21.59%;
        background: url("../../assets/images/cancellation.png") no-repeat;
        background-size: 100% 100%;
      }
      .portrayal-exp {
        width: 100%;
        height: 33%;
      }
      .portrayal-server {
        width: 100%;
        height: 33%;
      }
    }

    // 左侧
    .content-left {
      .credit-properties-div {
        background: url("../../assets/images/bg-7.png") no-repeat;
        background-size: 100% 100%;
        height: 32%;
        margin-bottom: 1.6%;
      }
      .credit-properties {
        width: 100%;
        height: 50%;
        // height: 48.45%;
        // height: 16%;
        // margin-bottom: 1.6%;
        // background: url("../../assets/images/bg-7.png") no-repeat;
        // background-size: 100% 100%;
      }
      .credit-properties-chart {
        width: 100%;
        // height: 48.45%;
        height: 32%;
        margin-bottom: 1.6%;
        background: url("../../assets/images/bg-7.png") no-repeat;
        background-size: 100% 100%;
      }
    }

    // 右侧
    .content-right {
      .enterprise-loan {
        width: 100%;
        height: 98.5%;
        background: url("../../assets/images/bg-8.png") no-repeat;
        background-size: 100% 100%;
      }
    }
  }
}
</style>
