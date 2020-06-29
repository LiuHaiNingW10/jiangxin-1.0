<template>
  <div class="container">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="title-left">
        <span>累计信贷服务金额：</span>
        <span class="left-coin">¥ {{mockData.totalCoin}}</span>
      </div>
      <div class="global-title">普惠业务信贷数据平台</div>
      <div class="right-time">
        <!-- <div class="weather-module">
    <img :src="currentImg" alt />
    <span class>{{currentWeather.high}} ℃ ~ {{currentWeather.low}} ℃</span>
    <span class="time-span">{{currentTime.date}}</span>
    <span class="time-span">{{currentTime.time}}</span>
  </div> -->
        <weather-com />
      </div>
    </div>

    <!-- 内容 -->
    <div class="frame-content">
      <div class="accruing-amounts">
        <div class="content-title">累计信贷服务金额</div>
        <indicator-chart :chartData="mockData.indicatorData[0]" />
        <line-chart :ids="id" :chartData="getChartData" class="line-chart" />
        <left-indicator-chart :chartData="allDataIndicator" />
        <repeat-purchase
          :ids="repeatPurchaseId"
          :chartData="repeatPurchaseData"
          class="repeat-purchase-chart"
        />
      </div>
      <div class="current-amounts">
        <div class="content-title">当日信贷服务金额</div>
        <indicator-chart :chartData="mockData.indicatorData[1]" />

        <!-- 地图 -->
        <map-chart class="map-charts" />
      </div>
      <div class="accruing-person">
        <div class="content-title">累计信贷服务人数</div>
        <indicator-chart :chartData="mockData.indicatorData[2]" />
        <columnar-chart :ids="columnarId" :chartData="columnarData" class="columnar-chart" />
        <div class="distribution">
          <funnel-chart
            class="left-distribution single-distribution"
            :ids="funnelId"
            :chartData="funnelData"
          />
          <pie-chart
            class="right-distribution single-distribution"
            :ids="pieId"
            :chartData="pieData"
          />
        </div>
        <form-chart :ids="formIds" :chartData="formData" class="form-charts" />
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
import WeatherCom from "../../components/weather.vue"
import Yin from "../../assets/images/yin.png";
import Yu from "../../assets/images/yu.png";

