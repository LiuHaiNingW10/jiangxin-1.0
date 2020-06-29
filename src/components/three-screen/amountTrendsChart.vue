<template>
  <div class="amount-trends-chart" id='amountTrendsChart'>
   
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
      let myChart = this.$echarts.init(document.getElementById('amountTrendsChart'));
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
        grid: {
          // top: "18%",
          // left: "15%",
          // right: "5%",
          bottom: "25%"
          // containLabel: true
        },
        xAxis: [
          {
            type: "category",
            boundaryGap: true,
            axisLine: {
              //坐标轴轴线相关设置。数学上的x轴
              show: true,
              lineStyle: {
                color: "#092b5d"
              }
            },
            axisLabel: {
              //坐标轴刻度标签的相关设置
              textStyle: {
                color: "#24c4ff",
                margin: 15
              },
              formatter: function(data) {
                return data + "点";
              }
            },
            axisTick: {
              show: false
            },
            data: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12']
          }
        ],
        yAxis: [
          {
            min: 0,
            max: 100,
            splitLine: {
              show: false,
              lineStyle: {
                color: "#092b5d"
              }
            },
            axisLine: {
              show: true,
              lineStyle: {
                color: "#092b5d"
              }
            },
            axisLabel: {
              show: false,
              textStyle: {
                color: "#24c4ff"
              }
            },
            axisTick: {
              show: false
            }
          }
        ],
        series: [
          {
            name: "注册总量",
            type: "line",
            symbol: "circle", // 默认是空心圆（中间是白色的），改成实心圆
            // showAllSymbol: false,
            showSymbol: false,
            smooth: 0.5,
            lineStyle: {
              normal: {
                color: "#00FFFF" // 线条颜色
              },
              borderColor: "rgba(0,0,0,.4)"
            },
            itemStyle: {
              color: "rgba(0,255,255,1)",
              borderColor: "#FFF",
              borderWidth: 2
            },
            label: {
              normal: {
                show: true,
                position: "top",
                formatter: [" {a|{c}%}"].join(","),
                rich: {
                  a: {
                    color: "#fff",
                    align: "center"
                  }
                }
              }
            },
            tooltip: {
              show: true
            },
            areaStyle: {
              //区域填充样式
              normal: {
                //线性渐变，前4个参数分别是x0,y0,x2,y2(范围0~1);相当于图形包围盒中的百分比。如果最后一个参数是‘true’，则该四个值是绝对像素位置。
                color: new this.$echarts.graphic.LinearGradient(
                  0,
                  0,
                  0,
                  1,
                  [
                    {
                      offset: 0,
                      color: "rgba(0,255,255,.3)"
                    },
                    {
                      offset: 1,
                      color: "rgba(0,255,255, 0)"
                    }
                  ],
                  false
                ),
                shadowColor: "rgba(53,142,215, 0.9)", //阴影颜色
                shadowBlur: 20 //shadowBlur设图形阴影的模糊大小。配合shadowColor,shadowOffsetX/Y, 设置图形的阴影效果。
              }
            },
            data: [12, 40, 50, 80, 90, 30, 20, 12, 40, 50, 80, 90, 30 ]
          }
        ]
      });
    }
  },
  components: {}
};
</script>

<style lang="less" scoped>
.amount-trends-chart{
  width: 100%;
  height: 30%;
}
</style>
