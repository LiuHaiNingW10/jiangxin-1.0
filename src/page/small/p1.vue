<template>
  <div class="container">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="title-left">
        <!-- <span class="title-left-span">累计信贷服务金额：</span>
        <div class="nwwest-roll" id="nwwest-roll">
          <ul id="roll-ul" class="roll-ul">
            <li
              v-for="(item, index) in Totallist"
              :key="index"
              ref="rollul"
              :class="{anim:animate==true}"
            >
              <span class="total-money-span">¥ {{item}}</span>
            </li>
          </ul>
        </div>

        <scroll-span :number="totalMoney" class="total-money-span" ids="total" />-->
        <!-- <span class="left-coin">¥ {{totalMoney}}</span> -->
      </div>
      <div class="global-title">消费金融</div>
      <div class="right-time">
        <!-- <div class="weather-module">
    <img :src="currentImg" alt />
    <span class>{{currentWeather.high}} ℃ ~ {{currentWeather.low}} ℃</span>
    <span class="time-span">{{currentTime.date}}</span>
    <span class="time-span">{{currentTime.time}}</span>
        </div>-->
        <weather-com />
      </div>
    </div>

    <!-- 内容 -->
    <div class="frame-content">
      <div class="accruing-person">
        <!-- <div class="content-title">信贷服务客户数</div> -->
        <indicator-div
          :titleContent="littleTitle[0]"
          :titleStyle="littleTitleStyle[1]"
          :digital="threeMoneyArr.third"
          :digitalStyle="digitalStyle[1]"
        />
        <!-- <indicator-chart
          v-if="threeMoneyArr.third"
          :chartData="threeMoneyArr.third"
          type="person"
          chartId="total-person"
          :styleData="styleObj"
          marginTop="-120px"
          :styleSingle="singleStyle"
        />-->
        <columnar-chart
          :ids="columnarId"
          v-if="currentPersonJudge"
          :chartData="columnarData"
          class="columnar-chart"
          :lineIds="repeatPurchaseId"
        />
        <div class="distribution middle-distirbution">
          <!-- <funnel-chart
            class="left-distribution single-distribution"
            :ids="funnelId"
            v-if="educationJudge"
            :chartData="ageData" 
          />-->
          <!-- <pie-chart
            class="left-distribution single-distribution"
            :ids="funnelId"
            v-if="educationJudge"
            :chartData="ageData"
            chartTitle="年龄分布"
          />-->
          <!-- <income-level
            class="left-distribution single-distribution"
            :ids="funnelId"
            v-if="educationJudge"
            :chartData="ageData"
            chartTitle="年龄分布"
          />
          <pie-chart
            class="right-distribution single-distribution"
            :ids="pieId"
            v-if="educationJudge"
            :chartData="educationData"
            chartTitle="学历分布"
          />-->
          <div class="single-distribution">
            <span class="single-graph-title">年龄</span>
            <cyclic-annular
              class="basic-chart"
              :ids="funnelId"
              v-if="educationJudge"
              :chartData="ageData"
            />
          </div>
          <div class="single-distribution">
            <span class="single-graph-title">学历</span>
            <whole-pie-chart
              class="basic-chart"
              :ids="pieId"
              v-if="educationJudge"
              :chartData="educationData"
            />
          </div>
        </div>
        <div class="distribution bottom-distribution">
          <!-- <funnel-chart
            class="left-distribution single-distribution"
            :ids="funnelId"
            v-if="educationJudge"
            :chartData="ageData" 
          />-->
          <!-- <pie-chart
            class="left-distribution single-distribution"
            :ids="cityId"
            v-if="cityJudge"
            :chartData="cityData"
            chartTitle="地域分布"
          />-->
          <!-- <pie-chart
            class="right-distribution single-distribution"
            :ids="pieId"
            v-if="educationJudge"
            :chartData="educationData"
          />-->
          <!-- <form-chart
            :ids="elite"
            v-if="eliteJudge"
            :chartData="{elite: eliteData}"
            class="form-charts"
            tableTitle="精准扶贫"
          />-->
          <div class="single-distribution">
            <span class="single-graph-title">地域</span>
            <simple-column
              class="basic-chart"
              :ids="cityId"
              v-if="cityJudge"
              :chartData="cityData"
            />
          </div>
          <div class="single-distribution">
            <span class="single-graph-title">收入</span>
            <simple-column
              class="basic-chart"
              :ids="incomeId"
              v-if="incomeJudge"
              :chartData="income"
            />
          </div>
        </div>
      </div>
      <div class="current-amounts">
        <!-- <div class="content-title">当日信贷投放金额</div> -->
        <!-- <indicator-chart
          v-if="threeMoneyArr.second"
          :chartData="threeMoneyArr.second"
          chartId="current-money"
          :styleData="styleObj"
          marginTop="-120px"
          :styleSingle="singleStyle"
        />-->
        <indicator-div
          :titleContent="littleTitle[1]"
          :titleStyle="littleTitleStyle[1]"
          :digital="threeMoneyArr.second"
          :digitalStyle="digitalStyle[1]"
          class="middle-digital-span"
        />

        <!-- 地图 -->
        <map-chart v-if="mapJudge" :chartData="mapData" class="map-charts" />
      </div>
      <div class="accruing-amounts">
        <!-- <div class="content-title">累计信贷投放金额</div> -->
        <!-- <indicator-chart
          v-if="threeMoneyArr.first"
          :chartData="threeMoneyArr.first"
          chartId="total-money"
          :styleData="styleObj"
          marginTop="-120px"
          :styleSingle="singleStyle"
        />-->
        <indicator-div
          :titleContent="littleTitle[2]"
          :titleStyle="littleTitleStyle[1]"
          :digital="threeMoneyArr.first"
          :digitalStyle="digitalStyle[1]"
        />
        <!-- <indicator-chart v-if="testN" :chartData="testN" chartId="total-money" /> -->
        <div class="single-distribution right-top-distribution">
          <span class="single-graph-title">当日实时放款</span>
          <line-chart
            :ids="id"
            v-if="leftLineJudge"
            :chartData="getChartData"
            :trueData="leftLineData"
            class="line-chart"
          />
        </div>
        <div class="right-middle-div">
          <div class="single-distribution half-middle-distribution">
            <span class="single-graph-title">授信时长</span>
            <sec-chart v-if="extensionDateJudge" :chartData="extensionDate" ids="extension" />
          </div>
          <div class="single-distribution half-middle-distribution">
            <span class="single-graph-title">贷款时长</span>
            <sec-chart v-if="loanDateJudge" :chartData="loanDate" ids="loan" />
          </div>
        </div>
        <div class="right-bottom-div">
          <div class="single-distribution half-bottom-distribution">
            <span class="single-graph-title">
              笔均金额
              <span class="avg-title">平均值</span>
              <span class="avg-num">{{avgNumberAvg}}</span>
            </span>
            <simple-column
              class="basic-chart"
              :ids="avgNumberId"
              v-if="avgNumberJudge"
              :chartData="avgNumber"
            />
          </div>
          <div class="single-distribution half-bottom-distribution">
            <span class="single-graph-title">
              用信天数
              <span class="avg-title">平均值</span>
              <span class="avg-num">{{creditDateAvg}}</span>
            </span>
            <simple-column
              class="basic-chart"
              :ids="creditDateId"
              v-if="creditDateJudge"
              :chartData="creditDate"
            />
          </div>
        </div>
        <!-- <left-indicator-chart v-if="allDataIndicator" :chartData="allDataIndicator" /> -->
        <!-- <repeat-purchase
          :ids="repeatPurchaseId"
          :v-if="plateJudge"
          :chartData="{linkRelativeRatio: linkRelativeRatio,dialPlate: dialPlate,lineData: lineData}"
          class="repeat-purchase-chart"
        />-->
        <!-- <form-chart
          :ids="formIds"
          v-if="consume"
          :chartData="{consume: consume, scale: scale}"
          class="form-charts"
        />
        横向柱状图-->
        <!-- <div class="bottom-title-div">
          <div class="bottom-title">审批时长（秒）</div>
          <div class="bottom-title">90天复购率</div>
        </div>
        <oil-pie-chart :ids="oilPieIds" :chartData="oilPieData" />-->
        <!-- <div class="bottom-title-div">
          <div class="bottom-title">星座&剁手</div>
          <div class="bottom-title">地域&贷款规模</div>
        </div>
        <person-columnar class="person-columnar" ids="consume" :chartData="consume" v-if="consume" />
        <person-columnar class="person-columnar" ids="scale" v-if="scale" :chartData="scale" />-->
      </div>
    </div>
    <!-- <div class="title-frame"></div> -->
  </div>
