<template>
  <div class="cyclic-annular" :id="ids"></div>
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
        // grid: {
        //   left: "20%"
        // },
        color: [
          "#2AC9FD",
          "#76FBC0",
          "#35C96E",
          "#FCC708",
          "#48B188",
          "#5957C2",
          "#4A5D73"
        ],
        series: [
          {
            color: [
              "#2AC9FD",
              "#76FBC0",
              "#35C96E",
              "#FCC708",
              "#48B188",
              "#5957C2"
            ],
            type: "pie",
            radius: ["62", "100"],
            labelLine: {
              normal: {
                length: 25,
                length2: 80,
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
                    "{c|" +
                    params.percent.toFixed(0) +
                    "%}"
                  );
                },
                borderWidth: 0,
                borderRadius: 4,
                padding: [0, -86],
                height: 70,
                fontSize: 13,
                align: "center",
                color: "#3494BD",
                rich: {
                  b: {
                    fontSize: 12,
                    lineHeight: 20,
                    color: "#41B3DC",
                    padding: [0, 0, 5, 0]
                  },
                  c: {
                    fontSize: 20,
                    lineHeight: 20,
                    color: "orange"
                  }
                }
              }
            },
            data: chartData
          },
          {
            type: "pie",
            radius: ["110", "111"],
            data: [100],
            label: {
              show: false
            }
          },
          {
            type: "pie",
            radius: ["50", "51"],
            data: [100],
            label: {
              show: false
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
