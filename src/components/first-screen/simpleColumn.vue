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
          color: "rgba(255 ,255 ,255 ,0.5)",
          // type: "dashed"
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
          right: "10%",
          left: "8%",
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
              color: "rgba(255 ,255 ,255 ,0.8)",
              textStyle: {
                fontSize: 18
              }
            }
          }
        ],
        yAxis: [
          {
            axisLabel: {
              fontSize: 18,
              formatter: "{value}",
              color: "rgba(255 ,255 ,255 ,0.5)"
            },
            axisTick: {
              show: false
            },
            axisLine: 'rgba(0, 0, 0, 0)',
            splitLine: lineOption,
            splitNumber: 3,
          },
          // {
          //   axisLine: lineOption,
          //   axisTick: {
          //     show: false
          //   },
          //   splitLine: {
          //     show: false
          //   }
          // }
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
                      color: "#4BE8FA" // 0% 处的颜色
                    },
                    {
                      offset: 1,
                      color: "#0047FF" // 100% 处的颜色
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
