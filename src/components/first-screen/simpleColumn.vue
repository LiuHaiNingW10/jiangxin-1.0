<template>
  <div class="simple-column" :id="ids"></div>
</template>

<script>
export default {
  name: "",
  props: ["ids", "chartData"],
  created() {},
  mounted() {
    this.initChart(this.ids, this.chartData);
  },
  data() {
    return {};
  },
  computed: {},
  methods: {
    initChart(chartId, chartData) {
      let myChart = this.$echarts.init(document.getElementById(chartId));
      let lineOption = {
        lineStyle: {
          color: "rgba(151,151,151,0.5)",
          type: "dashed"
        }
      };
      myChart.setOption({
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow"
          }
        },
        grid: {
          top: "14%",
          right: "3%",
          left: "5%",
          bottom: "14%"
        },
        xAxis: [
          {
            type: "category",
            data: chartData.xAxis,
            axisLine: lineOption,
            axisTick: {
              show: false
            },
            axisLabel: {
              margin: 10,
              color: "#FFF",
              textStyle: {
                fontSize: 14
              }
            }
          }
        ],
        yAxis: [
          {
            axisLabel: {
              formatter: "{value}",
              color: "#FFF"
            },
            axisTick: {
              show: false
            },
            axisLine: lineOption,
            splitLine: lineOption
          },
          {
            axisLine: lineOption,
            axisTick: {
              show: false
            },
            splitLine: {
              show: false
            }
          }
        ],
        series: [
          {
            type: "bar",
            data: chartData.yAxis,
            barWidth: "20px",
            itemStyle: {
              normal: {
                color: new this.$echarts.graphic.LinearGradient(
                  0,
                  0,
                  0,
                  1,
                  [
                    {
                      offset: 0,
                      color: "#00BD89" // 0% 处的颜色
                    },
                    {
                      offset: 1,
                      color: "#C9F9E1" // 100% 处的颜色
                    }
                  ],
                  false
                )
              }
            }
          }
        ]
      });
    }
  },
  components: {},
  watch: {
    chartData: function(newVal) {
      this.initChart(this.ids, newVal);
    }
  }
};
</script>

<style lang="less" scoped>
</style>
