<template>
  <div class="container">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="title-left">
        <span class="title-left-span">累计信贷服务金额：</span>
        <!-- <div class="nwwest-roll" id="nwwest-roll">
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
        </div>-->

        <scroll-span :number="totalMoney" class="total-money-span" ids="total" />
        <!-- <span class="left-coin">¥ {{totalMoney}}</span> -->
      </div>
      <div class="global-title">普惠业务信贷数据平台</div>
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
      <div class="accruing-amounts">
        <div class="content-title">累计信贷服务金额</div>
        <indicator-chart
          v-if="threeMoneyArr.first"
          :chartData="threeMoneyArr.first"
          chartId="total-money"
        />
        <!-- <indicator-chart v-if="testN" :chartData="testN" chartId="total-money" /> -->
        <line-chart
          :ids="id"
          v-if="leftLineData"
          :chartData="getChartData"
          :trueData="leftLineData"
          class="line-chart"
        />
        <left-indicator-chart v-if="allDataIndicator" :chartData="allDataIndicator" />
        <repeat-purchase
          :ids="repeatPurchaseId"
          :v-if="plateJudge"
          :chartData="{linkRelativeRatio: linkRelativeRatio,dialPlate: dialPlate,lineData: lineData}"
          class="repeat-purchase-chart"
        />
      </div>
      <div class="current-amounts">
        <div class="content-title">当日信贷服务金额</div>
        <indicator-chart
          v-if="threeMoneyArr.second"
          :chartData="threeMoneyArr.second"
          chartId="current-money"
        />

        <!-- 地图 -->
        <map-chart v-if="mapData" :chartData="mapData" class="map-charts" />
      </div>
      <div class="accruing-person">
        <div class="content-title">累计信贷服务人数</div>
        <indicator-chart
          v-if="threeMoneyArr.third"
          :chartData="threeMoneyArr.third"
          type="person"
          chartId="total-person"
        />
        <columnar-chart
          :ids="columnarId"
          v-if="currentPersonJudge"
          :chartData="columnarData"
          class="columnar-chart"
        />
        <div class="distribution">
          <funnel-chart
            class="left-distribution single-distribution"
            :ids="funnelId"
            v-if="educationJudge"
            :chartData="ageData"
          />
          <pie-chart
            class="right-distribution single-distribution"
            :ids="pieId"
            v-if="educationJudge"
            :chartData="educationData"
          />
        </div>
        <form-chart
          :ids="formIds"
          v-if="consume"
          :chartData="{consume: consume, scale: scale}"
          class="form-charts"
        />
      </div>
    </div>
    <!-- <div class="title-frame"></div> -->
  </div>
</template>

<script>
import IndicatorChart from "../../components/first-screen/indicator.vue";
import LineChart from "../../components/first-screen/lineChart.vue";
import LeftIndicatorChart from "../../components/first-screen/leftIndicator.vue";
import RepeatPurchase from "../../components/first-screen/repeatPurchase.vue";
import ColumnarChart from "../../components/first-screen/columnarChart.vue";
import PieChart from "../../components/first-screen/pieChart.vue";
import FunnelChart from "../../components/first-screen/funnelChart.vue";
import FormChart from "../../components/first-screen/formChart.vue";
import MapChart from "../../components/first-screen/mapChart.vue";
import WeatherCom from "../../components/weather.vue";

