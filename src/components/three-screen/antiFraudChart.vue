<template>
  <div class="anti-echarts" :id="ids"></div>
</template>

<script>
export default {
  name: "education",
  props: ["ids", "antiFraudDatas"],
  mounted() {
    // this.initPieChart(this.ids, this.antiFraudDatas);
  },
  data() {
    return {};
  },
  computed: {},
  methods: {
    initPieChart(id,antiFraudDatas) {
      // const colorList = [
      //   "#47A2FF",
      //   "#53C8D1",
      //   "#59CB74",
      //   "#FBD444",
      //   "#7F6AAD",
      //   "#585247"
      // ];
      let myCharts = this.$echarts.init(document.getElementById(id));
      // let _that = this;
      myCharts.setOption({
        title: {
          text: '南丁格尔玫瑰图',
          subtext: '纯属虚构',
          left: 'center'
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        legend: {
          type: 'scroll',
          orient: 'vertical',
          left: 'center',
          top: 'center',
          data: ['rose1', 'rose2', 'rose3']
        },
        toolbox: {
          show: true,
          feature: {
            mark: {show: true},
            dataView: {show: true, readOnly: false},
            magicType: {
                show: true,
                type: ['pie', 'funnel']
            },
            restore: {show: true},
            saveAsImage: {show: true}
          }
        },
        series: [
        {
          name: '半径模式',
          type: 'pie',
          radius: [20, 110],
          center: ['25%', '50%'],
          roseType: 'radius',
          label: {
              show: false
          },
          emphasis: {
              label: {
                  show: true
              }
          },
          data: antiFraudDatas
        }
      ]  
      });
    }
  },
  components: {},
  watch: {
    antiFraudDatas: function(newVal) {
      this.initPieChart(this.ids, newVal);
    }
  }
};
</script>

<style lang="less">
</style>
