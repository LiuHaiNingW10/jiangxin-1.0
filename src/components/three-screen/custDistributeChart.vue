<template>
  <div class="cust-distribute-chart" id='custDistributeChart'>
   
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  mounted () {
    this.drawChart()
  },
  methods: {
    drawChart() {
      let myChart = this.$echarts.init(document.getElementById('custDistributeChart'));
      // 绘制图表
      myChart.setOption({
        tooltip: {
          trigger: "axis",
          axisPointer: {
            lineStyle: {
              color: {
                type: "linear",
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [
                  {
                    offset: 0,
                    color: "rgba(255,255,255,0)" // 0% 处的颜色
                  },
                  {
                    offset: 0.5,
                    color: "rgba(255,255,255,1)" // 100% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "rgba(255,255,255,0)" // 100% 处的颜色
                  }
                ],
                global: false // 缺省为 false
              }
            }
          }
        },
        legend: {
            data: ['历史进件客群分布', '近一个月批核客群', '今日批核客群'],
            bottom: 0,
            textStyle: {
              color: '#fff'
            }
        },
        grid: {
            left: '3%',
            right: '4%',
            bottom: '10%',
            containLabel: true
        },
        toolbox: {
            feature: {
                saveAsImage: {}
            }
        },
        xAxis: {
            type: 'category',
            boundaryGap: false,
            axisLabel: {
              color: 'rgba(255,255,255)'
            },
            splitLine: {
              show: false
            },
            data: ['100', '200', '300', '400', '500', '600', '700', '800', '900']
        },
        yAxis: {
            type: 'value',
            axisLabel: {
              color: 'rgba(255,255,255)',
              formatter: '{value} %'
            },
            splitLine: {
              show: false
            }
        },
        series: [
            {
                name: '历史进件客群分布',
                type: 'line',
                symbol: "circle",
                showSymbol: false,
                stack: '总量',
                data: [120, 132, 101, 134, 90, 230, 210]
            },
            {
                name: '近一个月批核客群',
                type: 'line',
                symbol: "circle",
                showSymbol: false,
                stack: '总量',
                data: [220, 182, 191, 234, 290, 330, 310]
            },
            {
                name: '今日批核客群',
                type: 'line',
                symbol: "circle",
                showSymbol: false,
                stack: '总量',
                data: [150, 232, 201, 154, 190, 330, 410]
            }
        ]
      });
    }
  },
  components: {}
};
</script>

<style lang="less" scoped>
.cust-distribute-chart{
  width: 100%;
  height: 25%;
}
</style>
