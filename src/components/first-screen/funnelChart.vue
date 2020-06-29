<template>
  <div class="funnel-echarts" :id="ids"></div>
</template>

<script>
export default {
  name: "",
  props: ["ids", "chartData"],
  mounted() {
    this.initChart(this.ids, this.chartData);
  },
  data() {
    return {};
  },
  computed: {},
  methods: {
    initChart(id, data) {
      var colors = ["#f36119", "#ff9921", "#20c8ff", "#2cb7ff", "#1785ef"];
      var myCharts = this.$echarts.init(document.getElementById(id));
      myCharts.setOption({
        title: {
          text: "年龄分布",
          triggerEvent: true,
          textStyle: {
            fontSize: 14,
            color: "#FFF"
          }
        },
        color: colors,

        series: [
          {
            top: "10%",
            type: "funnel",
            left: "1%",
            width: "80%",
            gap: 16,
            minSize: "0%",
            maxSize: "80%",
            label: {
              show: true,
              position: "inside",
              formatter: "{b}"
            },
            data: data
          },

          {
            top: "10%",
            type: "funnel",
            left: "10%",
            width: "80%",
            gap: 16,
            z: 1,
            minSize: "0%",
            maxSize: "0%",
            label: {
              normal: {
                color: "#FFF",
                position: "right",
                borderWidth: 1,
                borderRadius: 4,
                padding: [11, 25, 11, 25],
                width: 50,
                /* formatter:function(d){
                        var ins='{aa|}'+d.percent;
                        console.log(ins)
                        return ins
                       
                    },*/
                rich: {
                  aa: {
                    backgroundColor: {
                      padding: [1, 2, 3, 4]
                    },
                    width: 14,
                    height: 9,
                    align: "center",
                    padding: [0, 0, 0, 0]
                  }
                }
              }
            },
            //右侧的百分比显示的地方
            labelLine: {
              show: true,
              normal: {
                length: 200,
                position: "right",
                lineStyle: {
                  width: 1,
                  color: "#e8e9f1",
                  type: "solid"
                }
              }
            },
            //主体是透明的
            itemStyle: {
              normal: {
                color: "transparent",
                borderWidth: 0,
                opacity: 1
              }
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
