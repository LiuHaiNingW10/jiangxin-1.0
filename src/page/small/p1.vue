<template>
  <div class="container">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="title-left">
        <span>累计信贷服务金额：</span>
        <span class="left-coin">¥ 390,802,158.58</span>
      </div>
      <div class="global-title">普惠业务信贷数据平台</div>
      <div class="right-time">
        <span>-5 C ~ 5 C 2019/05/05 10:24:51</span>
      </div>
    </div>

    <!-- 内容 -->
    <div class="frame-content">
      <div class="accruing-amounts">
        <div class="content-title">累计信贷服务金额</div>
        <indicator-chart :chartData="mockData.indicatorData[0]" />
        <line-chart :ids="id" :chartData="getChartData" class="line-chart" />
        <left-indicator-chart :chartData="allDataIndicator"  />
      </div>
      <div class="current-amounts">
        <div class="content-title">当日信贷服务金额</div>
        <indicator-chart :chartData="mockData.indicatorData[1]" />
      </div>
      <div class="accruing-person">
        <div class="content-title">累计信贷服务人数</div>
        <indicator-chart :chartData="mockData.indicatorData[2]" />
      </div>
    </div>
    <!-- <div class="title-frame"></div> -->
  </div>
</template>

<script>
import IndicatorChart from "../../components/first-screen/indicator.vue";
import LineChart from "../../components/first-screen/lineChart.vue";
import LeftIndicatorChart from "../../components/first-screen/leftIndicator.vue";
export default {
  mounted() {},
  data() {
    return {
      allData: [
        {
          chartName: "当日实时放款/七日均值",
          money: Math.floor(Math.random() * 100000)
        }
      ],
      id: ["echarts01"],
      allDataB: [
        {
          chartName: "信贷客户数(表内)",
          yAxis: ["信贷客户数"],
          legend: [
            {
              name: "个经贷款客户数",
              icon: "roundRect"
            },
            {
              name: "鑫伙伴客户数",
              icon: "roundRect"
            },
            {
              name: "非鑫伙伴客户数",
              icon: "circle"
            }
          ],
          money: Math.floor(Math.random() * 100000),
          xAxis: [
            "2019年01月",
            "2019年02月",
            "2019年03月",
            "2019年04月",
            "2019年05月",
            "2019年06月"
          ]
        },
        {
          chartName: "信贷资产余额(表内)",
          yAxis: ["信贷资产余额"],
          legend: [
            {
              name: "个经贷款客户数",
              icon: "roundRect"
            },
            {
              name: "鑫伙伴客户数",
              icon: "roundRect"
            },
            {
              name: "非鑫伙伴客户数",
              icon: "circle"
            }
          ],
          xAxis: [
            "2019年01月",
            "2019年02月",
            "2019年03月",
            "2019年04月",
            "2019年05月",
            "2019年06月"
          ],
          money: Math.floor(Math.random() * 100000)
        }
      ],
      idB: ["echarts03", "echarts04"],
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
      allDataC: [
        {
          chartName: "信贷客户数(表内)",
          legend: [
            {
              name: "个经贷款客户数",
              icon: "roundRect"
            },
            {
              name: "鑫伙伴客户数",
              icon: "roundRect"
            },
            {
              name: "非鑫伙伴客户数",
              icon: "circle"
            }
          ],
          money: Math.floor(Math.random() * 100000)
        },
        {
          chartName: "信贷资产余额(表内)",
          money: Math.floor(Math.random() * 100000),
          legend: [
            {
              name: "个经贷款客户数",
              icon: "roundRect"
            },
            {
              name: "鑫伙伴客户数",
              icon: "roundRect"
            },
            {
              name: "非鑫伙伴客户数",
              icon: "circle"
            }
          ]
        }
      ],
      idC: ["echarts05", "echarts06"],
      filterParams: {
        org: [
          { key: "1", value: "鼓楼支行" },
          { key: "2", value: "江宁区支行" },
          { key: "3", value: "建邺区支行" },
          { key: "4", value: "浦口区支行" },
          { key: "5", value: "六合区支行" },
          { key: "6", value: "雨花台支行" }
        ],
        time: [
          { key: "01", value: "2019年01月25日" },
          { key: "02", value: "2019年02月25日" },
          { key: "03", value: "2019年03月25日" },
          { key: "04", value: "2019年04月25日" },
          { key: "05", value: "2019年05月25日" },
          { key: "06", value: "2019年06月25日" }
        ]
      },
      mockData: {
        leftTop: [
          "89,181,516.35",
          "67,342,516.35",
          "34,181,146.35",
          "12,181,516.35"
        ],
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
    }
  },
  components: {
    "indicator-chart": IndicatorChart,
    "line-chart": LineChart,
    "left-indicator-chart": LeftIndicatorChart
  }
};
</script>

<style lang="less">
.container {
  width: 4224px;
  height: 1536px;
  background: url("../../assets/images/background.png") no-repeat;
  background-size: 100% 100%;
  padding-top: 1%;
  font-size: 28px;
  color: #fff;

  // 标题样式
  .title-frame {
    width: 100%;
    height: 5%;
    // background: url("../../assets/images/title-frame.png") no-repeat;
    // background-size: 100% 100%;
    display: flex;
    justify-content: space-between;
    .title-left {
      margin-left: 1%;
      .left-coin {
        color: #ffcc22;
      }
    }
    .global-title {
      width: 30%;
      text-align: center;
      font-size: 40px;
      font-weight: bold;
    }
  }

  // 内容样式
  .frame-content {
    display: flex;
    justify-content: space-between;
    padding-top: 1%;
    .accruing-amounts,
    .accruing-person {
      width: 30%;
      height: 100%;
    }
    .current-amounts {
      width: 40%;
      height: 100%;
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
        padding-top: 5%;
      }
    }
  }
}
</style>