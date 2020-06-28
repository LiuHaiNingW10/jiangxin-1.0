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
              borderColor: "rgba(147, 235, 248, 0)",
              borderWidth: 0.5,
              areaColor: {
                x: 1000,
                y: 1000,
                x2: 1000,
                y2: 0,
                colorStops: [
                  {
                    offset: 0,
                    color: "#69c5d8" // 0% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "#126caf" // 50% 处的颜色
                  }
                ],
                global: true // 缺省为 false
              },
              opacity: 1
            },
            emphasis: {
              show: false,
              areaColor: "#69c5d8"
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
            label: {
              emphasis: {
                show: false,
                color: "#fff"
              }
            },
            top: "9.7%",
            left: "12.5%",
            aspectScale: 0.75,
            roam: false,
            itemStyle: {
              normal: {
                borderColor: "#2cb3dd",
                borderWidth: 0.8,
                areaColor: {
                  type: "linear-gradient",
                  x: 1000,
                  y: 600,
                  x2: 1000,
                  y2: 0,
                  colorStops: [
                    {
                      offset: 0,
                      color: "#274d68" // 0% 处的颜色
                    },
                    {
                      offset: 1,
                      color: "#09132c" // 50% 处的颜色
                    }
                  ],
                  global: true // 缺省为 false
                }
              },
              emphasis: {
                show: false,
                areaColor: "#274d62"
              }
            },
            zlevel: 1
          },
          {
            type: "effectScatter",
            coordinateSystem: "geo",
            z: 1,
            data: [
              {
                name: "北京",
                username: "王先生",
                telphone: "176****9692",
                address: "在陕西-奔驰4S店购买车辆",
                value: 333
              }
            ],
            symbolSize: 14,
            label: {
              normal: {
                show: true,
                formatter: function() {
                  return "{fline|客户：99}";
                },
                position: "top",
                backgroundColor: "rgba(8,186,236,.9)",
                padding: [0, 0],
                borderRadius: 3,
                lineHeight: 32,
                color: "#ffffff",
                rich: {
                  fline: {
                    padding: [0, 10, 10, 10],
                    color: "#ffffff"
                  },
                  tline: {
                    padding: [10, 10, 0, 10],
                    color: "#ffffff"
                  }
                }
              },
              emphasis: {
                show: true
              }
            },
            itemStyle: {
              color: "#08baec"
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
