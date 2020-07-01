<template>
  <div class="second-screen">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="title-left"></div>
      <div class="global-title">累计服务金额/当日服务金额</div>
      <div class="right-time">
        <weather-com />
      </div>
    </div>

    <!-- 内容 -->
    <div class="frame-content">
      <div class="content-left">
        <credit-properties
          class="credit-properties"
          :typeData="propertyLoans"
          :indicatorData="propertyData"
          :showCredit="showPropertyCredit"
        />
        <credit-properties
          class="credit-properties"
          :typeData="personLoans"
          :indicatorData="personData"
          :showCredit="showPropertyCredit"
        />
      </div>
      <div class="content-middle">
        <server-money class="server-money" />
        <portrayal-exp v-if="showPortrayal" class="portrayal-exp" :tableDatas="OpperiodAndFinance" />
        <portrayal-server v-if="showPortrayal" class="portrayal-server" :tableDatas="serverData" />
      </div>
      <div class="content-right">
        <enterprise-loan class="enterprise-loan" />
      </div>
    </div>
  </div>
</template>

<script>
import WeatherCom from "../../components/weather.vue";
import CreditProperties from "../../components/second-screen/creditProperties.vue";
import EnterpriseLoan from "../../components/second-screen/enterpriseLoan.vue";
import ServerMoney from "../../components/second-screen/serverMoney.vue";
import PortrayalExp from "../../components/second-screen/portrayalExp.vue";
import PortrayalServer from "../../components/second-screen/portrayalServer.vue";
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
      serverData: []
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
            financeData = obj[0].data.data
              ? obj[0].data.data.map(item => {
                  return {
                    name: item.xid,
                    value: parseInt(item.val)
                  };
                })
              : [];
            opPeriodData = obj[1].data.data
              ? obj[1].data.data.map(item => {
                  return {
                    name: item.xid,
                    value: parseInt(item.val)
                  };
                })
              : [];
            this.OpperiodAndFinance = Object.assign(
              {},
              { finance: financeData, op_period: opPeriodData }
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
    "enterprise-loan": EnterpriseLoan,
    "server-money": ServerMoney,
    "portrayal-exp": PortrayalExp,
    "portrayal-server": PortrayalServer
  }
};
</script>

<style lang="less">
.second-screen {
  width: 4224px;
  height: 1536px;
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
      font-size: 16px;
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
      width: 41%;
      height: 100%;
      padding: 0.6% 1%;
      .server-money {
        width: 100%;
        height: 33.59%;
        background: url("../../assets/images/销户.png") no-repeat;
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
      .credit-properties {
        width: 100%;
        height: 48.45%;
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
