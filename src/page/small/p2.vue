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
        <titleOrNum :title="xtitle" :num="xnum" />
        <!-- <indicator-div
          :titleContent="littleTitle[0]"
          :titleStyle="littleTitleStyle[0]"
          :digital="threeMoneyArr.third"
          :digitalStyle="digitalStyle[0]"
        /> -->
        <!-- <portrayal-exp v-if="showPortrayal" class="left-center" :tableDatas="OpperiodAndFinance" /> -->
        
        <div class="left-center">
          <div class="single-distribution">
            <span class="single-graph-title">首贷占比</span>
            <cyclic-annular
              class="basic-chart"
              :ids="financeId"
              :chartData="opPeriodData"
            />
          </div>
          <div class="single-distribution">
            <span class="single-graph-title">经营年限</span>
            <whole-pie-chart
              class="basic-chart"
              :ids="financeNameId"
              :chartData="financeData"
            />
          </div>
        </div>
        <credit-properties-chart
          class="left-btm"
          :typeData="personLoans"
          :indicatorData="personData"
          :showCredit="showPropertyCredit"
        />
      </div>
      <div class="content-middle">
        <centerTitleOrNum :title="ctitle" :num="cnum" />
        <sec-map-chart v-if="mapJudge" :chartData="mapData" class="sec-map-charts" />
      </div>
      <div class="content-right">
        <titleOrNum :title="dtitle" :num="dnum" />
        <div class="right-center">
          <cyclicAndPie class="item-pie" :ids="loanModelId" :title="loanModelTitle" :chartData="loanModelData" />
          <cyclicAndPie  class="item-pie" :ids="serveModelId" :title="serveModelTitle" :chartData="serveModelData" />
        </div>
        <createOrLoan
          class="right-btm"
          :typeData="averangeLoans"
          :indicatorData="personData"
          :showCredit="showPropertyCredit"
        />
        <distribution-chart :ids="distributionIds" v-if="showRight" :chartData="distributionData" />
      </div>
    </div>
  </div>
</template>

<script>
import WeatherCom from "../../components/weather.vue";
import CreditPropertiesChart from "../../components/second-screen/creditPropertiesChart.vue";
import SecMapChart from "../../components/second-screen/secondMapChart.vue";
// import PortrayalExp from "../../components/second-screen/portrayalExp.vue";
import titleOrNum from "../../components/titleOrNum.vue"
import centerTitleOrNum from "../../components/centerTitleOrNum.vue"
import createOrLoan from "../../components/second-screen/createOrLoan.vue"
import cyclicAndPie from "../../components/second-screen/cyclicAndPie.vue"
// import dkOrFwModel from "../../components/second-screen/dkOrFwModel.vue"
// import IndicatorDiv from "@/components/first-screen/indicatorDiv.vue";
import CyclicAnnular from "@/components/second-screen/cyclicAnnlar.vue";
import WholePieChart from "@/components/second-screen/wholePieChart.vue";

