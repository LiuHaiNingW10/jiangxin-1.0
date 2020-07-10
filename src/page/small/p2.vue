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

        <sec-map-chart v-if="mapJudge" :chartData="mapData" class="sec-map-charts" />
        <!-- <portrayal-exp v-if="showPortrayal" class="portrayal-exp" :tableDatas="OpperiodAndFinance" />
        <portrayal-server v-if="showPortrayal" class="portrayal-server" :tableDatas="serverData" />-->
      </div>
      <div class="content-right">
        <!-- <enterprise-loan class="enterprise-loan" /> -->
        <form-chart
          :ids="agriculture"
          v-if="showRight"
          :chartData="{agriculture: agricultureData}"
          class="form-charts"
          :tableTitle="agriculture[0].title"
          :tableIndex="agricultureIndex"
        />
        <form-chart
          :ids="primaryPro"
          v-if="showRight"
          :chartData="{primaryPro: primaryProData}"
          class="form-charts"
          :tableTitle="primaryPro[0].title"
          :tableIndex="primaryProIndex"
        />
        <form-chart
          :ids="threeProject"
          v-if="showRight"
          :chartData="{threeProject: threeProjectData}"
          class="form-charts"
          :tableTitle="threeProject[0].title"
          :tableIndex="threeProjectIndex"
          :tableHeader="threeProjectHeader"
        />
        <distribution-chart :ids="distributionIds" v-if="showRight" :chartData="distributionData" />
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
import FormChart from "../../components/first-screen/newFormChart.vue";
import DistributionChart from "../../components/second-screen/distributionChart.vue";

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
      // agricultureData: [
      //   [
      //     { name: "累计放款金额", style: "" },
      //     { name: "711,745,603.61", style: "number-div" },
      //     { name: "元", style: "" }
      //   ],
      //   [
      //     { name: "累计放款企业数", style: "" },
      //     { name: "3,782.0", style: "number-div" },
      //     { name: "家", style: "" }
      //   ],
      //   [
      //     { name: "累计授信额度", style: "" },
      //     { name: "680,112,980.61", style: "number-div" },
      //     { name: "元", style: "" }
      //   ]
      // ],
      agricultureData: [],
      // agricultureData: [
      //   {
      //     indicator: "累计放款金额",
      //     value: "711,745,603.61",
      //     unit: "元"
      //   },
      //   {
      //     indicator: "累计放款企业数",
      //     value: "3,782.0",
      //     unit: "家"
      //   },
      //   {
      //     indicator: "累计授信额度",
      //     value: "680,112,980.61",
      //     unit: "元"
      //   }
      // ],

      // 主要产品
      primaryPro: [{ id: "primaryPro", title: "主要产品" }],
      primaryProData: [],
      // primaryProData: [
      //   [
      //     { name: "产品", style: "" },
      //     { name: "累计放款金额", style: "" },
      //     { name: "累计放款企业数", style: "" }
      //   ],
      //   [
      //     { name: "百链融 - 订货易", style: "" },
      //     { name: "¥ 198,994,351.9", style: "number-div" },
      //     { name: "3613", style: "number-div" }
      //   ],
      //   [
      //     { name: "百链融 - 销货易", style: "" },
      //     { name: "¥ 3,247,000", style: "number-div" },
      //     { name: "11", style: "number-div" }
      //   ],
      //   [
      //     { name: "百链融 - 外贸易", style: "" },
      //     { name: "¥ 6,190,000", style: "number-div" },
      //     { name: "10", style: "number-div" }
      //   ],
      //   [
      //     { name: "百农宝 - 养殖易", style: "" },
      //     { name: "¥ 195,600,000", style: "number-div" },
      //     { name: "79", style: "number-div" }
      //   ]
      // ],

      // 三农专案
      threeProject: [{ id: "threeProject", title: "百兴贷三农专案" }],
      threeProjectData: [],
      // threeProjectData: [
      //   [
      //     { name: "农户数", style: "" },
      //     { name: "养猪头数", style: "" },
      //     { name: "放款金额", style: "" }
      //   ],
      //   [
      //     { name: "79", style: "number-div" },
      //     { name: "93,150", style: "number-div" },
      //     { name: "¥ 19,560", style: "number-div" }
      //   ]
      // ],

      // 分布数据
      distributionIds: { id: "distributionIds", title: "养户分布" },
      distributionData: {},
      // distributionData: {
      //   xAxis: [
      //     "安徽",
      //     "河北",
      //     "河南",
      //     "湖北",
      //     "吉林",
      //     "江西",
      //     "辽宁",
      //     "四川",
      //     "总计"
      //   ],
      //   yAxis1: [
      //     "4471",
      //     "1563",
      //     "3071",
      //     "4270",
      //     "1112",
      //     "1562",
      //     "653",
      //     "2858",
      //     "19560"
      //   ],
      //   yAxis2: ["18", "7", "9", "21", "3", "6", "3", "12", "79"]
      // },

      // 对接接口数据
      agricultureIndex: [
        {
          dataIndex: "indexname",
          style: ""
        },
        {
          dataIndex: "val",
          style: "number-div"
        },
        {
          dataIndex: "unit",
          style: ""
        }
      ],
      // agricultureHeader: ["产品", "累计放款金额", "累计放款企业数"],

      primaryProIndex: [
        {
          dataIndex: "productname",
          style: ""
        },
        {
          dataIndex: "bal",
          style: "number-div"
        },
        {
          dataIndex: "company",
          style: "number-div"
        }
      ],
      primaryProHeader: ["产品", "累计放款金额", "累计放款企业数"],

      threeProjectIndex: [
        {
          dataIndex: "person",
          style: "number-div"
        },
        {
          dataIndex: "quantity",
          style: "number-div"
        },
        {
          dataIndex: "bal",
          style: "number-div"
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
            financeData = obj[1].data.data
              ? obj[1].data.data.map(item => {
                  return {
                    name: item.xid,
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
            //     name: "4年",
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
      // this.axios
      //   .all([
      //     this.axios.get("/api/p2/xwBaseinfo"),
      //     this.axios.get("/api/p2/smallCreditInfo?flag=grjy")
      //   ])
      //   .then(
      //     this.axios.spread((...obj) => {
      //       // this.propertyData = obj[0].data.data || 0;
      //       // this.personData = obj[1].data.data || 0;
      //       // // 假数据
      //       // this.propertyData.applyperson = 28.16;
      //       // this.propertyData.applyvalue = 1100774;
      //       // this.propertyData.applynum = 52.25;
      //       // this.personData.applyperson = 10163;
      //       // this.personData.applyvalue = 56509;
      //       // this.personData.applynum = 6;
      //       // this.$nextTick(() => {
      //       //   this.showPropertyCredit = true;
      //       // });
      //     })
      //   );
      let _that = this;
      this.axios({
        url: "/api/p2/xwBaseinfo",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        var indiData = data.data.data || {};
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
          this.axios.get("/api/p2/specialCase")
        ])
        .then(
          this.axios.spread((...obj) => {
            // 0、小微企业及三农
            this.agricultureData = obj[0].data.data;

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
      var _that = this;
      var index = 0;
      // if (mapInterval) clearInterval(mapInterval);
      this.mapInterval = setInterval(
        () => {
          _that.getMapDataHandler();
        },
        20000,
        _that,
        index
      );
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
          _that.mapData = [
            ...tData.map(item => {
              return {
                company: item.companyname,
                credit: _that.thousandFormat(item.credit, 2),
                province: item.province,
                value: [item.longitude, item.latitude]
              };
            })
          ];
          // _that.mapData = [
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [116.4551, 40.2539, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [103.9526, 30.7617, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [119.1803, 31.2891, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [108.384366, 30.439702, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [87.9236, 43.5883, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [117.29, 32.0581, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [113.0823, 28.2568, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [121.4648, 25.563, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [113.12244, 23.009505, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [103.9526, 30.7617, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [116.4551, 40.2539, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [103.9526, 30.7617, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [116.4551, 40.2539, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [103.9526, 30.7617, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [116.4551, 40.2539, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [103.9526, 30.7617, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [116.4551, 40.2539, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [103.9526, 30.7617, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "28岁",
          //     sex: "男",
          //     type: "授信申请",
          //     sum: "3000000",
          //     value: [116.4551, 40.2539, 48]
          //   },
          //   {
          //     name: "王**",
          //     age: "25岁",
          //     sex: "女",
          //     type: "授信申请",
          //     sum: "7000000",
          //     value: [103.9526, 30.7617, 48]
          //   }
          // ];
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
    "credit-properties": CreditProperties,
    "credit-properties-chart": CreditPropertiesChart,
    "enterprise-loan": EnterpriseLoan,
    "server-money": ServerMoney,
    "portrayal-exp": PortrayalExp,
    // "portrayal-server": PortrayalServe,
    "sec-map-chart": SecMapChart,
    "distribution-chart": DistributionChart,
    "form-chart": FormChart
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
      // width: 100%;
      height: 100%;
      padding: 1% 0% 0%;
      background: url("../../assets/images/bg-8.png") no-repeat;
      background-size: 100% 100%;
      .enterprise-loan {
        width: 100%;
        height: 98.5%;
        background: url("../../assets/images/bg-8.png") no-repeat;
        background-size: 100% 100%;
      }
      .form-charts {
        margin-left: 10%;
      }
    }
  }
}
</style>
