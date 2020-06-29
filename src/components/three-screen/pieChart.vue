<template>
  <div class="pie-chart" id='pieChart'>
      <div class="pie-item"  v-for="(item, index) in data" :key="index" >
          <div class="item-chart" :id="'pieChart'+index"></div>
          <div class="item-title">{{item.label}}</div>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
        data: [
            {
                label: '反欺诈模型决策占比',
                value: 40
            },
            {
                label: '反欺诈策略决策占比',
                value: 60
            },
            {
                label: '反欺诈人工审核占比',
                value: 25
            }
        ]
    };
  },
  mounted () {
    this.data.forEach((item, index)=>{
        this.drawChart(item, index)
    })
    
  },
  methods: {
    drawChart(data, index) {
      let myChart = this.$echarts.init(document.getElementById('pieChart'+index));
      // 绘制图表
      myChart.setOption({
        series: [{
            name: '',
            type: 'pie',
            clockWise: false,
            radius: [55, 60],
            hoverAnimation: false,
            itemStyle: {
                normal: {
                    labelLine: {
                        show: false
                    }
                }
            },
            data: [
                {
                    value: parseFloat(data.value),
                    label: {
                        normal: {
                            rich: {
                                a: {
                                    color: '#6FFEFF',
                                    align: 'center',
                                    fontSize: 24,
                                    fontWeight: "bold"
                                },
                                b: {
                                    color: '#fff',
                                    align: 'center',
                                    fontSize: 12
                                }
                            },
                            formatter: function(params){
                                return "{a|"+params.value+"%}";
                            },
                            position: 'center',
                            show: true,
                            textStyle: {
                                fontSize: '14',
                                fontWeight: 'normal',
                                color: '#fff'
                            }
                        }
                    },
                    itemStyle: {
                        normal: {
                            borderWidth: 5,
                            shadowBlur: 30,
                            borderColor: {
                                type: 'linear',
                                x: 0,
                                y: 0,
                                x2: 0,
                                y2: 1,
                                colorStops: [{
                                        offset: 0, color: '#0DFEFE' // 0% 处的颜色
                                    }, {
                                        offset: 1, color: '#088BFF' // 100% 处的颜色
                                    }],
                                global: false // 缺省为 false
                            },
                            shadowColor: 'rgba(142, 152, 241, 0.6)'
                        }
                    }
                }, 
                {
                    value: 100 - parseFloat(data.value),
                    name: '',
                    itemStyle: {
                        normal: {
                        label: {
                            show: false
                        },
                        labelLine: {
                            show: false
                        },
                        color: 'rgba(0, 0, 0, 0)',
                        borderColor: 'rgba(0, 0, 0, 0)',
                        borderWidth: 0
                    }
                }
            }]
        }]
      });
    }
  },
  components: {}
};
</script>

<style lang="less" scoped>
.pie-chart{
  margin: 0 auto;
  width: 60%;
  height: 20%;
  display: flex;
  .pie-item{
    width: 20%;
    height: 100%;
    .item-chart{
        width: 100%;
        height: 90%;
    }
    .item-title{
        font-size: 20px;
    }
  }
}
</style>