// import PortrayalServer from "../../components/second-screen/portrayalServer.vue";
export default {
  mounted() {},
  data() {
    return {
      xtitle:'小微客户服务数',
      xnum: '',
      dtitle:'小微贷款累计投放金额',
      dnum: '',
      ctitle:'小微服务金额',
      cnum: '',
      loanModelId: 'loanModelId',
      loanModelData: [],
      loanModelTitle:'信用占比',
      serveModelId: 'serveModelId',
      serveModelData: [],
      serveModelTitle:'线上占比',
      personLoans: {
        littleTitle: ["服务行业", "企业规模"],
        type: 2
      },
      averangeLoans: {
        littleTitle: ["笔均金额", "贷款期限"],
        type: 2
      },
      showPortrayal: false,
      showPropertyCredit: false,
      propertyData: {},
      personData: {},
      OpperiodAndFinance: {},
      financeId:'financeId',
      financeData:[],
      financeNameId:'financeNameId',
      opPeriodData:[],
      serverData: [],
      mapData: [],
      mapJudge: false,

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

      showRight: false,
      agriculture: [{ id: "agriculture", title: "小微企业及三农" }],
      agricultureJudge: false,
      agricultureData: [],

      // 主要产品
      primaryPro: [{ id: "primaryPro", title: "主要产品" }],
      primaryProData: [],

      // 三农专案
      threeProject: [{ id: "threeProject", title: "百兴贷三农专案" }],
      threeProjectData: [],

      // 分布数据
      distributionIds: { id: "distributionIds", title: "养户分布" },
      distributionData: {},
      
      // 对接接口数据
      agricultureIndex: [
        {
          dataIndex: "indexname",
          style: "",
          formatJudge: false,
          dollorJudge: false
        },
        {
          dataIndex: "val",
          style: "number-div",
          formatJudge: true,
          dollorJudge: false
        },
        {
          dataIndex: "unit",
          style: "",
          formatJudge: false,
          dollorJudge: false
        }
      ],
      // agricultureHeader: ["产品", "累计放款金额", "累计放款企业数"],

      primaryProIndex: [
        {
          dataIndex: "productname",
          style: "",
          formatJudge: false,
          dollorJudge: true
        },
        {
          dataIndex: "bal",
          style: "number-div",
          formatJudge: true,
          dollorJudge: true
        },
        {
          dataIndex: "company",
          style: "number-div",
          formatJudge: false,
          dollorJudge: true
        }
      ],
      primaryProHeader: ["产品", "累计放款金额", "累计放款企业数"],

      threeProjectIndex: [
        {
          dataIndex: "person",
          style: "number-div",
          formatJudge: false,
          dollorJudge: true
        },
        {
          dataIndex: "quantity",
          style: "number-div",
          formatJudge: true,
          dollorJudge: true
        },
        {
          dataIndex: "bal",
          style: "number-div",
          formatJudge: true,
          dollorJudge: true
        }
      ],
      threeProjectHeader: ["农户数", "养猪头数", "放款金额"],

      mapInterval: undefined
    };
  },
  computed: {},
  created() {
    this.getOpperiodAndFinance();
    this.getPropertyCredit();
    this.getProductHandler();
    this.getMapData();
    this.getLoanModelData()
    this.getServeModelData()
  },
  methods: {
    // 将数字进行千分位格式化
      toThousands(num,tofix) {
        num = (num || 0).toString();
        var parts = num.split(".");
        var bigZeroPart = parts[0];
        var result = "";
        while (bigZeroPart.length > 3) {
          result = "," + bigZeroPart.slice(-3) + result;
          bigZeroPart = bigZeroPart.slice(0, bigZeroPart.length - 3);
        }
        if (bigZeroPart) {
          result = bigZeroPart + result;
        }
        if (parts.length > 1) {
          result += "." + parts[1].toString();
        }else {
          result += "." + '00';
        }
        if(tofix && tofix == '0') {
          result = result.split('.')[0]
        }
        return result;
      },
    getLoanModelData() {
      let _that = this;
      this.axios({
        url: "/api/p1/distributeInfo?category=xw_xydb&flag=xw",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        var indiData = data.data.data? 
        data.data.data.map(item => {
          return {
            name: item.key,
            value: item.perc
          };
        }): [];
        _that.loanModelData = indiData
      });
    },
    getServeModelData() {
      let _that = this;
      this.axios({
        url: "/api/p1/distributeInfo?category=xw_xsxx&flag=xw",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        var indiData = data.data.data? 
        data.data.data.map(item => {
          return {
            name: item.key,
            value: item.perc
          };
        }): [];
        _that.serveModelData = indiData
      });
    },
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
            let reg = /4年/g;
            financeData = obj[1].data.data
              ? obj[1].data.data.map(item => {
                  return {
                    name: item.xid.replace(reg, '3年+'),
                    value: item.perc
                  };
                })
              : [];
            opPeriodData = obj[0].data.data
              ? obj[0].data.data.map(item => {
                  return {
                    name: item.xid,
                    value: item.perc
                  };
                })
              : [];
            // financeData = [
            //   {
            //     name: "1年",
            //     value: 2158
            //   },
            //   {
            //     name: "2年",
            //     value: 1896
            //   },
            //   {
            //     name: "3年",
            //     value: 1543
            //   },
            //   {
            //     name: "3年+",
            //     value: 4405
            //   }
            // ];
            // opPeriodData = [
            //   {
            //     name: "首贷",
            //     value: 5475
            //   },
            //   {
            //     name: "两次",
            //     value: 3947
            //   },
            //   {
            //     name: "三次及以上",
            //     value: 578
            //   }
            // ];
            this.financeData=financeData;
            this.opPeriodData=opPeriodData;
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
      let _that = this;
      this.axios({
        url: "/api/p2/xwBaseinfo",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        var indiData = data.data.data || {};
        _that.xnum = this.toThousands(indiData.custnum,'0')
        _that.dnum = this.toThousands(indiData.bal)
        _that.cnum = this.toThousands(indiData.principal)
        _that.propertyData = Object.assign(
          {},
          {
            applyperson: (indiData.credit / 100000000).toFixed(2),
            applyvalue: indiData.custnum,
            applynum: (indiData.bal / 100000000).toFixed(2)
          }
        );
        _that.personData = Object.assign(
          {},
          {
            applyperson: indiData.bijun,
            applyvalue: indiData.hujun,
            applynum: indiData.avgnum
          }
        );
        _that.$nextTick(() => {
          _that.showPropertyCredit = true;
        });
      });
    },

    // 获取右侧产品
    getProductHandler() {
      // this.agricultureData = [
      //   {
      //     indexname: "ljfkje",
      //     val: 711745603,
      //     unit: "元"
      //   }
      // ];
      this.axios
        .all([
          this.axios.get("/api/p2/establishment?indexname=ljfkje"),
          this.axios.get("/api/p2/product"),
          this.axios.get("/api/p2/specialCase"),
          this.axios.get("/api/p2/distribution"),
          this.axios.get("/api/p2/specialCase"),
          this.axios.get("/api/p2/establishment?indexname=ljfkqy"),
          this.axios.get("/api/p2/establishment?indexname=ljsxed")
        ])
        .then(
          this.axios.spread((...obj) => {
            // 0、小微企业及三农
            this.agricultureData = [];
            this.agricultureData.push(
              obj[0].data.data ? obj[0].data.data[0] : []
            );
            this.agricultureData.push(
              obj[5].data.data ? obj[5].data.data[0] : []
            );
            this.agricultureData.push(
              obj[6].data.data ? obj[6].data.data[0] : []
            );

            // // 主要产品
            this.primaryProData = obj[1].data.data || 0;

            // //百兴贷三农专案
            this.threeProjectData = [obj[2].data.data] || 0;

            // //养户分布
            let chartData = obj[3].data.data || [];
            let xAxis = [],
              yAxis1 = [],
              yAxis2 = [];
            chartData.forEach(item => {
              xAxis.push(item.province);
              yAxis1.push(item.quantity);
              yAxis2.push(item.person);
            });
            let totalData = obj[4].data.data || {};
            xAxis.push("总计");
            yAxis1.push(totalData.quantity);
            yAxis2.push(totalData.person);
            this.distributionData = Object.assign(
              {},
              { xAxis, yAxis1, yAxis2 }
            );

            this.$nextTick(() => {
              this.showRight = true;
            });
          })
        );
    },

    getMapData() {
      this.getMapDataHandler();
    },

    // 格式化千分位
    thousandFormat(value, fixed) {
      fixed = fixed !== undefined ? fixed : 2;
      if (value === null || value === undefined || isNaN(parseFloat(value))) {
        return;
      }
      // 将数字进行千分位格式化
      function toThousands(num) {
        num = (num || 0).toString();
        var parts = num.split(".");
        var bigZeroPart = parts[0];
        var result = "";
        while (bigZeroPart.length > 3) {
          result = "," + bigZeroPart.slice(-3) + result;
          bigZeroPart = bigZeroPart.slice(0, bigZeroPart.length - 3);
        }
        if (bigZeroPart) {
          result = bigZeroPart + result;
        }
        if (parts.length > 1) {
          result += "." + parts[1].toString();
        }
        return result;
      }

      value = parseFloat(value).toFixed(fixed);
      value = toThousands(value);
      return value;
    },
    getMapDataHandler() {
      var _that = this;
      // 获取地图数据
      this.axios({
        url: "/api/p2/graphInfo",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var tData = data.data.data;
          let index = 0;
          _that.mapData = [];
          let firstTmpData = tData[index];
          _that.mapData.push({
            company: firstTmpData.companyname,
            province: firstTmpData.province,
            credit: _that.thousandFormat(firstTmpData.credit, 2),
            value: [firstTmpData.longitude, firstTmpData.latitude]
          });

          this.mapInterval = setInterval(
            () => {
              // _that.getMapDataHandler();
              if (index >= tData.length - 1) {
                _that.getMapDataHandler();
                clearInterval(this.mapInterval);
                return;
              }
              index++;
              _that.mapData = [];
              let tmpData = tData[index];
              _that.mapData.push({
                company: tmpData.companyname,
                province: tmpData.province,
                credit: _that.thousandFormat(tmpData.credit, 2),
                value: [tmpData.longitude, tmpData.latitude]
              });
            },
            5000,
            _that,
            index
          );
          this.$nextTick(() => {
            _that.mapJudge = true;
          });
          // _that.mapData = [
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000",
          //     value: [116.4551, 40.2539, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000",
          //     value: [103.9526, 30.7617, 48]
          //   }
          // ];
        }
      });
    }
  },
  components: {
    "weather-com": WeatherCom,
    "credit-properties-chart": CreditPropertiesChart,
    // "portrayal-exp": PortrayalExp,
    // "portrayal-server": PortrayalServe,
    "sec-map-chart": SecMapChart,
    "titleOrNum":titleOrNum,
    "centerTitleOrNum":centerTitleOrNum,
    "createOrLoan":createOrLoan,
    "cyclicAndPie":cyclicAndPie,
    // "indicator-div": IndicatorDiv,
    "cyclic-annular": CyclicAnnular,
    "whole-pie-chart": WholePieChart,
  }
};
</script>

