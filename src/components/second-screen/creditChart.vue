<template>
  <div class="credit-chart" id='creditChart'>
   
  </div>
</template>

<script>
export default {
  data() {
    return {
    };
  },
  mounted () {
    this.drawChart()
    this.axios.get('/api/p2/cmpGraph')
    .then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });
  },
  methods: {
    drawChart() {
        let myChart = this.$echarts.init(document.getElementById('creditChart'));
        // 绘制图表
        myChart.setOption({
            color: [{
                type: 'linear',
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [{
                    offset: 0, color: '#22ABF5' // 0% 处的颜色
                }, {
                    offset: 1, color: '#0F72E7' // 100% 处的颜色
                }],
                global: false // 缺省为 false
            },{
                type: 'linear',
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [{
                    offset: 0, color: '#76F9FF' // 0% 处的颜色
                }, {
                    offset: 1, color: '#41F1FD' // 100% 处的颜色
                }],
                global: false // 缺省为 false
            },
            {
                type: 'linear',
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [{
                    offset: 0, color: '#FAD961' // 0% 处的颜色
                }, {
                    offset: 1, color: '#F9A842' // 100% 处的颜色
                }],
                global: false // 缺省为 false
            }],
            backgroundColor: 'rgba(0,0,0,0)',
            tooltip: {},
            legend: {
                data: ['指标1', '指标2', '指标3'],
                icon: "circle",
                orient: 'vertical',
                itemWidth: 10,
                itemHeight: 10,
                itemGap: 20,
                right: 200,
                bottom: 100,
                textStyle: {
                    color: 'rgba(255,255,255,0.72)'
                }
            },
            radar: {
                center: ['40%', '50%'],
                name: {
                    textStyle: {
                        color: 'rgba(255,255,255,0.72)',
                        backgroundColor: 'transparent',
                        borderRadius: 3,
                        padding: [3, 5]
                    }
                },
                indicator: [
                    { name: '星期一', max: 100},
                    { name: '星期二', max: 100},
                    { name: '星期三', max: 100},
                    { name: '星期四', max: 100},
                    { name: '星期五', max: 100},
                    { name: '星期六', max: 100},
                    { name: '星期天', max: 100}
                
                ],
                axisLine: { //指向外圈文本的分隔线样式
                lineStyle: {
                    color: 'rgba(81,164,228,0.5'
                }
                },
            splitLine: {
                lineStyle: {
                    color: ['rgba(81,164,228,0.5']
                }
            },
            splitArea: {
                areaStyle: {
                    color: ['transparent']
                }
            },
            },
            series: [{
                type: 'radar',
                areaStyle: {},
                data: [
                    {
                        value: [50, 60, 65, 70, 55, 60, 70],
                        name: '指标1'
                    },
                    {
                        value: [30, 33, 56, 44, 30, 22, 50],
                        name: '指标2'
                    },
                    {
                        value: [10, 20, 30, 40, 80, 60, 80],
                        name: '指标3'
                    }
                ]
            }]
        })
    }
  },
  components: {}
};
</script>

<style lang="less" scoped>
.credit-chart{
  width: 100%;
  height: 360px;
}
</style>
