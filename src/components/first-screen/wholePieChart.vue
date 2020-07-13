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
        legend: {
          orient: "vertical",
          right: "8%",
          top: "50%",
          icon: "rect",
          itemWidth: 10,
          itemHeight: 10,
          itemGap: 15,
          textStyle: {
            color: "#FFF"
          }
        },
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
            center: ["40%", "50%"],
            radius: "60%",
            label: {
              position: "inside",
              formatter: "{b}{c}%",
              color: "#FFF",
              fontSize: 14
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