<style lang="less" scoped>
.second-screen {
  width: 4224px;
  height: 1536px;
  overflow: hidden;
  background: url("../../assets/images/background-second.png") no-repeat;
  background-size: 100% 100%;
  // padding-top: 1%;
  font-size: 28px;
  color: #fff;
  font-family: Microsoft YaHei;

  // 标题样式
  .title-frame {
    width: 100%;
    height: 7%;
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
      letter-spacing: 1px;
      text-align: center;
      font-size: 64px;
      font-weight: bold;
      font-family: FZZhengHeiS-B-GB;
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
    height: 94%;
    width: 100%;
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
    }

    // 左侧
    .content-left {
      background: rgba(22, 28, 40, 0.32);
      // background: url("../../assets/images/bg-8.png") no-repeat;
      // background-size: 100% 100%;
      .left-center {
        width: 100%;
        height: 38%;
        display: flex;
        .single-distribution {
          width: 50%;
          height: 100%;
          .single-graph-title {
            font-weight: bold;
            font-size: 32px;
            line-height: 40px;
            letter-spacing: 0px;
            margin-left: 40px;
          }
          .basic-chart {
            height: 87%;
            width: 100%;
          }
        }
      }
      .left-btm {
        width: 100%;
        height: 40%;
        margin-bottom: 1.6%;
      }
    }

    // 右侧
    .content-right {
      // width: 100%;
      height: 100%;
      background: rgba(22, 28, 40, 0.32);
      // background: url("../../assets/images/bg-8.png") no-repeat;
      // background-size: 100% 100%;
      .right-center {
        width: 100%;
        height: 38%;
        display: flex;
        flex-wrap: nowrap;

        .item-pie {
          height: 100%;
          width: 45%;
          &:first-child {
            margin-right: 5%;
          }
        }
      }
      .right-btm{
        width: 100%;
        height: 40%;
      }
    }
  }
}
</style>
