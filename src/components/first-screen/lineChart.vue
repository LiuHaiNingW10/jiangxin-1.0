<template>
  <div class>
    <div class="line-chart-div" v-for="(item) in ids" :key="item" :id="item"></div>
  </div>
</template>

<script>
export default {
  name: "lineChart",
  props: ["chartData", "ids", "trueData"],
  data() {
    return {
      echartState: {} // 标识 图表是否已经下钻
    };
  },
  mounted() {
    for (let i in this.ids) {
      this.drawLineH(this.ids[i], this.chartData[i], this.trueData);
      // this.renderChartState(this.ids[i].id, this.chartData[i]);
    }
  },
  methods: {
    drawLineH(id, data, trueData) {
      var _this = this;
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById(id));
      // 绘制图表
      // var a = data.chartName || "";
      myChart.setOption({
        // title: {
        //   text: [data.DrillDown ? `{c|}` : "", `{a|${a} }`].join(""),
        //   triggerEvent: true,
        //   textStyle: {
        //     fontSize: 14,
        //     color: "#FFF",
        //     rich: {
        //       c: {
        //         backgroundColor: {
        //           image: require("../../assets/images/DrillDown.svg")
        //         },
        //         height: 20,
        //         width: 20
        //       }
        //     }
        //   },

        //   itemGap: 10,
        //   margin: 10,
        //   left: "center"
        // },
        color: ["#0078d7", "#e5b322"],
        // legend: {
        //   data: data.legend || [
        //     { name: "当日实时放款" }
        //     // { name: "七日均值" }
        //   ],
        //   orient: "vertical",
        //   align: "left",
        //   top: 50,
        //   left: 120,
        //   height: 4,
        //   width: 4,
        //   itemHeight: 8,
        //   itemWidth: 8,
        //   textStyle: {
        //     color: "#fff",
        //     fontSize: 12
        //   }
        // },
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
                    "" +
                    "</span>" +
                    "<br/>"
                  : `<span style="color: ${option[i].color}" >` +
                    option[i].seriesName +
                    ": " +
                    option[i].value +
                    "" +
                    "</span>";
            }
            return str + "</div>";
          }
        },
        grid: {
          top: 140,
          bottom: 60,
          left: "10%",
          right: "10%",
          borderWidth: 1
        },
        xAxis: {
          type: "category",
          axisLabel: {
            //刻度标签文字的颜色
            show: true,
            color: "#FFF"
          },
          axisLine: {
            show: false,
            lineStyle: {
              color: "#FFF"
            }
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: "#32346c "
            }
          },
          data: trueData.data_dt || [
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
          name: (data.yAxis && data.yAxis[0]) || "",
          // nameRotate: 90,
          // nameLocation: "middle",
          // nameGap: "80",
          nameTextStyle: {
            color: "#FFF"
          },
          // min: "10",
          axisLine: {
            show: false
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: "#32346c "
            }
          },
          axisTick: {
            show: false
          },
          axisLabel: {
            show: true,
            color: "#FFF",
            formatter: "{value}"
          }
        },
        series: [
          {
            name: (data.legend && data.legend[0].name) || "当日实时放款",
            type: "line",
            smooth: 0.5,
            label: {
              normal: {
                show: true,
                color: "#FFF",
                formatter: "{c}"
              }
            },
            data: trueData.amt_cur || [
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
    },
  }
};
</script>
<style lang="less">
.line-chart-div {
  height: 100%;
  width: 100%;
}
</style>

