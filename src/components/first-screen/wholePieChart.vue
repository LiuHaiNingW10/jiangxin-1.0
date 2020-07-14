<template>
  <div class="whole-pie-chart" :id="ids"></div>
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
      myChart.setOption({
        // legend: {
        //   orient: "vertical",
        //   left: "70%",
        //   top: "50%",
        //   icon: "rect",
        //   itemWidth: 10,
        //   itemHeight: 10,
        //   itemGap: 15,
        //   textStyle: {
        //     color: "#FFF"
        //   }
        // },
        color: [
          "#5B4CFF",
          "#00F7FF",
          "#1D9DFA",
          "#0048F1",
          "#70ad47",
          "#ff0000"
        ],
        series: [
          {
            type: "pie",
            // center: ["40%", "50%"],
            radius: "70%",
            // label: {
            //   position: "inside",
            //   formatter: "{b}{c}%",
            //   color: "#FFF",
            //   fontSize: 14
            // },
            labelLine: {
              normal: {
                length: 40,
                length2: 50,
                lineStyle: {
                  type: "solid"
                }
              }
            },
            label: {
              normal: {
                formatter: params => {
                  // console.log(params);
                  return (
                    "{b| " +
                    params.name +
                    "}  " +
                    "\n" +
                    "{c|" +
                    params.percent.toFixed(0) +
                    "%}"
                  );
                },
                borderWidth: 0,
                borderRadius: 4,
                padding: [50, -40],
                height: 70,
                fontSize: 13,
                align: "center",
                color: "#3494BD",
                rich: {
                  b: {
                    fontSize: 16,
                    lineHeight: 20,
                    color: "rgba(255, 255, 255, .7)",
                    padding: [0, 0, 5, 0],
                    fontWeight: "bold"
                  },
                  c: {
                    fontSize: 16,
                    lineHeight: 20,
                    color: "rgba(255, 255, 255, 1)",
                    padding: [0, 0, 5, 0],
                    fontWeight: "bold"
                  }
                }
              }
            },
            data: chartData
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
