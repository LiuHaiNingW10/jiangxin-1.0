<template>
  <div class="loan-monitor-chart" id='loanMonitorChart'>
   
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
      let myChart = this.$echarts.init(document.getElementById('loanMonitorChart'));
      // 绘制图表
      myChart.setOption({
        color: [ '#1F71FF', '#F5A623', '#ccc'],
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
        grid: {
            top: '8%',
            left: '10%',
            right: '10%',
            bottom: '20%',
            containLabel: true,
        },
        legend: {
            itemGap: 50,
            data: ['放款量','户均额度' ,'批核率'],
            textStyle: {
                color: '#f9f9f9',
                borderColor: '#fff'
            },
            bottom: 0
        },
        xAxis: [{
            type: 'category',
            boundaryGap: true,
            axisLine: { //坐标轴轴线相关设置。数学上的x轴
                show: true,
                lineStyle: {
                    color: '#f9f9f9'
                },
            },
            axisLabel: { //坐标轴刻度标签的相关设置
                textStyle: {
                    color: '#d1e6eb',
                    margin: 15,
                },
            },
            axisTick: {
                show: false,
            },
            data: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24' ],
        }],
        yAxis: [{
            type: 'value',
            min: 0,
            // max: 140,
            splitNumber: 7,
            splitLine: {
                show: true,
                lineStyle: {
                    color: '#0a3256'
                }
            },
            axisLine: {
                show: false,
            },
            axisLabel: {
                margin: 20,
                textStyle: {
                    color: '#d1e6eb',

                },
            },
            axisTick: {
                show: false,
            },
        }],
        series: [{
            name: '户均额度',
            type: 'line',
            symbol: 'circle',  // 默认是空心圆（中间是白色的），改成实心圆
            showSymbol: false,
            // lineStyle: {
            //     normal: {
            //         color: "#28ffb3", // 线条颜色
            //     },
            //     borderColor: '#f0f'
            // },
            // label: {
            //     show: true,
            //     position: 'top',
            //     textStyle: {
            //         color: '#fff',
            //     }
            // },
            // itemStyle: {
            //     normal: {
            //         color: "#28ffb3",

            //     }
            // },
            tooltip: {
                show: true
            },
            data: [393, 438, 485, 631, 689, 824, 987,393,393,393,393,393,393,393,393,393,393,393,93, 438, 485, 631, 689, 824]
        }, 
        {
            name: '批核率',
            type: 'line',
            symbol: 'circle',  // 默认是空心圆（中间是白色的），改成实心圆
            showSymbol: false,
            // lineStyle: {
            //     normal: {
            //         color: "rgba(0,255,255)", // 线条颜色
            //     },
            //     borderColor: '#f0f'
            // },
            // label: {
            //     show: true,
            //     position: 'top',
            //     textStyle: {
            //         color: '#fff',
            //     }
            // },
            // itemStyle: {
            //     normal: {
            //         color: "#28ffb3",

            //     }
            // },
            tooltip: {
                show: true
            },
            data: [500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500, 500]
        },
        {
            name: '放款量',
            type: 'bar',
            barWidth: 15,
            tooltip: {
                show: true
            },
            // label: {
            //     show: true,
            //     position: 'top',
            //     textStyle: {
            //         color: '#fff',
            //     }
            // },
            data: [800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800, 800]
        }]
      });
    }
  },
  components: {}
};
</script>

<style lang="less" scoped>
.loan-monitor-chart{
  width: 100%;
  height: 25%;
}
</style>
