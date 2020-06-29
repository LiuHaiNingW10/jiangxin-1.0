<template>
  <div class="amount-type-chart" id='amountTypeChart'>
   
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
      let myChart = this.$echarts.init(document.getElementById('amountTypeChart'));
      // 绘制图表
      myChart.setOption({
        color: [
            {
                type: 'linear',
                x: 0,
                y: 0,
                x2: 1,
                y2: 0,
                colorStops: [{
                    offset: 0, color: '#1F7CFF' // 0% 处的颜色
                }, {
                    offset: 0.9, color: '#2BF1FF' 
                }, {
                    offset: 1, color: '#FFFFFF ' // 100% 处的颜色
                }],
                global: false // 缺省为 false
            }
        ],
        tooltip: {
            trigger: 'axis',
            axisPointer: {
                type: 'shadow'
            },
            formatter: function(params) {
            return params[0].name + '<br/>' +
                "<span style='display:inline-block;margin-right:5px;border-radius:10px;width:9px;height:9px;background-color:rgba(36,207,233,0.9)'></span>" +
                params[0].seriesName + ' : ' + Number((params[0].value.toFixed(4) / 10000).toFixed(2)).toLocaleString() + ' 万元<br/>'
            }
        },
        grid: {
          top: "10%",
          left: "10%",
          right: "10%",
          bottom: "10%",
          containLabel: true
        },
        xAxis: {
            show: false,
            type: 'value',
            boundaryGap: [0, 0.01],
            axisLabel: {
              color: 'rgba(255,255,255)'
            },
            splitLine: {
              show: false
            }
        },
        yAxis:[
          {
            type: 'category',
            axisLabel: {
              color: 'rgba(255,255,255)'
            },
            data: [ '其他','银联','理财','网联','代收', '支付', '转账', '提现', '充值']
          },
        ],
        series: [
            {
              name: '拦截金融',
              type: 'bar',
              zlevel: 1,
              itemStyle: {
                barBorderRadius: 6,
                color:  {
                  type: 'linear',
                  x: 0,
                  y: 0,
                  x2: 1,
                  y2: 0,
                  colorStops: [{
                      offset: 0, color: '#1F7CFF' // 0% 处的颜色
                  }, {
                      offset: 0.9, color: '#2BF1FF' 
                  }, {
                      offset: 1, color: '#FFFFFF ' // 100% 处的颜色
                  }],
                  global: false // 缺省为 false
              }
              },
              barWidth: 12,
              data: [50000000, 22000000, 10000000, 5000000, 2000000,50000000, 22000000, 10000000, 5000000]
            },
            {
              name: '背景',
              type: 'bar',
              barWidth: 12,
              barGap: '-100%',
              label: {
                show: true,
                position: 'right',
                color: 'rgba(255,255,255)'
              },
              data: [50000000, 50000000, 50000000, 50000000, 50000000,50000000, 50000000, 50000000, 50000000],
              itemStyle: {
                color: 'rgba(255,255,255,0)',
                barBorderRadius: 30,
                borderWidth: 1,
                borderColor: 'rgba(255,255,255,.4)'
              },
            }
        ]
      });
    }
  },
  components: {}
};
</script>

<style lang="less" scoped>
.amount-type-chart{
  width: 100%;
  height: 24%;
}
</style>