</template>

<script>
import LineChart from "../../components/first-screen/lineChart.vue";
import SecChart from "../../components/first-screen/secChart.vue";
// import LeftIndicatorChart from "../../components/first-screen/leftIndicator.vue";
import ColumnarChart from "../../components/first-screen/columnarChart.vue";
import MapChart from "../../components/first-screen/mapChart.vue";
// import OilPieChart from "../../components/first-screen/oilPieChart.vue";
import WeatherCom from "../../components/weather.vue";
import IndicatorDiv from "@/components/first-screen/indicatorDiv.vue";
import CyclicAnnular from "@/components/first-screen/cyclicAnnular.vue";
import WholePieChart from "@/components/first-screen/wholePieChart.vue";
import SimpleColumn from "@/components/first-screen/simpleColumn.vue";

import Money from "../../components/first-screen/icon/money.svg";
import Avg from "../../components/first-screen/icon/avg.svg";
import Life from "../../components/first-screen/icon/life.svg";
import User from "../../components/first-screen/icon/user.svg";

// import IndicatorChart from "../../components/first-screen/indicator.vue";
// import RepeatPurchase from "../../components/first-screen/repeatPurchase.vue";
// import PieChart from "../../components/first-screen/pieChart.vue";
// import FunnelChart from "../../components/first-screen/funnelChart.vue";
// import FormChart from "../../components/first-screen/formChart.vue";
// import PersonColumnar from "../../components/second-screen/personColumnar.vue";
// import IncomeLevel from "../../components/second-screen/incomeLevel.vue";
// import ScrollSpan from "../../components/scrollSpan.vue";
import { setInterval, clearInterval } from "timers";
export default {
  created() {
    this.getLineData();
    this.getMapData();
    this.rollData();
    this.rollTimelyData();

    // let that = this; //模拟数字增长(也可以是数据库定时查的或ws中接收的)
    // setInterval(function() {
    //   that.testN = ++that.testN; //数字变化后调用滚动事件
    //   that.scroll(that.testN, that.$refs);
    // }, 3000);
  },
  mounted() {
    // this.getTime();
    // this.getWeather();
    // this.getCurrentWeather();
  },
  data() {
    return {
      littleTitle: ["信贷服务客户数", "当日信贷投放金额", "累计信贷投放金额"],
      littleTitleStyle: [
        {
          textAlign: "left"
        },
        {
          textAlign: "center"
        },
        {}
      ],
      digitalStyle: [
        {
          marginTop: "34px",
          textAlign: "left"
        },
        {
          marginTop: "34px",
          textAlign: "center"
        }
      ],
      styleObj: {
        height: "90px",
        fontSize: "64px"
      },
      singleStyle: {
        width: "3%",
        // marginLeft: "1%",
        lineHeight: "150%"
      },
      mapInterval: undefined,
      coinInterval: undefined,

      testN: 100,
      animate: true,

      Totallist: [0, 0],
      // 累计信贷服务金额
      totalMoney: "",
      preTotalMoney: "",

      // 三个数字栏目
      threeMoneyArr: {
        first: "",
        second: "",
        third: ""
      },

      // mapData
      mapData: [],
      mapJudge: false,

      // 当日人数
      currentPerson: undefined,
      currentPersonJudge: false,

      // 左侧实时放款数据
      allData: [
        {
          chartName: "当日实时放款",
          money: Math.floor(Math.random() * 100000)
        }
      ],
      leftLineData: undefined,
      leftLineJudge: false,
      id: ["echarts01"],

      //授信时长 贷款时长
      extensionDate: undefined,
      extensionDateJudge: false,
      loanDate: undefined,
      loanDateJudge: false,
      // 左侧六个指标数据
      allDataIndicator: undefined,
      // allDataIndicator: [
      //   {
      //     name: "余额",
      //     index: "amount",
      //     data: 4320
      //   },
      //   {
      //     name: "笔均",
      //     index: "avg",
      //     data: 4320
      //   },
      //   {
      //     name: "户均",
      //     index: "savg",
      //     data: "50%"
      //   },
      //   {
      //     name: "平均期限",
      //     index: "avg-deadline",
      //     data: 4320
      //   },
      //   {
      //     name: "贷款时长",
      //     index: "loan-time",
      //     data: "651min"
      //   },
      //   {
      //     name: "授信成功率",
      //     index: "rate",
      //     data: "18%"
      //   }
      // ],
      idIndicator: ["card1", "card2", "card3", "card4", "card5", "card6"],

      // 左侧复购数据
      linkRelativeRatio: {},
      plateJudge: false,
      dialPlate: [],
      lineData: [],
      // lineData: [
      //   {
      //     city: ["0", "4", "8", "12", "16", "20"],
      //     num: ["40", "60", "22", "85", "50", "40"]
      //   },
      //   {
      //     city: ["0", "4", "8", "12", "16", "20"],
      //     num: ["40", "60", "22", "85", "50", "40"]
      //   }
      // ],
      // repeatPurchaseData: {
      //   linkRelativeRatio: {
      //     echarts02: [
      //       {
      //         title: "环比上周",
      //         data: "+23.19%"
      //       },
      //       {
      //         title: "环比上月",
      //         data: "+15.19%"
      //       }
      //     ],
      //     echarts03: [
      //       {
      //         title: "环比上周",
      //         data: "+12.19%"
      //       },
      //       {
      //         title: "环比上月",
      //         data: "+10.19%"
      //       }
      //     ]
      //   },
      //   dialPlate: ["34", "89"],
      //   lineData: [
      //     {
      //       city: ["0", "4", "8", "12", "16", "20"],
      //       num: ["40", "60", "22", "85", "50", "40"]
      //     },
      //     {
      //       city: ["0", "4", "8", "12", "16", "20"],
      //       num: ["40", "60", "22", "85", "50", "40"]
      //     }
      //   ]
      // },
      repeatPurchaseId: [
        {
          // id: "echarts02",
          id: "lineCharts01",
          title: ""
          // title: "30天复购"
        },
        {
          id: "lineCharts02",
          // id: "echarts03",
          title: ""
          // title: "90天复购"
        }
      ],

      // 油表
      oilPieIds: ["oilCharts01", "oilCharts02"],
      oilPieData: {},

      // 右侧柱状图数据
      columnarData: {
        totalData: [0, 0],
        columnAllData: []
        // columnAllData: [
        //   {
        //     xAxis: [
        //       "制造业",
        //       "建筑业",
        //       "农林牧渔",
        //       "房地产",
        //       "金融业",
        //       "居民服务及其他"
        //     ],
        //     yAxis: [5000, 2600, 1300, 1300, 1250, 1500]
        //   },
        //   {
        //     xAxis: [
        //       "制造业",
        //       "建筑业",
        //       "农林牧渔",
        //       "房地产",
        //       "金融业",
        //       "居民服务及其他"
        //     ],
        //     yAxis: [5000, 2600, 1300, 1300, 1250, 1500]
        //   }
        // ]
      },
      columnarId: [
        // {
        //   id: "echarts04",
        //   title: "当日授信人数"
        // },
        {
          id: "echarts05",
          title: "当日用信人数"
        }
      ],

      // 右侧饼图
      pieId: "echarts06",
      pieData: [
        { name: "南京a", value: 100 },
        { name: "南京b", value: 136 },
        { name: "南京c", value: 46 },
        { name: "南京d", value: 78 },
        { name: "南京e", value: 100 }
      ],

      // 右侧漏斗图
      funnelId: "echarts07",

      funnelData: [
        { value: 100, name: "20.0%" },
        { value: 80, name: "20.2%" },
        { value: 60, name: "20.1%" },
        { value: 40, name: "20.5%" },
        { value: 20, name: "20.6%" }
      ],

      cityId: "echarts09",
      cityJudge: false,
      cityData: [],
      // cityData: [
      //   { name: "一线城市", value: 3584, total: 10000 },
      //   { name: "二线城市", value: 2218, total: 10000 },
      //   { name: "三线城市", value: 2263, total: 10000 },
      //   { name: "四线城市", value: 1806, total: 10000 },
      //   { name: "五线城市", value: 129, total: 10000 }
      //   // return { name: item.xid, value: item.val, total: total };
      // ],

      // 右下表格
      formIds: [
        { id: "consume", title: "星座&剁手" },
        { id: "scale", title: "地域&贷款规模" }
      ],

      elite: [{ id: "elite", title: "精英扶贫文字稿" }],

      // formData: {
      //   // 星座消费
      //   consume: [
      //     { name: "猴子", value: "345" },
      //     { name: "兔子", value: "335" },
      //     { name: "老虎", value: "325" },
      //     { name: "螃蟹", value: "245" },
      //     { name: "蝎子", value: "145" }
      //   ],

      //   // 地域贷款规模
      //   scale: [
      //     { name: "浙江", value: "345" },
      //     { name: "广东", value: "335" },
      //     { name: "山东", value: "325" },
      //     { name: "北京", value: "245" },
      //     { name: "江苏", value: "145" }
      //   ]
      // },

      eliteData: [],
      eliteJudge: false,
      // eliteData: [
      //   {
      //     icon: Money,
      //     indicator: "累计放款金额",
      //     value: "144",
      //     unit: "亿元"
      //   },
      //   {
      //     icon: User,
      //     indicator: "用信用户数",
      //     value: "531,014",
      //     unit: "人"
      //   },
      //   {
      //     icon: Avg,
      //     indicator: "笔均",
      //     value: "5,028",
      //     unit: "元"
      //   },
      //   {
      //     icon: Avg,
      //     indicator: "户均",
      //     value: "27,196",
      //     unit: "元"
      //   },
      //   {
      //     icon: Life,
      //     indicator: "平均用信周期",
      //     value: "147",
      //     unit: "天"
      //   }
      // ],
      consume: undefined,
      scale: undefined,

      // 年龄/学历
      ageData: undefined,
      ageJudge: false,
      educationJudge: false,
      educationData: undefined,

      // 收入、笔均金额、用信天数
      income: undefined,
      incomeJudge: false,
      incomeId: "echarts10",

      avgNumberAvg: "",
      avgNumber: undefined,
      avgNumberJudge: false,
      avgNumberId: "echarts11",
      creditDateAvg: "",
      creditDate: undefined,
      creditDateJudge: false,
      creditDateId: "echarts12",

      // 伪造数据
      mockData: {
        leftTop: [
          "89,181,516.35",
          "67,342,516.35",
          "34,181,146.35",
          "12,181,516.35"
        ],
        totalCoin: "89,181,516.35",
        indicatorData: [15984981.56, 898498781.56, 35981981.56]
      }
    };
  },
  computed: {
    getChartData() {
      return this.id.reduce((pre, item, index) => {
        pre.push(this.allData[index]);
        return pre;
      }, []);
    }
  },
  methods: {
    scroll(num, refs) {
      if (refs === undefined || Object.keys(refs).length === 0) return;
      let con1 = refs.rollul;
      if (con1 === undefined) return;
      con1[0].style.marginTop = "-30px";
      this.animate = !this.animate;

      var that = this;
      setTimeout(function() {
        that.Totallist = [num, num];
        con1[0].style.marginTop = "12px";
        that.animate = !that.animate;
      }, 10);
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

    getLineData() {
      var _that = this;
      this.axios({
        url: "/api/p1/loanTrend",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var cdata = data.data.data;
          _that.leftLineData = {};
          _that.leftLineData.amt_cur = cdata.map(item => {
            return item.amt_cur;
          });
          // _that.leftLineData.amtavg = cdata.map(item => {
          //   return item.amtavg;
          // });
          _that.leftLineData.data_dt = cdata.map(item => {
            return item.event_hour + "时";
          });
          this.$nextTick(() => {
            this.leftLineJudge = true;
          });
        }
      });
    },

    drawLineD(id, data) {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById(id));
      // 绘制图表
      var a = data.chartName || "交易银行保证金存款产品结构分析";
      var b = data.filterType || "2019年01月25日 ";
      myChart.setOption({
        title: {
          text: [`{a|${a}  }`, `{b|${b}}`].join(""),
          textStyle: {
            rich: {
              b: {
                color: "gray",
                fontSize: "14"
              }
            }
          },
          itemGap: 10,
          margin: 10
        },
        color: ["#2F8156", "#BFAA3F", "#3B63AB"],
        legend: {
          data: data.legend,
          align: "left",
          top: 30,
          itemHeight: 8,
          itemWidth: 8
        },
        tooltip: {
          trigger: "axis",
          formatter: function(option) {
            var str = "<div class='echart-shadow tool-tip'>";
            for (var i = 0, l = option.length; i < l; i++) {
              str +=
                i % 2 == 0
                  ? '<span style="color:#000">' +
                    option[i].name +
                    "</span>" +
                    "<br/>" +
                    `<span style="color: ${option[i].color}" >` +
                    option[i].seriesName +
                    ": " +
                    option[i].value +
                    "亿" +
                    "</span>" +
                    "<br/>"
                  : `<span style="color: ${option[i].color}" >` +
                    option[i].seriesName +
                    ": " +
                    option[i].value +
                    "亿" +
                    "</span>";
            }
            return str + "</div>";
          }
        },
        grid: {
          top: 160,
          bottom: 60,
          left: "10%",
          right: "10%",
          borderWidth: 1
        },
        xAxis: {
          type: "category",
          data: data.xAxis || [
            "进出口代付",
            "进口信用证",
            "口信用余额",
            "保兑仓",
            "进口押汇",
            "出口押汇",
            "融资性保函",
            "非融资性保函"
          ]
        },
        yAxis: {
          name: (data.yAxis && data.yAxis[0]) || "保证金存款产品",
          nameGap: "80",
          nameTextStyle: {
            width: "200",
            height: "200",
            backgroundColor: {
              image: "../assets/images/logo.png"
            }
          },
          splitLine: {
            show: false
          },
          axisLine: { show: false },
          axisTick: { show: false },
          axisLabel: {
            formatter: "{value}亿"
          }
        },
        series: [
          {
            name: (data.legend && data.legend[0].name) || "贷款余额",
            type: "bar",
            stack: "余额",
            label: {
              normal: {
                show: true,
                position: "inside",
                color: "#000",
                formatter: "{c}亿"
              }
            },
            data: ["5", "20", "36", "10", "10", "20", "36", "20"]
          },
          {
            name: (data.legend && data.legend[1].name) || "全行贷款余额",
            type: "bar",
            stack: "余额",
            label: {
              normal: {
                show: true,
                position: "inside",
                color: "#000",
                formatter: "{c}亿"
              }
            },
            data: ["5", "20", "36", "10", "10", "20", "36", "79"]
          },
          {
            name: (data.legend && data.legend[2].name) || "贷款客户数",
            type: "bar",
            stack: "余额",
            label: {
              normal: {
                show: true,
                position: "inside",
                color: "#000",
                formatter: "{c}亿"
              }
            },
            data: ["5", "20", "36", "10", "10", "20", "36", "79"]
          }
        ]
      });
    },
    getMsgFormSon(data) {
      this.msgFormSon = data;
      // console.log(this.msgFormSon);
    },

    // 5s请求一次数据
    rollData() {
      this.getRollDataHandler();
      var _that = this;
      var index = 0;
      // if (coinInterval) clearInterval(coinInterval);
      this.coinInterval = setInterval(
        () => {
          this.getRollDataHandler();
        },
        5000,
        _that,
        index
      );
    },

    // 30s一次请求数据
    rollTimelyData() {
      this.getTimelyData();
      var _that = this;
      var index = 0;
      // if (coinInterval) clearInterval(coinInterval);
      this.coinInterval = setInterval(
        () => {
          this.getTimelyData();
        },
        30000,
        _that,
        index
      );
    },

    getRollDataHandler() {
      var _that = this;
      // 获取累计信贷服务金额
      this.axios({
        url: "/api/p1/accCredAmt",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          _that.totalMoney = "¥" + _that.thousandFormat(data.data.data, 0) || 0;
          _that.threeMoneyArr = Object.assign({}, _that.threeMoneyArr, {
            first: _that.thousandFormat(data.data.data, 0) || 0
          });
          if (_that.totalMoney !== _that.preTotalMoney) {
            _that.scroll(_that.totalMoney, _that.$refs);
            _that.preTotalMoney = _that.totalMoney;
          }
          // _that.testN = _that.testN + 84;
        }
      });

      // 获取当日信贷服务金额
      this.axios({
        url: "/api/p1/currCredAmt",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          _that.threeMoneyArr = Object.assign({}, _that.threeMoneyArr, {
            second: _that.thousandFormat(data.data.data, 0) || 0
          });
        }
      });

      // 获取当日信贷服务人数
      this.axios({
        url: "/api/p1/accCredNum",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          _that.threeMoneyArr = Object.assign({}, _that.threeMoneyArr, {
            third: _that.thousandFormat(data.data.data, 0) || 0
          });
        }
      });

      // 获取当日授信人数
      this.axios({
        url: "/api/p1/currCustNum",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          var tData = data.data.data;
          // _that.columnarData.totalData[0] = tData[0] ? tData[0].val : 0;
          var tmpTotal = 0;
          _that.columnarData.columnAllData[0] = {
            xAxis: tData
              ? tData.map(item => {
                  return item.event_hour;
                })
              : [],
            yAxis: tData
              ? tData.map(item => {
                  tmpTotal += parseInt(item.shouxin);
                  return item.shouxin;
                })
              : []
          };
          var tmpArr = [tmpTotal, _that.columnarData.totalData[1]];
          _that.columnarData.totalData = [].concat(tmpArr);
        }
      });
      // 获取当日用信人数
      this.axios({
        url: "/api/p1/currCustNum",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          var tData = data.data.data;
          // _that.columnarData.totalData[1] = tData[0] ? tData[0].val : 0;
          var tmpTotal = 0;
          _that.columnarData.columnAllData[1] = {
            xAxis: tData
              ? tData.map(item => {
                  return item.event_hour;
                })
              : [],
            yAxis: tData
              ? tData.map(item => {
                  tmpTotal += parseInt(item.yongxin);
                  return item.yongxin;
                })
              : []
          };
          var tmpArr = [_that.columnarData.totalData[0], tmpTotal];
          _that.columnarData.totalData = [].concat(tmpArr);
          this.$nextTick(() => {
            _that.currentPersonJudge = true;
          });
        }
      });
    },

    // 时间较久数据
    getTimelyData() {
      var _that = this; // 获取星座排行
      this.axios({
        url: "/api/p1/rank?flag=xz",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          var tData = data.data.data;
          // _that.formData.consume =
          //   tData.filter(item => {
          //     return parseInt(item.ranking) <= 5;
          //   }) || [];
          //_that.consume =
          //  tData.filter(item => {
          //    return parseInt(item.ranking) <= 5;
          //  }) || [];

          // _that.consume.sort((a, b) => {
          //  return a.ranking - b.ranking;
          //});

          // _that.consume = _that.consume.slice(0, 5);

          var tData = data.data.data;
          let xAxis = [];
          let yAxis = [];
          tData.forEach((item, index) => {
            xAxis.push("Top" + (index + 1) + " " + item.xid);
            yAxis.push(parseFloat(item.score));
          });
          xAxis = xAxis.slice(0, 5).reverse();
          yAxis = yAxis.slice(0, 5).reverse();

          _that.consume = Object.assign(
            {},
            { xAxis: xAxis, yAxis: yAxis, chartType: "value" }
          );
        }
      });

      // 获取地域排行
      this.axios({
        url: "/api/p1/rank?flag=region",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          var tData = data.data.data;
          // _that.formData.scale =
          //   tData.filter(item => {
          //     return parseInt(item.ranking) <= 5;
          //   }) || [];
          // _that.formData.scale.sort((a, b) => {
          //   return a - b < 0;
          // });

          //_that.scale =
          //  tData.filter(item => {
          //    return parseInt(item.ranking) <= 5;
          //  }) || [];

          // _that.scale.sort((a, b) => {
          //   return a.ranking - b.ranking;
          // });
          //_that.scale = _that.scale.slice(0, 5);

          var tData = data.data.data;
          let xAxis = [];
          let yAxis = [];
          tData.forEach((item, index) => {
            xAxis.push("Top" + (index + 1) + " " + item.xid);
            yAxis.push(parseFloat(item.score));
          });
          xAxis = xAxis.slice(0, 5).reverse();
          yAxis = yAxis.slice(0, 5).reverse();

          _that.scale = Object.assign(
            {},
            { xAxis: xAxis, yAxis: yAxis, chartType: "value" }
          );
        }
      });

      // 获取年龄数据
      this.axios({
        url: "/api/p1/distribution?dcode=age",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // var tData = data.data.data;
          //if (tData == null) return;
          //var tmpVal = 100;
          //_that.ageData = tData.map((item, index) => {
          //  return {
          //    value: tmpVal - (100 / tData.length) * index,
          //    name: _that.thousandFormat(item.val, 0) + "  人",
          //    num: item.xid
          //  };
          //});

          var tData = data.data.data;
          var total = 0;
          tData.forEach(item => {
            total += Number(item.val);
          });
          _that.ageData = tData.map(item => {
            return { name: item.xid, value: item.val };
          });

          // let xAxis = [];
          // let yAxis = [];

          // tData.forEach(item => {
          //   xAxis.push(item.xid);
          //   yAxis.push(((parseFloat(item.val) / total) * 100).toFixed(1));
          // });
          // xAxis = xAxis.reverse();
          // yAxis = yAxis.reverse();

          // _that.ageData = Object.assign({}, { xAxis: xAxis, yAxis: yAxis });
        }
        this.$nextTick(() => {
          _that.ageJudge = true;
        });
      });

      // 获取学历数据
      this.axios({
        url: "/api/p1/education",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var tData = data.data.data;
          // var total = 0;
          // tData.forEach(item => {
          //   total += Number(item.percent);
          // });
          _that.educationData = tData.map(item => {
            // return { name: item.education, value: item.percent, total: total };
            return { name: item.education, value: item.percent };
          });
        }
        this.$nextTick(() => {
          _that.educationJudge = true;
        });
      });

      // 获取复购数据
      this.axios({
        url: "/api/p1/credBasic",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100 || data.data.data !== null) {
          var tData = data.data.data;
          // 六个指标
          _that.allDataIndicator = [
            {
              name: "余额",
              index: "amount",
              data: tData ? "¥ " + _that.thousandFormat(tData.bal, 2) : ""
            },
            {
              name: "笔均",
              index: "avg",
              data: tData ? "¥ " + tData.bijun : ""
            },
            {
              name: "户均",
              index: "savg",
              data: tData ? "¥ " + tData.hujun : ""
            },
            {
              name: "平均放款期限（月）",
              index: "avg-deadline",
              data: tData ? tData.tenor : ""
            },
            {
              name: "平均用信天数（天）",
              index: "loan-time",
              data: tData ? tData.avgday : ""
            },
            {
              name: "平均用信次数",
              index: "rate",
              data: tData ? tData.avgnum : ""
            }
          ];
          _that.linkRelativeRatio = {
            echarts02: [
              {
                title: "环比上周",
                data: tData ? tData.reloan30ratehuanbi : "",
                type: tData
                  ? Number(tData.reloan30ratehuanbi) > 0
                    ? "positive"
                    : "negative"
                  : "positive"
              }
              // {
              //   title: "环比上年",
              //   data: "+15.19%",
              //   type: "positive"
              // }
            ],
            echarts03: [
              {
                title: "环比上周",
                data: tData ? tData.reloan90ratehuanbi : "",
                type: tData
                  ? Number(tData.reloan90ratehuanbi) > 0
                    ? "positive"
                    : "negative"
                  : "positive"
              }
              // {
              //   title: "环比上年",
              //   data: "+10.19%",
              //   type: "positive"
              // }
            ]
          };
          // 复购
          _that.dialPlate = [
            tData ? tData.reloan30rate : "",
            tData ? tData.reloan90rate : ""
          ];
          _that.oilPieData = Object.assign({}, _that.oilPieData, {
            oilCharts01: tData ? tData.ddsecond : "",
            oilCharts02: tData ? tData.reloan90rate * 100 : ""
          });
          this.$nextTick(() => {
            _that.plateJudge = true;
          });
        }
      });

      // 获取复购折线图数据
      this.axios({
        url: "/api/p1/credBasicTrend",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100 || data.data.data !== null) {
          var tData = data.data.data;
          _that.lineData = [
            {
              xAxis: tData.map(item => {
                return item.data_dt;
              }),
              yAxis: tData.map(item => {
                return item.reloan30rate * 100;
              })
            },
            {
              xAxis: tData.map(item => {
                return item.data_dt;
              }),
              yAxis: tData.map(item => {
                return item.reloan90rate * 100;
              })
            }
          ];
          // [
          //     {
          //       city: ["0", "4", "8", "12", "16", "20"],
          //       num: ["40", "60", "22", "85", "50", "40"]
          //     },
          //     {
          //       city: ["0", "4", "8", "12", "16", "20"],
          //       num: ["40", "60", "22", "85", "50", "40"]
          //     }
          //   ]
        }
      });

      // 精准扶贫
      this.axios({
        url: "/api/p1/precise",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100 || data.data.data !== null) {
          var tData = data.data.data;
          _that.eliteData = tData.map(item => {
            let IconFont = undefined;
            switch (item.icon) {
              case "Money":
                IconFont = Money;
                break;
              case "User":
                IconFont = User;
                break;
              case "Avg":
                IconFont = Avg;
                break;
              case "Life":
                IconFont = Life;
                break;
              default:
                break;
            }
            let formatArr = ["笔均", "户均", "累计放款金额"];
            return {
              icon: IconFont,
              indicator: item.indicator,
              value:
                formatArr.indexOf(item.indicator) > -1
                  ? _that.thousandFormat(item.value, 0)
                  : item.value,
              unit: item.unit
            };
          });
          this.$nextTick(() => {
            _that.eliteJudge = true;
          });
        }
      });

      // 地域分布
      this.axios({
        url: "/api/p1/area",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100 || data.data.data !== null) {
          var tData = data.data.data;

          // let total = tData.reduce((p, e) => p + e.val, 0);
          // _that.cityData = tData.map(item => {
          //   // let IconFont = undefined;
          //   // switch (item.icon) {
          //   //   case "Money":
          //   //     IconFont = Money;
          //   //     break;
          //   //   case "User":
          //   //     IconFont = User;
          //   //     break;
          //   //   case "Avg":
          //   //     IconFont = Avg;
          //   //     break;
          //   //   case "Life":
          //   //     IconFont = Life;
          //   //     break;
          //   //   default:
          //   //     break;
          //   // }
          //   // let formatArr = ["笔均", "户均", "累计放款金额"];
          //   return {
          //     name: item.regionname,
          //     value: item.percent,
          //     total: total
          //   };

          //   //        cityData: [
          //   //   { name: "一线城市", value: 3584, total: 10000 },
          //   //   { name: "二线城市", value: 2218, total: 10000 },
          //   //   { name: "三线城市", value: 2263, total: 10000 },
          //   //   { name: "四线城市", value: 1806, total: 10000 },
          //   //   { name: "五线城市", value: 129, total: 10000 }
          //   //   // return { name: item.xid, value: item.val, total: total };
          //   // ],
          // });

          let xAxis = [];
          let yAxis = [];
          tData.forEach(item => {
            xAxis.push(item.regionname);
            yAxis.push(item.percent);
          });
          xAxis = xAxis.slice(0, 5);
          yAxis = yAxis.slice(0, 5);

          _that.cityData = Object.assign({}, { xAxis: xAxis, yAxis: yAxis });

          this.$nextTick(() => {
            _that.cityJudge = true;
          });
        }
      });

      // 收入、笔均金额、用信天数
      this.axios
        .all([
          this.axios.get("/api/p1/distributeInfo?category=ph_sr&flag=ph"),
          this.axios.get("/api/p1/distributeInfo?category=ph_fkje&flag=ph"),
          this.axios.get("/api/p1/distributeInfo?category=ph_yxts&flag=ph")
        ])
        .then(
          this.axios.spread((...obj) => {
            // 收入
            let data1 = obj[0].data.data;
            let xAxis1 = [],
              yAxis1 = [];
            data1.forEach(item => {
              xAxis1.push(item.key);
              yAxis1.push(item.perc);
            });
            // xAxis1 = xAxis1.reverse();
            // yAxis1 = yAxis1.reverse();
            _that.income = Object.assign({}, { xAxis: xAxis1, yAxis: yAxis1 });

            // 笔均金额
            let data2 = obj[1].data.data;
            let xAxis2 = [],
              yAxis2 = [];
            data2.forEach(item => {
              xAxis2.push(item.key);
              yAxis2.push(item.perc);
            });
            // xAxis2 = xAxis2.reverse();
            // yAxis2 = yAxis2.reverse();
            let avgNumberAvg = "";
            if (xAxis2[0] == "均值") {
              xAxis2.shift();
              avgNumberAvg = this.thousandFormat(yAxis2.shift(), 2);
            }
            _that.avgNumberAvg = avgNumberAvg;
            _that.avgNumber = Object.assign(
              {},
              { xAxis: xAxis2, yAxis: yAxis2 }
            );

            // 笔均金额
            let data3 = obj[2].data.data;
            let xAxis3 = [],
              yAxis3 = [];
            data3.forEach(item => {
              xAxis3.push(item.key);
              yAxis3.push(item.perc);
            });
            // xAxis3 = xAxis3.reverse();
            // yAxis3 = yAxis3.reverse();
            let creditDateAvg = "";
            if (xAxis3[0] == "均值") {
              xAxis3.shift();
              creditDateAvg = parseInt(yAxis3.shift()) + "天";
            }
            _that.creditDateAvg = creditDateAvg;
            _that.creditDate = Object.assign(
              {},
              { xAxis: xAxis3, yAxis: yAxis3 }
            );

            this.$nextTick(() => {
              this.incomeJudge = true;
              this.avgNumberJudge = true;
              this.creditDateJudge = true;
            });
          })
        )
        .catch(function(error) {
          console.log(error);
        });

      // 授信、用信时长
      this.axios
        .all([
          this.axios.get("/api/p1/randValue?flag=sxsc"),
          this.axios.get("/api/p1/randValue?flag=dksc")
        ])
        .then(
          this.axios.spread((...obj) => {
            // 授信时长
            this.extensionDate = obj[0].data.data;

            // 贷款时长
            this.loanDate = obj[1].data.data;

            this.$nextTick(() => {
              this.extensionDateJudge = true;
              this.loanDateJudge = true;
            });
          })
        )
        .catch(function(error) {
          console.log(error);
        });
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

    getMapDataHandler() {
      var _that = this;
      // 获取地图数据
      this.axios({
        url: "/api/p1/mapinfo?count=20",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var tData = data.data.data;
          _that.mapData = [
            ...tData.map(item => {
              return {
                name: item.name,
                age: item.age,
                sex: item.sex,
                type: item.trade_type,
                sum: item.trade_amount,
                inbusshopaddress: item.inbusshopaddress,
                value: [item.longitude, item.latitude, item.score]
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
  beforeDestroy() {
    clearInterval(this.mapInterval);
    clearInterval(this.coinInterval);
  },
  components: {
    "line-chart": LineChart,
    "sec-chart": SecChart,
    "columnar-chart": ColumnarChart,
    "map-chart": MapChart,
    "weather-com": WeatherCom,
    "indicator-div": IndicatorDiv,
    "cyclic-annular": CyclicAnnular,
    "whole-pie-chart": WholePieChart,
    "simple-column": SimpleColumn
    // "left-indicator-chart": LeftIndicatorChart,
    // "oil-pie-chart": OilPieChart,
    // "indicator-chart": IndicatorChart,
    // "repeat-purchase": RepeatPurchase,
    // "pie-chart": PieChart,
    // "funnel-chart": FunnelChart,
    // "form-chart": FormChart,
    // "scroll-span": ScrollSpan,
    // "person-columnar": PersonColumnar,
    // "income-level": IncomeLevel,
  }
};
</script>

<style lang="less" scoped>
.container {
  width: 4224px;
  height: 1536px;
  // width: 100%;
  // height: 100%;
  background: url("../../assets/images/material/exports/background.png")
    no-repeat;
  background-size: 100% 100%;
  // padding-top: 1%;
  font-size: 28px;
  color: #fff;
  overflow: hidden;
  font-family: Microsoft YaHei;

  .total-money-span {
    color: #ffcc22;
    height: 37%;
    margin-top: 0.4%;
  }

  .anim {
    transition: all 1s;
  }
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
      .left-coin {
        color: #ffcc22;
      }
      .title-left-span {
        display: inline-block;
        float: left;
        // position: relative;
        // top: -44%;
      }
    }
    .global-title {
      width: 33%;
      text-align: center;
      font-size: 64px;
      font-weight: bold;
      font-family: FZZhengHeiS-B-GB;
    }
    .right-time {
      width: 33%;
      text-align: right;
    }
  }

  // 内容样式
  .frame-content {
    display: flex;
    // padding: 0 40px;
    justify-content: space-between;
    .accruing-amounts,
    .accruing-person {
      width: 27%;
      height: 92.96%;
      padding: 40px;
      background: rgba(22, 28, 40, 0.32);
      // background: url("../../assets/images/p3/bg-cont.png") no-repeat;
      // background-size: 100% 97%;
      .single-distribution {
        width: 50%;
        height: 100%;
        text-align: left;
        position: relative;
        .single-graph-title {
          // position: absolute;
          // left: 0;
          // top: 0;
          font-weight: bold;
          font-size: 32px;
          line-height: 40px;
          letter-spacing: 0px;
          .avg-title {
            display: inline-block;
            margin-left: 24px;
            color: rgba(255, 255, 255, 0.7);
            font-family: Microsoft YaHei;
            font-weight: bold;
            font-size: 24px;
            line-height: 40px;
            letter-spacing: 0px;
            text-align: left;
          }
          .avg-num {
            display: inline-block;
            margin-left: 24px;
            color: #49e5fa;
            font-family: Microsoft YaHei;
            font-weight: bold;
            font-size: 8px;
            line-height: 40px;
            letter-spacing: 0px;
            text-align: left;
          }
        }
        .basic-chart {
          height: 87%;
          width: 100%;
        }
      }
    }
    .current-amounts {
      width: 45%;
      height: 100%;
      .map-charts {
        width: 100%;
        height: 1200px;
      }
      .middle-digital-span {
        margin-top: 38px;
      }
    }
    .content-title {
      background: url("../../assets/images/fs-title-bg.png") no-repeat;
      background-size: 100% 100%;
      width: 40%;
      height: 15%;
      margin: 0 auto;
      text-align: center;
    }
    .accruing-amounts {
      .line-chart {
        // height: 500px;
        height: 90%;
        padding-top: 2%;
      }
      .bottom-title-div {
        display: flex;
        justify-content: space-between;
        padding-top: 20px;
        .bottom-title {
          padding-left: 40px;
          width: 50%;
          height: 20px;
        }
      }
      .person-columnar {
        height: 530px;
        padding-left: 30px;
        display: inline-block;
        width: 50%;
        &:first-child {
          margin-right: 5%;
        }
      }
      .right-top-distribution {
        width: 100%;
        height: 320px;
        margin-top: 48px;
      }
      .right-middle-div,
      .right-bottom-div {
        display: flex;
      }
      .half-middle-distribution {
        width: 50%;
        height: 332px;
        margin-top: 40px;
      }
      .half-bottom-distribution {
        height: 408px;
        width: 50%;
        margin-top: 40px;
      }
    }
    .accruing-person {
      .columnar-chart {
        margin-top: 48px;
        height: 40%;
        margin-top: 48px;
      }
      .distribution {
        display: flex;
        height: 40%;
        padding-bottom: 20px;

        .left-distribution {
          height: 400px;
        }
        .right-distribution {
          height: 400px;
        }

        &:first-child {
          padding-bottom: 40px;
        }
      }
      .middle-distirbution {
        height: 374px;
        margin-top: 48px;
      }
      .bottom-distribution {
        height: 408px;
      }
      .form-charts {
        width: 50%;
        height: 400px;
      }
    }
  }
}
</style>