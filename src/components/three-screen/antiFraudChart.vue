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
      // [{
      //     "value": "60",
      //     "name": "mxjczb"
      // },
      // {
      //     "value": "20",
      //     "name": "rgsh"
      // },
      // {
      //     "value": "20",
      //     "name": "cljc"
      // }]
      const colorList = [
        "#7F6AAD",
        "#59CB74",
        "#FBD444"
      ];
      let myCharts = this.$echarts.init(document.getElementById(id));
      // let _that = this;
      myCharts.setOption({
        title: {
          text: '',
          subtext: '',
          left: 'center',
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        legend: {
          type: 'scroll',
          orient: 'vertical',
          right: "30%",
          top: 'center',
          textStyle: {
            fontSize: 16,
            color: "#FFF",
            lineHeight: 20
          },
          data: antiFraudDatas.map(item => {
            return item.name;
          }),
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
        color: colorList,
        series: [
        {
          name: '半径模式',
          type: 'pie',
          radius: [20, 110],
          center: ['35%', '50%'],
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
  .anti-echarts {
    width: 100%;
    height: 260px;
  }
</style>
