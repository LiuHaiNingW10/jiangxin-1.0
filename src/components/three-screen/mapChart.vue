<template>
  <div class="map-charts">
    <div id="threeMapChart"></div>
  </div>
</template>

<script>
export default {
  name: "mapChart",
  data() {
    return {
      timer: null,
      relationRes: []
    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    getData() {
      let _that = this;
      this.axios.get("/api/p3/relationmapData").then(relaRes => {
        let relaData = relaRes.data.data;
        _that.relationRes = relaData ? relaData : [];
        _that.axios
          .get("/api/p3/mapData")
          .then(res => {
            const { data } = res.data;
            let index = 0;
            let timer = setInterval(() => {
              if (index >= data.length) {
                _that.getData();
                clearInterval(timer);
              }
              index++;
              _that.initMap(data, index);
            }, 3000);
          })
          .catch(function(error) {
            console.log(error);
          });
      });
    },
    initMap(data, index) {
      let relationMesg = {};
      this.relationRes.forEach(item => {
        if (item.location === data[index].location) {
          relationMesg =
            item.relationInfoList && item.relationInfoList.length > 0
              ? Object.assign({}, item)
              : {};
        }
      });
      // console.log([relationMesg], "rsm");
      // console.log([data[index]], "di");
      data = data.map(item => {
        return {
          ...item,
          value: [item.longitude, item.latitude]
        };
      });
      relationMesg = {
        ...relationMesg,
        value: [relationMesg.longitude, relationMesg.latitude]
      };
      let myEcharts = this.$echarts.init(
        document.getElementById("threeMapChart")
      );
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
              borderColor: "rgba(0,255,255, 1)",
              borderWidth: 1,
              areaColor: {
                type: "radial",
                x: 0.5,
                y: 0.5,
                r: 0.8,
                colorStops: [
                  {
                    offset: 0,
                    color: "rgba(0,255,255, 0)" // 0% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "rgba(0,255,255, .2)" // 100% 处的颜色
                  }
                ],
                globalCoord: false // 缺省为 false
              },
              shadowColor: "rgba(128, 217, 248, 1)",
              // shadowColor: 'rgba(255, 255, 255, 1)',
              shadowOffsetX: -2,
              shadowOffsetY: 2,
              shadowBlur: 10
            },
            emphasis: {
              areaColor: "#389BB7",
              borderWidth: 0
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
              normal: {
                // areaColor: "rgba(0, 187, 255, .3)",
                // borderColor: "#00FFFF",
                areaColor: "rgba(0, 187, 255, .0)",
                borderColor: "rgba(0, 187, 255, .0)",
                borderWidth: 1
              },
              emphasis: {
                // areaColor: "#00FFFF"
                areaColor: "rgba(0, 187, 255, .0)"
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
            symbolSize: function(value, params) {
              const { rejamt } = params.data;
              let num = parseInt(rejamt.replace(/','/g, ""));
              var max = Math.max.apply(
                null,
                data.map(item => parseInt(item.rejamt.replace(/','/g, "")))
              );
              let size = (20 / max) * num;
              let result = size > 5 ? size : 5;
              return result;
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
                  const {
                    location,
                    eventid,
                    appname,
                    risktype,
                    rejamt
                  } = params.data;
                  return `{a|区域名称}{b|${location}}{a|交易类型}{b|${eventid}}\n{a|渠道类型}{b|${appname}}{a|事件类型}{b|${risktype}}\n{c|实时拦截金额}{d|${rejamt}万}`;
                },
                position: [-400, -150],
                distance: 0,
                width: 340,
                height: 120,
                backgroundColor: {
                  image: require("@/assets/images/p3/map-modal.png")
                },
                padding: [30, 40],
                lineHeight: 40,
                // verticalAlign: "middle",
                color: "#fff",
                z: 11,
                rich: {
                  a: {
                    color: "rgba(255,255,255,.7)",
                    fontSize: 16
                  },
                  b: {
                    padding: [0, 10],
                    color: "#ffffff",
                    fontSize: 16,
                    fontWeight: "bold"
                  },
                  c: {
                    margin: 20,
                    fontSize: 16,
                    color: "rgba(255,255,255,.9)"
                  },
                  d: {
                    padding: [0, 10],
                    fontSize: 20,
                    color: "#FFAF2B"
                  }
                }
              }
            }
          },
          {
            // 关联关系
            type: "effectScatter",
            coordinateSystem: "geo",
            zlevel: 10,
            data: [relationMesg],
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
                  const { relationInfoList } = params.data;
                  // return `{a|区域名称}{b|${location}}{a|交易类型}{b|${eventid}}\n{a|渠道类型}{b|${appname}}{a|事件类型}{b|${risktype}}\n{c|实时拦截金额}{d|${rejamt}万}`;
                  let tmp = relationInfoList.map((item, index) => {
                    return (
                      item.relate_name +
                      "、" +
                      item.relate_age +
                      "、" +
                      item.relate_location +
                      "、处理方式：" +
                      item.dealtypename +
                      (index < relationInfoList.length - 1 ? "\n" : "")
                    );
                  });
                  let returnStr = tmp.join("").replace(/','/g, "");
                  return returnStr;
                  // return `aaaaaa`;
                },
                position: [0, -150],
                distance: 0,
                width: 340,
                height: 120,
                backgroundColor: {
                  image: require("@/assets/images/p3/map-modal.png")
                },
                padding: [30, 40],
                lineHeight: 40,
                // verticalAlign: "middle",
                color: "#fff",
                z: 11
                // rich: {
                //   a: {
                //     color: "rgba(255,255,255,.7)",
                //     fontSize: 16
                //   },
                //   b: {
                //     padding: [0, 10],
                //     color: "#ffffff",
                //     fontSize: 16,
                //     fontWeight: "bold"
                //   },
                //   c: {
                //     margin: 20,
                //     fontSize: 16,
                //     color: "rgba(255,255,255,.9)"
                //   },
                //   d: {
                //     padding: [0, 10],
                //     fontSize: 20,
                //     color: "#FFAF2B"
                //   }
                // }
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
#threeMapChart {
  height: 100%;
  width: 100%;
}
</style>
