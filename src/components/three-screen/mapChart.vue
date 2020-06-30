<template>
  <div class="map-charts">
    <div id="middleMap"></div>
  </div>
</template>

<script>
export default {
  name: "mapChart",
  data() {
    return {};
  },
  mounted() {
    this.getData()
  },
  methods: {
    getData () {
      this.axios.get('/api/p3/mapData')
      .then( (res)  => {
        const { data } = res.data
        let index = 0
        let timer = setInterval(()=>{
          if(index>=data.length) {
            this.getData()
            clearInterval(timer)
          }
          index ++ 
          this.initMap(data, index)
        }, 2000)
        
      })
      .catch(function (error) {
          console.log(error);
      });
    },
    initMap(data, index) {
      data = data.map(item=>{
        return {
          ...item,
          value: [item.longitude, item.latitude]
        }
      })
      let myEcharts = this.$echarts.init(document.getElementById("middleMap"));
      myEcharts.setOption({
        legend: {
          show: false
        },
        tooltip: {
          show: true
        },
        geo: {
          map: "china",
          aspectScale: 0.75,
          label: {
            emphasis: {
              show: false
            }
          },
          regions: [
            {
              name: "南海诸岛",
              value: 0,
              itemStyle: {
                normal: {
                  opacity: 0,
                  label: {
                    show: false
                  }
                }
              }
            }
          ],
          itemStyle: {
            normal: {
              borderColor: "rgba(28, 180, 0, 0)",
              borderWidth: 0.5,
              areaColor: {
                x: 1000,
                y: 1000,
                x2: 1000,
                y2: 0,
                colorStops: [
                  {
                    offset: 0,
                    // color: "#69c5d8" // 0% 处的颜色
                    color: "rgba(170,170,170, 0.5)"
                  },
                  {
                    offset: 1,
                    // color: "#126caf" // 50% 处的颜色
                    color: "rgba(170,170,170, 0.5)"
                  }
                ],
                global: true // 缺省为 false
              },
              opacity: 1
            },
            emphasis: {
              show: false,
              // areaColor: "#69c5d8"
              areaColor: "rgba(170,170,170, 0.5)"
            }
          },
          z: 2
        },
        series: [
          {
            type: "map",
            map: "china",
            tooltip: {
              show: false
            },
            top: "9.5%",
            aspectScale: 0.75,
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false,
                textStyle: {
                  color: "#fff"
                }
              }
            },
            roam: false,
            itemStyle: {
              color: 'red',
              areaColor: "rgba(15,50,255,0)",
              borderColor: "#00FFFF",
              borderWidth: 1,
              emphasis: {
                areaColor: "#00FFFF"
              }
            },
            animation: false,
            zlevel: 1
          },
          {
            type: "effectScatter",
            coordinateSystem: "geo",
            zlevel: 10,
            data: data,
            symbolSize: function(value, params){
              const { rejamt } = params.data
              let num = parseInt(rejamt.replace(/','/g,''))
              var max = Math.max.apply(null, data.map(item=>parseInt(item.rejamt.replace(/','/g,''))));
              let size = 20/max*num
              let result = size > 5 ? size : 5
              return result
            },
            showEffectOn: "render",
            // 涟漪的设置
            rippleEffect: {
              color: "#F5B523",
              scale: 4,
              brushType: "stroke"
            },
            itemStyle: {
              normal: {
                color: "#F5B523",
                shadowBlur: 2
              }
            }
          },
          {
            type: "effectScatter",
            coordinateSystem: "geo",
            zlevel: 10,
            data: [data[index]],
            itemStyle: {
              normal: {
                color: "#F5B523",
                shadowBlur: 2
              }
            },
            // 标签
            label: {
              normal: {
                show: true,
                formatter: function(params) {
                  const { location, eventid, appname, risktype, rejamt } = params.data
                  return `
                    区域名称：${location} 交易类型：${eventid}
                    渠道类型: ${appname} 交易时间：--
                    事件类型：${risktype} 
                    实时拦截金融：${rejamt}
                  `
                },
                position: [-340, -70],
                distance: 0,
                width: 300,
                height: 160,
                backgroundColor: {
                  image: require('@/assets/images/p3/map-modal.png')
                },
                padding: [20, 30],
                lineHeight: 24,
                verticalAlign: "middle",
                color: "#fff",
                z: 11,
                rich: {
                  fline: {
                    padding: [0, 10],
                    color: "#ffffff"
                  }
                }
              }
            }
          }
        ]
      });
    }
  },
  components: {}
};
</script>

<style lang="less">
.map-charts,
#middleMap {
  height: 100%;
  width: 100%;
}
</style>
