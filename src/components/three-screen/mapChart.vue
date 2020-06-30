<template>
  <div class="map-charts">
    <div id="middleMap"></div>
  </div>
</template>

<script>
export default {
  name: "mapChart",
  props: [],
  mounted() {
    this.initMap();
  },
  data() {
    return {};
  },
  computed: {},
  methods: {
    initMap() {
      var geoCoordMap = {
        上海: [119.1803, 31.2891],
        福建: [119.4543, 25.9222],
        重庆: [108.384366, 30.439702],
        北京: [116.4551, 40.2539],
        辽宁: [123.1238, 42.1216],
        河北: [114.4995, 38.1006],
        天津: [117.4219, 39.4189],
        山西: [112.3352, 37.9413],
        陕西: [109.1162, 34.2004],
        甘肃: [103.5901, 36.3043],
        宁夏: [106.3586, 38.1775],
        青海: [101.4038, 36.8207],
        新疆: [87.9236, 43.5883],
        西藏: [91.11, 29.97],
        四川: [103.9526, 30.7617],
        吉林: [125.8154, 44.2584],
        山东: [117.1582, 36.8701],
        河南: [113.4668, 34.6234],
        江苏: [118.8062, 31.9208],
        安徽: [117.29, 32.0581],
        湖北: [114.3896, 30.6628],
        浙江: [119.5313, 29.8773],
        内蒙古: [110.3467, 41.4899],
        江西: [116.0046, 28.6633],
        湖南: [113.0823, 28.2568],
        贵州: [106.6992, 26.7682],
        云南: [102.9199, 25.4663],
        广东: [113.12244, 23.009505],
        广西: [108.479, 23.1152],
        海南: [110.3893, 19.8516],
        黑龙江: [127.9688, 45.368],
        台湾: [121.4648, 25.563]
      };

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
            data: [
              {
                name: "王**",
                age: "28岁",
                sex: "男",
                type: "授信申请",
                sum: "3000",
                value: [116.4551, 40.2539, 48]
              },
              {
                name: "王**",
                age: "25岁",
                sex: "女",
                type: "授信申请",
                sum: "7000",
                value: [103.9526, 30.7617, 48]
              }
            ],
            symbolSize: 20,
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
            },
            // 标签
            label: {
              normal: {
                show: true,
                formatter: function(params) {
                  return (
                    params.data.name +
                    "   " +
                    params.data.age +
                    "   " +
                    params.data.sex +
                    "\n" +
                    "交易类型：" +
                    params.data.type +
                    "\n" +
                    "交易金额：¥" +
                    params.data.sum
                  )
                },
                position: [-330, -60],
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
