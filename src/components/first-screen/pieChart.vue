<template>
  <div class="pie-echarts" :id="id"></div>
</template>

<script>
export default {
  name: "education",
  props: ["id", "chartData"],
  mounted() {
    this.initPieChart(this.id, this.chartData);
  },
  data() {
    return {};
  },
  computed: {},
  methods: {
    initPieChart(id, data) {
      const colorList = [
        "#47A2FF ",
        "#53C8D1",
        "#59CB74",
        "#FBD444",
        "#7F6AAD",
        "#585247"
      ];
      let myCharts = this.$echarts.init(document.getElementById(id));
      myCharts.setOption({
        title: {
          text: "学历分布",
          textStyle: {
            fontSize: 38,
            color: "#FFF",
            lineHeight: 20
          },
          textAlign: "center",
          left: "30%",
          top: "48%"
        },
        tooltip: {
          trigger: "item"
        },
        legend: {
          type: "scroll",
          orient: "vertical",
          right: "10%",
          top: "center",
          itemGap: 30,
          selectedMode: false,
          icon: "pin",
          data: data.map(item => {
            return item.name;
          }),
          textStyle: {
            color: "#FFF",
            rich: {
              uname: {
                width: 100
              },
              unum: {
                color: "#4ed139",
                width: 40,
                align: "right"
              }
            }
          },
          formatter(name) {
            if (data && data.length) {
              for (var i = 0; i < data.length; i++) {
                if (name === data[i].name) {
                  return `{uname|${name}}{unum|` + data[i].value + `}`;
                }
              }
            }
          }
        },
        color: colorList,
        series: [
          {
            name: "姓名",
            type: "pie",
            radius: [140, 180],
            center: ["30%", "50%"],
            label: {
              show: false
            },
            labelLine: {
              show: false
            },
            data: data
          }
        ]
      });
    }
  },
  components: {}
};
</script>

<style lang="less">
</style>