import { setInterval, clearInterval } from "timers";
export default {
  mounted() {
    this.rollData();
    // this.getTime();
    // this.getWeather();
    // this.getCurrentWeather();
  },
  data() {
    return {
      // 左侧实时放款数据
      allData: [
        {
          chartName: "当日实时放款/七日均值",
          money: Math.floor(Math.random() * 100000)
        }
      ],
      id: ["echarts01"],

      // 左侧六个指标数据
      allDataIndicator: [
        {
          name: "余额",
          index: "amount",
          data: 4320
        },
        {
          name: "笔均",
          index: "avg",
          data: 4320
        },
        {
          name: "户均",
          index: "savg",
          data: "50%"
        },
        {
          name: "平均期限",
          index: "avg-deadline",
          data: 4320
        },
        {
          name: "贷款时长",
          index: "loan-time",
          data: "651min"
        },
        {
          name: "授信成功率",
          index: "rate",
          data: "18%"
        }
      ],
      idIndicator: ["card1", "card2", "card3", "card4", "card5", "card6"],

      // 左侧复购数据
      repeatPurchaseData: {
        linkRelativeRatio: {
          echarts02: [
            {
              title: "环比上周",
              data: "+23.19%"
            },
            {
              title: "环比上月",
              data: "+15.19%"
            }
          ],
          echarts03: [
            {
              title: "环比上周",
              data: "+12.19%"
            },
            {
              title: "环比上月",
              data: "+10.19%"
            }
          ]
        },
        dialPlate: ["34", "89"],
        lineData: [
          {
            city: ["0", "4", "8", "12", "16", "20"],
            num: ["40", "60", "22", "85", "50", "40"]
          },
          {
            city: ["0", "4", "8", "12", "16", "20"],
            num: ["40", "60", "22", "85", "50", "40"]
          }
        ]
      },
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
        totalData: ["234,561,345", "5,234,715"],
        columnAllData: [
          {
            xAxis: [
              "制造业",
              "建筑业",
              "农林牧渔",
              "房地产",
              "金融业",
              "居民服务及其他"
            ],
            yAxis: [5000, 2600, 1300, 1300, 1250, 1500]
          },
          {
            xAxis: [
              "制造业",
              "建筑业",
              "农林牧渔",
              "房地产",
              "金融业",
              "居民服务及其他"
            ],
            yAxis: [5000, 2600, 1300, 1300, 1250, 1500]
          }
        ]
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
      formData: {
        // 星座消费
        consume: [
          { name: "猴子", value: "345" },
          { name: "兔子", value: "335" },
          { name: "老虎", value: "325" },
          { name: "螃蟹", value: "245" },
          { name: "蝎子", value: "145" }
        ],

        // 地域贷款规模
        scale: [
          { name: "浙江", value: "345" },
          { name: "广东", value: "335" },
          { name: "山东", value: "325" },
          { name: "北京", value: "245" },
          { name: "江苏", value: "145" }
        ]
      },

      // 天气
      localweather: [
        { high: "29", low: "22" },
        { high: "28", low: "23" },
        { high: "27", low: "23" },
        { high: "25", low: "23" },
        { high: "26", low: "22" },

        { high: "29", low: "22" },
        { high: "29", low: "22" }
      ],
      weatherImg: [Yin, Yu, Yu, Yu, Yu, Yu, Yu],

      currentWeather: {},
      currentImg: "",

      // 时间
      currentTime: {},
      currentDate: undefined,

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

    // 获取左上角累计金额ß
    rollData() {
      var _that = this;
      var index = 0;
      if (coinInterval) clearInterval(coinInterval);
      var coinInterval = setInterval(
        () => {
          _that.mockData.totalCoin = _that.mockData.leftTop[index];
          index++;
          if (index === _that.mockData.leftTop.length) index = 0;
        },
        2000,
        _that,
        index
      );
    },

    // 获取右上角当前时间
    getTime() {
      if (timeInterval) clearInterval(timeInterval);
      var timeInterval = setInterval(this.nowTime, 1000);
    },

    nowTime() {
      var myDate = new Date();
      var y = myDate.getFullYear();
      var M = myDate.getMonth() + 1; //获取当前月份(0-11,0代表1月)
      var d = myDate.getDate(); //获取当前日(1-31)
      var h = myDate.getHours(); //获取当前小时数(0-23)
      var m = myDate.getMinutes(); //获取当前分钟数(0-59)
      var s = myDate.getSeconds(); //获取当前秒数(0-59)

      //检查是否小于10
      M = this.check(M);
      d = this.check(d);
      h = this.check(h);
      m = this.check(m);
      s = this.check(s);
      var dateStr = y + "/" + M + "/" + d;
      var timeStr = h + ":" + m + ":" + s;
      this.currentTime = Object.assign({}, { date: dateStr, time: timeStr });
      // 获取当前天气
      this.getCurrentWeather(myDate.getDate());
      // document.getElementById("nowtime").innerHTML = "当前时间：" + timestr;
    },

    //时间数字小于10，则在之前加个“0”补位。
    check(i) {
      var num = i < 10 ? "0" + i : i;
      return num;
    },

    // 写死天气
    getCurrentWeather(d) {
      if (this.currentDate === d) {
        return;
      }
      var index = 0;
      if (d > 6) {
        this.currentWeather = this.localweather[index];
        this.currentImg = this.weatherImg[index];
      } else {
        this.currentWeather = this.localweather[d];
        this.currentImg = this.weatherImg[d];
      }
    },

    // 获取当前天气
    getWeather() {
      var _this = this;
      this.axios
        .get(
          "https://tianqiapi.com/api?version=v6&appid=71549884&appsecret=XH6bWw5A"
        )
        .then(function(response) {
          _this.localweather = response.data;
          _this.weatherImg =
            "http://tq.daodaoim.com//tianqiapi/skin/pitaya/" +
            response.data.wea_img +
            ".png";
          console.log(_this.weaterImg, "img");
        })
        .catch(() => {});

      // this.axios({
      //   url: "https://www.tianqiapi.com/api/?version=v1&cityid=101280601",
      //   method: "get",
      //   data: {
      //     results: 10
      //   },
      //   type: "json"
      // })
      //   .then(res => {
      //     // let datas = res.data.data[0];//下标为0即表示当天天气数据
      //     console.log(res.data);
      //   })
      //   .catch(err => {
      //     console.log(err);
      //   });
    }
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
    "weather-com" : WeatherCom
  }
};
</script>

<style lang="less">
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
    .time-span {
      display: inline-block;
      margin-left: 5%;
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