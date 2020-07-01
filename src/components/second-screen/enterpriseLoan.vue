<template>
  <div class="enterprise-loan">
    <div class="cloud-content enterprise-div">
      <div class="content-title">
        <span class="first-text-span">小微企业贷款</span>
        <loan-chart v-if="bubbleJudge" :chartData="currentBubbleData" />
      </div>
    </div>
    <div class="enterprise-atlas enterprise-div enterprise-content">
      <div class="second-title">
        <span class="text-span">企业图谱</span>
      </div>
      <div id="graph"></div>
    </div>
    <div class="enterprise-credit enterprise-div enterprise-content">
      <div class="second-title">
        <span class="text-span">企业信用</span>
        <credit-chart :chartData='creditData' v-if="creditJudge"/>
      </div>
    </div>
  </div>
</template>

<script>
import LoanChart from "@/components/second-screen/loanChart";
import CreditChart from "@/components/second-screen/creditChart";
export default {
  name: "",
  props: [],
  data() {
    return {
      bubbleData: [],
      currentBubbleData: [],
      bubbleJudge: false,
      creditJudge: false,
      creditData: []
    };
  },
  computed: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.getBubbleData();
    },
    getGraphData(name) {
      var that = this
      let url = "/api/p2/cmpGraph?company=" + name;
      this.axios({
        url: url,
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var graphData = data.data.data;
          var sxjeData = graphData.sxje;
          let tmpData = [];
          tmpData.push({
            value: [
              sxjeData.basicinfo,
              sxjeData.jyfx,
              sxjeData.nsxy,
              sxjeData.rzqk,
              sxjeData.ylnl,
              sxjeData.zczj
            ],
            name: sxjeData.companyname
          });
          that.creditData = [...tmpData]
          that.$nextTick(() => {
            that.creditJudge = true
          })
        }
      });
    },

    getBubbleData() {
      var that = this;
      this.axios({
        url: "/api/p2/creditAmount",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var tmpData = data.data.data;
          if (tmpData == null) return;
          that.bubbleData = tmpData.map(item => {
            return {
              name: item.companyname,
              value: item.credit,
              symbolSize: Math.floor(Math.random() * 100) + 300,
              symbol: `image://${require("@/assets/images/p2/loan" +
                (Math.floor(Math.random() * 5) + 1) +
                ".svg")}`,
              draggable: true
            };
          });
          // that.bubbleData = tmpData
          that.$nextTick(() => {
            // that.bubbleJudge = true;
            that.rollBubble();
          });
        }
      });
    },
    rollBubble() {
      var that = this;
      var index = 0;
      if (bubbleInterval) clearInterval(bubbleInterval);
      var bubbleInterval = setInterval(
        () => {
          if (index === that.bubbleData.length) index = 0;
          that.currentBubbleData = [];
          that.currentBubbleData.push(that.bubbleData[index]);
          let company = that.currentBubbleData[0].name;
          this.getGraphData(company);
          index++;
          that.$nextTick(() => {
            that.bubbleJudge = true;
          });
        },
        5000,
        that,
        index
      );
      this.bubbleData;
    }
  },
  components: {
    LoanChart,
    CreditChart
  }
};
</script>

<style lang="less">
.enterprise-loan {
  .enterprise-div {
    width: 100%;
    height: 32%;
    margin-bottom: 1%;
    padding: 1.7% 1.97%;
    .second-title {
      width: 100%;
      height: 9.22%;
      font-size: 24px;
      background: url("../../assets/images/title4备份 4.png") no-repeat;
      background-size: 100% 100%;
    }
  }
  .cloud-content {
    .content-title {
      width: 100%;
      height: 12.96%;
      background: url("../../assets/images/bg-content-title.png") no-repeat;
      background-size: 100% 100%;
      padding-left: 4.64%;
      margin-bottom: 2%;
      line-height: 220%;
    }
  }
  .enterprise-content {
    padding: 0 4%;
  }
  .text-span {
    display: inline-block;
    margin-left: 4%;
  }
}
</style>
