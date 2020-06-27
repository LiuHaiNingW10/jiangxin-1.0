<template>
  <div id="app" class="ts-mc">
    <div class="echarts w100" id="echarts-a"></div>
    <div class="echarts w100" id="echarts-b"></div>
    <div class="echarts w100" id="echarts-c"></div>
    <div class="echarts w100" id="echarts-d"></div>
    <div class="echarts w100" id="echarts-e"></div>
    <div class="echarts w100" id="echarts-f"></div>
    <div class="echarts w100" id="echarts-g"></div>
    <div class="echarts w100" id="echarts-h"></div>
  </div>
</template>

<script>
// import axios from "axios";

export default {
  name: "home",
  components: {},
  mounted() {
    this.drawLineA();
    this.drawLineB();
    this.drawLineC();
    this.drawLineD();
    this.drawLineE();
    this.drawLineF();
    this.drawLineG();
    this.drawLineH();
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    calculateMax(e, type) {
      var arr = [];
      e.forEach(el => {
        arr.push(el[type]);
      });
      return arr.indexOf(Math.max.apply(null, arr)) || 0;
    },
    drawLineA() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-a"));
      // 绘制图表
      var a = "贷款客户数和余额趋势";
      var b = "2019年01月25日";
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
          }
        },
        color: ["#0078d7", "#e5b322"],
        legend: {
          data: [
            { name: "贷款余额", icon: "circle" },
            { name: "贷款客户数", icon: "roundRect" }
          ],
          align: "left",
          bottom: 10
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
                    "</span>";
            }
            return str + "</div>";
          }
        },
        toolbox: {
          show: false,
          feature: {
              saveAsImage: {}
          }
        },
        grid: {
          top: 60,
          bottom: 60,
          left: "10%",
          right: "10%",
          borderWidth: 1
        },
        xAxis: {
          data: [
            "2018年1月",
            "2018年2月",
            "2018年3月",
            "2018年4月",
            "2018年5月",
            "2018年6月"
          ]
        },
        yAxis: [
          {
            name: "贷款余额",
            nameGap: "40",
            splitLine: {
              show: false
            },
            axisLine: { show: false },
            axisTick: { show: false },
            axisLabel: {
              formatter: "{value}亿"
            }
          },
          {
            name: "贷款客户数",
            nameRotate: 270,
            nameLocation: "middle",
            nameGap: "40",
            splitLine: {
              show: false
            },
            axisLine: { show: false },
            axisTick: { show: false },
            axisLabel: {
              formatter: "{value}k"
            }
          }
        ],
        series: [
          {
            name: "贷款余额",
            type: "bar",
            data: ["5", "20", "36", "10", "10", "20"]
          },
          {
            name: "贷款客户数",
            type: "line",
            yAxisIndex: 1,
            data: ["50", "200", "360", "100", "100", "200"]
          }
        ]
      });
    },

    // 数组排序或 对象数组排序
    sortByValue(arr, value) {
      arr.sort(function(x, y) {
        const m = (value && x[value]) || x,
          n = (value && y[value]) || y;
        return n - m;
      });
      return arr;
    },
    drawLineB() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-b"));
      // 绘制图表
      var a = "贷款余额五级分类-余额";
      var b = "2019年01月25日 五级客户";
      var c = [
        { value: 335, name: "正常类余额" },
        { value: 310, name: "关注类余额" },
        { value: 1548, name: "损失类余额" },
        { value: 234, name: "次级类余额" },
        { value: 135, name: "可疑类余额" }
      ];
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
          }
        },
        color: ["#0078d7", "#e5b322"],
        legend: {
          orient: "vertical",
          x: "right",
          icon: "circle",
          data: [
            "正常类余额",
            "关注类余额",
            "次级类余额",
            "可疑类余额",
            "损失类余额"
          ]
        },
        tooltip: {
          trigger: "item",
          formatter: function(option) {
            var str =
              "<div class='echart-shadow tool-tip'>" +
              `<span style="color: ${option.color}" >` +
              option.name +
              ": " +
              option.value +
              "</span><br/>" +
              `<span style="color:gray"> 占比: ` +
              option.percent +
              "%" +
              "</span>";
            return str + "</div>";
          }
        },
        grid: {
          top: 60,
          bottom: 60,
          left: "10%",
          right: "10%",
          borderWidth: 1
        },

        series: [
          {
            type: "pie",
            radius: ["50%", "70%"],
            avoidLabelOverlap: false,
            label: {
              normal: {
                show: false,
                position: "center"
              },
              emphasis: {
                show: true,
                formatter: ["{bt|{b}}", "{num|{d}%}"].join("\n"),
                rich: {
                  bt: {
                    fontSize: "12",
                    lineHeight: "25"
                  },
                  num: {
                    fontSize: "18",
                    color: "gray"
                  }
                },
                textStyle: {
                  fontSize: "18",
                  fontWeight: "bold"
                }
              }
            },
            data: this.sortByValue(c, "value")
          }
        ]
      });
      // 高亮当前图形
      myChart.dispatchAction({
          type: "highlight",
          seriesIndex: 0,
          dataIndex: 0
      });
      myChart.on("mouseover", function(a) {
        if (a.dataIndex != 0) {
          // 取消之前高亮的图形
          myChart.dispatchAction({
            type: "downplay",
            seriesIndex: 0,
            dataIndex: 0
          });
        }
      });
      myChart.on("mouseout", function() {
        // 高亮当前图形
        myChart.dispatchAction({
          type: "highlight",
          seriesIndex: 0,
          dataIndex: 0
        });
      });
    },
    drawLineC() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-c"));
      // 绘制图表
      var a = "交易银行保证金存款产品结构分析";
      var b = "2019年01月25日";
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
          margin: 10,
          subtext: ["{b|8,676}", "{c|总计}"].join("\n"),
          subtextStyle: {
            rich: {
              b: {
                color: "gray",
                fontSize: "34",
                fontFamily: "",
                fontWeight: "100"
              },
              c: {
                color: "gray",
                fontSize: "14"
              }
            }
          }
        },
        color: ["#0078d7", "#e5b322"],
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
                    "k" +
                    "</span>" +
                    "<br/>"
                  : `<span style="color: ${option[i].color}" >` +
                    option[i].seriesName +
                    ": " +
                    option[i].value +
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
          axisLabel: {
            rotate: 45
          },
          data: [
            "2018年1月",
            "2018年2月",
            "2018年3月",
            "2018年4月",
            "2018年5月",
            "2018年6月",
            "2018年7月",
            "2018年8月",
            "2018年9月",
            "2018年10月",
            "2018年11月"
          ]
        },
        yAxis: {
          name: "客户数",
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
            formatter: "{value}k"
          }
        },
        series: [
          {
            name: "客户数",
            type: "bar",
            label: {
              normal: {
                show: true,
                position: "top",
                color: "#000"
              }
            },
            data: [
              "5",
              "20",
              "36",
              "10",
              "10",
              "20",
              "36",
              "10",
              "10",
              "20",
              "30"
            ]
          }
        ]
      });
    },
    drawLineD() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-d"));
      // 绘制图表
      var a = "交易银行保证金存款产品结构分析";
      var b = "2019年01月25日 ";
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
        color: ["#0078d7", "#e5b322"],
        legend: {
          data: [
            { name: "存单质压余额", icon: "circle" },
            { name: "保证金余额", icon: "circle" }
          ],
          align: "left",
          bottom: 10
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
          data: [
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
          name: "保证金存款产品",
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
            name: "存单质压余额",
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
            name: "保证金余额",
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
    drawLineE() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-e"));
      // 绘制图表
      var a = "类放金额";
      var b = "";
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
          }
        },
        color: ["#0078d7", "#92A956", "#367F55", "#C49C2B"],
        legend: {
          orient: "vertical",
          x: "right",
          icon: "circle",
          data: [
            "小企业C类客户",
            "小企业D类客户",
            "小企业B类客户",
            "小企业A类客户"
          ]
        },
        tooltip: {
          trigger: "item",
          formatter: function(option) {
            var str =
              "<div class='echart-shadow tool-tip'>" +
              `<span style="color: ${option.color}" >` +
              option.name +
              ": " +
              option.value +
              "</span><br/>" +
              `<span style="color:gray"> 占比: ` +
              option.percent +
              "%" +
              "</span>";
            return str + "</div>";
          }
        },
        grid: {
          top: 60,
          bottom: "18%",
          left: "10%",
          right: "10%",
          borderWidth: 1
        },

        series: [
          {
            type: "pie",
            avoidLabelOverlap: false,
            radius: "60%",
            center: ["50%", "60%"],
            labelLine: {
              show: true,
              length: 10,
              length2: 50,
              position: "outside",
              formatter: "{a}%"
            },
            itemStyle: {
              c: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            },
            data: [
              { value: 335, name: "小企业C类客户" },
              { value: 310, name: "小企业D类客户" },
              { value: 234, name: "小企业B类客户" },
              { value: 135, name: "小企业A类客户" }
            ]
          }
        ]
      });
    },
    drawLineF() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-f"));
      // 绘制图表
      var a = "公司客户结构";
      var b = "2019年10月";
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
          }
        },
        color: ["#B03F13", "#BC7718", "#C8B72C", "#70932F"],
        legend: {
          orient: "vertical",
          right: 100,
          top: 40,
          data: [
            "交易银行考核客户",
            "交易银行有效客户",
            "优先结算客户",
            "公司客户"
          ]
        },
        tooltip: {
          trigger: "item",
          formatter: function(option) {
            var str =
              "<div class='echart-shadow tool-tip'>" +
              `<span style="color: ${option.color}" >` +
              option.name +
              ": " +
              option.value +
              "</span><br/>" +
              `<span style="color:gray"> 占比: ` +
              option.percent +
              "%" +
              "</span>";
            return str + "</div>";
          }
        },
        grid: {
          top: "200",
          bottom: "200",
          left: "100",
          right: "100",
          borderWidth: 1
        },
        series: [
          {
            name: "漏斗图",
            type: "funnel",
            left: "25%",
            rotate: 180,
            top: 60,
            bottom: 60,
            width: "40%",
            height: "70%",
            sort: "ascending",
            gap: 4,
            label: {
              show: true,
              position: "inside",
              fontSize: 10,
              formatter: "{c}"
            },
            labelLine: {
              length: 10,
              lineStyle: {
                width: 1,
                type: "solid"
              }
            },
            itemStyle: {
              borderColor: "#fff",
              borderWidth: 1
            },
            emphasis: {
              label: {
                fontSize: 20
              }
            },
            data: [
              { value: 60, name: "交易银行考核客户" },
              { value: 40, name: "交易银行有效客户" },
              { value: 20, name: "优先结算客户" },
              { value: 80, name: "公司客户" }
            ]
          }
        ]
      });
    },
    drawLineG() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-g"));
      // 绘制图表
      var a = "信贷客户数——表内";
      var b = "2018年12月31日";
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
          margin: 10,
          subtext: ["{b|9,205}", "{c|总计}"].join("\n"),
          subtextStyle: {
            rich: {
              b: {
                color: "gray",
                fontSize: "34",
                fontFamily: "",
                fontWeight: "100"
              },
              c: {
                color: "gray",
                fontSize: "14"
              }
            }
          }
        },
        color: ["#C98531"],
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
                    option[i].marker +
                    option[i].name +
                    ": " +
                    option[i].value +
                    "</span>" +
                    "<br/>"
                  : `<span style="color: ${option[i].color}" >` +
                    option[i].seriesName +
                    ": " +
                    option[i].value +
                    "</span>";
            }
            return str + "</div>";
          }
        },
        grid: {
          top: 100,
          bottom: 60,
          left: "20%",
          right: "10%",
          borderWidth: 1
        },
        xAxis: {
          type: "value",
          boundaryGap: [0, 0.01],
          splitLine: {
            show: false
          }
        },
        yAxis: {
          type: "category",
          data: [
            "3000万以上",
            "1000-3000万以上(含)",
            "500-1000以上(含)",
            "0-500万(含)"
          ]
        },
        series: [
          {
            type: "bar",
            data: [18203, 29034, 23489, 104970]
          }
        ]
      });
    },
    drawLineH() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("echarts-h"));
      // 绘制图表
      var a = "对公存款日均余额趋势图";
      var b = "";
      myChart.setOption({
        title: {
          text: [`{c| }`, `{a|${a} }`, `{b|${b}}`].join(""),
          textStyle: {
            rich: {
              c: {
                backgroundColor: {
                  image: "../assets/images/logo.png"
                },
                height: 10,
                width: 10
              },
              b: {
                color: "gray",
                fontSize: "14"
              }
            }
          },

          itemGap: 10,
          margin: 10
        },
        color: ["#0078d7", "#e5b322"],
        legend: {
          data: [{ name: "活期存款时余额" }, { name: "一般性存款时点余额" }],
          orient: "vertical",
          align: "left",
          top: 60,
          height: 4,
          width: 4
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
          top: 100,
          bottom: 60,
          left: "10%",
          right: "10%",
          borderWidth: 1
        },
        xAxis: {
          type: "category",
          data: [
            "2018年01月",
            "2018年02月",
            "2018年03月",
            "2018年04月",
            "2018年05月",
            "2018年06月",
            "2018年07月",
            "2018年08月",
            "2018年09月",
            "2018年10月",
            "2018年11月"
          ]
        },
        yAxis: {
          name: "时点余额",
          nameRotate: 90,
          nameLocation: "middle",
          nameGap: "80",
          nameTextStyle: {
            width: "200",
            height: "200",
            backgroundColor: {
              image: "../assets/images/logo.png"
            }
          },
          min: "10",
          splitLine: {
            show: false
          },
          axisTick: {
            show: false
          },
          axisLabel: {
            formatter: "{value}亿"
          }
        },
        series: [
          {
            name: "活期存款时余额",
            type: "line",
            label: {
              normal: {
                show: true,
                color: "#000",
                formatter: "{c}亿"
              }
            },
            data: [
              "50",
              "25",
              "36",
              "20",
              "100",
              "60",
              "55",
              "77",
              "10",
              "20",
              "30"
            ]
          },
          {
            name: "一般性存款时点余额",
            type: "line",
            label: {
              normal: {
                show: true,
                position: "inside",
                color: "#000",
                formatter: "{c}亿"
              }
            },
            data: [
              "5",
              "20",
              "36",
              "10",
              "10",
              "22",
              "36",
              "79",
              "233",
              "54",
              "200"
            ]
          }
        ]
      });
    }
  }
};
</script>

<style>
.ts-mc{
  padding: 10px;
}
.w70 {
  width: 70%;
  height: 300px;
}
.w50 {
  width: 50%;
  height: 400px;
}
.w100 {
  height: 400px;
  max-width: 1300px;
}

</style>
