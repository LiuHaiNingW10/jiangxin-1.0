<template>
  <div class="amount-type-chart" id='amountTypeChart'>
   
  </div>
</template>

<script>
export default {
  data() {
    return {
      timer: null
    };
  },
  mounted () {
    this.getData()
    this.timer = setInterval(() => {
      setTimeout(this.getData, 0)
    }, 3000)
  },
  methods: {
    getData () {
      this.axios.get('/api/p3/interceptTradeAllType')
      .then((response) => {
        const { data } = response.data
        this.drawChart(data)
      })
      .catch((error)=> {
          console.log(error);
      });
    },
    drawChart(data) {
      let myChart = this.$echarts.init(document.getElementById('amountTypeChart'));
      if(!data || !data.length) return
      var payAmounts = data.map(item=>item.payamount).reverse()
      var totalAmount = payAmounts.reduce((x,y)=>x+y)
      var totalArr = new Array(data.length).fill(parseFloat(totalAmount).toFixed(2))
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
                params[0].seriesName + ' : ' + Number(params[0].value).toLocaleString() + '元<br/>'
            }
        },
        grid: {
          top: 40,
          left: 73,
          right: 29,
          bottom: '11%',
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
              textStyle: {
                color: 'rgba(255,255,255,0.7)',
                fontSize:20
              }
            },
            axisLine:{
             show:false
            },
            data: data.map(item=>item.event).reverse()
          },
        ],
        series: [
            {
              name: '拦截金融',
              type: 'bar',
              zlevel: 1,
              label: {
                show: true,
                position: 'right',
                color: 'rgba(255,255,255)',
                formatter: '{@score}元'
              },
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
              data: payAmounts
            },
            {
              name: '背景',
              type: 'bar',
              barWidth: 12,
              barGap: '-100%',
              label: {
                show: false,
                position: 'right',
                color: 'rgba(255,255,255)'
              },
              data: totalArr,
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
  beforeDestroy () {
    clearInterval(this.timer)  
    this.timer = null
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