import ScrollSpan from "../../components/scrollSpan.vue";

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
      mapData: undefined,

      // 当日人数
      currentPerson: undefined,
      currentPersonJudge: false,

      // 左侧实时放款数据
      allData: [
        {
          chartName: "当日实时放款/七日均值",
          money: Math.floor(Math.random() * 100000)
        }
      ],
      leftLineData: undefined,
      id: ["echarts01"],

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
          id: "echarts02",
          title: "30天复购"
        },
        {
          id: "echarts03",
          title: "90天复购"
        }
      ],

      // 右侧柱状图数据
      columnarData: {
        totalData: [],
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
        {
          id: "echarts04",
          title: "当日授信人数"
        },
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

      // 右下表格
      formIds: [
        { id: "consume", title: "星座&剁手" },
        { id: "scale", title: "地域&贷款规模" }
      ],
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

      consume: undefined,
      scale: undefined,

      // 年龄/学历
      ageData: undefined,
      ageJudge: false,
      educationJudge: false,
      educationData: undefined,
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
          _that.leftLineData.amtavg = cdata.map(item => {
            return item.amtavg;
          });
          _that.leftLineData.data_dt = cdata.map(item => {
            return item.data_dt;
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
      console.log(this.msgFormSon);
    },

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
          _that.totalMoney = "¥" + _that.thousandFormat(data.data.data, 2) || 0;
          _that.threeMoneyArr = Object.assign({}, _that.threeMoneyArr, {
            first: data.data.data.toString() || 0
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
            second: data.data.data.toString() || 0
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
            third: data.data.data.toString() || 0
          });
        }
      });

      // 获取当日授信人数
      this.axios({
        url: "/api/p1/currCustNum?eventType=授信",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          var tData = data.data.data;
          _that.columnarData.totalData[0] = tData[0] ? tData[0].val : 0;
          _that.columnarData.columnAllData[0] = {
            xAxis: tData
              ? tData.map(item => {
                  return item.event_hour;
                })
              : [],
            yAxis: tData
              ? tData.map(item => {
                  return item.val;
                })
              : []
          };
        }
      });
      // 获取当日用信人数
      this.axios({
        url: "/api/p1/currCustNum?eventType=用信",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          // _that.currentMoney = _that.thousandFormat(data.data.data, 2) || 0;
          var tData = data.data.data;
          _that.columnarData.totalData[1] = tData[0] ? tData[0].val : 0;
          _that.columnarData.columnAllData[1] = {
            xAxis: tData
              ? tData.map(item => {
                  return item.event_hour;
                })
              : [],
            yAxis: tData
              ? tData.map(item => {
                  return item.val;
                })
              : []
          };
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
          _that.consume =
            tData.filter(item => {
              return parseInt(item.ranking) <= 5;
            }) || [];

          _that.consume.sort((a, b) => {
            return a.ranking - b.ranking;
          });

          _that.consume = _that.consume.slice(0, 5);
        }
      });

      // 获取星座排行
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

          _that.scale =
            tData.filter(item => {
              return parseInt(item.ranking) <= 5;
            }) || [];

          _that.scale.sort((a, b) => {
            return a.ranking - b.ranking;
          });
          _that.scale = _that.scale.slice(0, 5);
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
          var tData = data.data.data;
          if (tData == null) return;
          var tmpVal = 100;
          _that.ageData = tData.map((item, index) => {
            debugger;
            return {
              value: tmpVal - (100 / tData.length) * index,
              name: _that.thousandFormat(item.val, 0) + "  人",
              num: item.xid
            };
          });
        }
        this.$nextTick(() => {
          _that.ageJudge = true;
        });
      });

      // 获取学历数据
      this.axios({
        url: "/api/p1/distribution?dcode=education",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var tData = data.data.data;
          var total = 0;
          tData.forEach(item => {
            total += Number(item.val);
          });
          _that.educationData = tData.map(item => {
            return { name: item.xid, value: item.val, total: total };
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
              data: tData ? tData.bal : ""
            },
            {
              name: "笔均",
              index: "avg",
              data: tData ? tData.bijun : ""
            },
            {
              name: "户均",
              index: "savg",
              data: tData ? tData.hujun * 100 + "%" : ""
            },
            {
              name: "平均期限",
              index: "avg-deadline",
              data: tData ? tData.tenor : ""
            },
            {
              name: "贷款时长",
              index: "loan-time",
              data: tData ? tData.ddsecond : ""
            },
            {
              name: "授信成功率",
              index: "rate",
              data: tData ? tData.applyrate * 100 + "%" : ""
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
        10000,
        _that,
        index
      );
    },

    getMapDataHandler() {
      var _that = this;
      // 获取地图数据
      this.axios({
        url: "/api/p1/mapinfo",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var tData = data.data.data;
          _that.mapData = tData.map(item => {
            return {
              name: item.name,
              age: item.age,
              sex: item.sex,
              type: item.trade_type,
              sum: item.trade_amount,
              value: [item.longitude, item.latitude, item.score]
            };
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
    "indicator-chart": IndicatorChart,
    "line-chart": LineChart,
    "left-indicator-chart": LeftIndicatorChart,
    "repeat-purchase": RepeatPurchase,
    "columnar-chart": ColumnarChart,
    "pie-chart": PieChart,
    "funnel-chart": FunnelChart,
    "form-chart": FormChart,
    "map-chart": MapChart,
    "weather-com": WeatherCom,
    "scroll-span": ScrollSpan
  }
};
</script>

<style lang="less" scoped>
.container {
  width: 4224px;
  height: 1536px;
  // width: 100%;
  // height: 100%;
  background: url("../../assets/images/background.png") no-repeat;
  background-size: 100% 100%;
  padding-top: 1%;
  font-size: 28px;
  color: #fff;
  // .nwwest-roll {
  //   display: inline-block;
  //   height: 40px;
  //   overflow: hidden;
  // }

  // .roll-ul {
  //   list-style: none;
  //   padding: 0;
  //   margin: 0;
  // }

  // .nwwest-roll li {
  //   height: 35px;
  //   line-height: 35px;
  // }
  .total-money-span {
    color: #ffcc22;
    height: 32%;
  }

  .anim {
    transition: all 1s;
  }
  // 标题样式
  .title-frame {
    width: 100%;
    height: 5%;
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
        position: relative;
        top: -44%;
      }
    }
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
  }

  // 内容样式
  .frame-content {
    display: flex;
    justify-content: space-between;
    .accruing-amounts,
    .accruing-person {
      width: 30%;
      height: 100%;
    }
    .current-amounts {
      width: 40%;
      height: 100%;
      .map-charts {
        width: 100%;
        height: 1200px;
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
        height: 400px;
        padding-top: 2%;
      }
    }
    .accruing-person {
      .columnar-chart {
        height: 30%;
      }
      .distribution {
        display: flex;
        height: 25%;
        .single-distribution {
          width: 50%;
          text-align: left;
        }
        .eft-distribution {
          height: 400px;
        }
        .right-distribution {
          height: 400px;
        }
      }
      .form-charts {
        width: 100%;
        height: 300px;
      }
    }
  }
}
</style>