<template>
  <div class="brain-content" @keyup.enter="keyEnter">
    <div class="brain-main">
      <div id="ChinaMap" v-if="showAudio"></div>
      <div class="light-spot">
        <canvas id="audio-art"></canvas>
      </div>
      <img :src="robot" alt id="robot-img" v-if="!showAudio" />
      <!-- <div class="topCity">
        <RiskPortraitChart :ids="idRisk" :tableDatas="tableDataR" />
      </div> -->
    </div>
    <div class="brain-foot">
      <audio id="audio" ref="audio" :src="require('../../assets/video/shanxi.wav')"></audio>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import a from "../../assets/images/brain-a.png";
import b from "../../assets/images/brain-b.png";
import c from "../../assets/images/brain-c.png";
import d from "../../assets/images/brain-d.png";
import e from "../../assets/images/brain-e.png";
import f from "../../assets/images/brain-f.png";
import g from "../../assets/images/brain-g.png";
import RiskPortraitChart from "@/components/p4/line-toRight.vue";
import * as baseBox from "@/assets/base64.js";
export default {
  name: "tableAuto",
  props: ["tableDatas", "ids", "heights", "bigPoint"],
  data() {
    return {
      tableData: [],
      id: this.ids,
      height: this.heights,
      lineData: [],
      showAudio: true,
      analyser: {},
      xy: "",
      robot: baseBox.robot.value,
      idRisk: {
        id: "echarts05",
        style: "height: 100%",
        title: '客群城市排行'
      },
      tableDataR: [
        {
          area: "Top5 南京",
          percent: 343
        },
        {
          area: "Top4 福州",
          percent: 234
        },
        {
          area: "Top3 杭州",
          percent: 134
        },
        {
          area: "Top2 贵阳",
          percent:90
        },
        {
          area: "Top1 厦门",
          percent: 56
        }
      ]
    };
  },
  computed: {},
  mounted() {
    this.init();
  },
  watch: {
    bigPoint: function(a, b) {
      this.tableData = a;
      if (!this.showAudio) {
        return;
      }
      this.drawMap();
    }
  },
  methods: {
    init() {
      this.drawMap();
      
    },
    drawMap() {
      let obj = this.tableData,
        seriesData = {
          ...obj,
          value: [obj.longitude, obj.latitude]
        };
      let myChart = this.$echarts.init(document.getElementById("ChinaMap"));
      let mapName = "china";
      var data = [
        { name: "北京", value: 199 },
        { name: "天津", value: 42 },
        { name: "河北", value: 102 },
        { name: "山西", value: 81 },
        { name: "内蒙古", value: 47 },
        { name: "辽宁", value: 67 },
        { name: "吉林", value: 82 },
        { name: "黑龙江", value: 123 },
        { name: "上海", value: 24 },
        { name: "江苏", value: 92 },
        { name: "浙江", value: 114 },
        { name: "安徽", value: 139 },
        { name: "福建", value: 116 },
        { name: "江西", value: 91 },
        { name: "山东", value: 119 },
        { name: "河南", value: 137 },
        { name: "湖北", value: 116 },
        { name: "湖南", value: 114 },
        { name: "重庆", value: 91 },
        { name: "四川", value: 125 },
        { name: "贵州", value: 62 },
        { name: "云南", value: 83 },
        { name: "西藏", value: 9 },
        { name: "陕西", value: 80 },
        { name: "甘肃", value: 56 },
        { name: "青海", value: 10 },
        { name: "宁夏", value: 18 },
        { name: "新疆", value: 18 },
        { name: "广东", value: 183 },
        { name: "香港", value: 203 },
        { name: "澳门", value: 199 },
        { name: "广西", value: 59 },
        { name: "海南", value: 14 }
      ];

      var geoCoordMap = {};

      /*获取地图数据*/
      var mapFeatures = this.$echarts.getMap(mapName).geoJson.features;
      mapFeatures.forEach(function(v) {
        // 地区名称
        var name = v.properties.name;
        // 地区经纬度
        geoCoordMap[name] = v.properties.cp;
      });
      var max = 480,
        min = 9; // todo
      var maxSize4Pin = 100,
        minSize4Pin = 20;

      var convertData = function(data) {
        var res = [];
        for (var i = 0; i < data.length; i++) {
          var geoCoord = geoCoordMap[data[i].name];
          if (geoCoord) {
            res.push({
              name: data[i].name,
              value: geoCoord.concat(data[i].value)
            });
          }
        }
        return res;
      };
      var option = {
        visualMap: {
          show: false,
          min: 0,
          max: 200,
          left: "10%",
          top: "bottom",
          calculable: true,
          seriesIndex: [1],
          inRange: {
            color: ["#467bc0", "#04387b"] // 蓝绿
          }
        },
        geo: {
          show: true,
          map: mapName,
          label: {
            normal: {
              show: false
            },
            emphasis: {
              show: false
            }
          },
          roam: false,
          itemStyle: {
            normal: {
              areaColor: "#023677",
              borderColor: "#1180c7"
            },
            emphasis: {
              areaColor: "#4499d0"
            }
          }
        },
        series: [
          {
            name: "散点",
            type: "scatter",
            coordinateSystem: "geo",
            data: convertData(data),
            symbolSize: 0,
            label: {
              normal: {
                formatter: "{b}",
                position: "right",
                show: false
              },
              emphasis: {
                show: false
              }
            },
            itemStyle: {
              normal: {
                show: false,
                color: "#fff"
              }
            }
          },
          {
            type: "map",
            map: mapName,
            geoIndex: 0,
            aspectScale: 0.75, //长宽比
            showLegendSymbol: false, // 存在legend时显示
            label: {
              normal: {
                show: true
              },
              emphasis: {
                show: false,
                textStyle: {
                  color: "#fff"
                }
              }
            },
            roam: true,
            itemStyle: {
              normal: {
                areaColor: "#031525",
                borderColor: "#3B5077"
              },
              emphasis: {
                areaColor: "#2B91B7"
              }
            },
            animation: false,
            data: data
          },
          {
            type: "effectScatter",
            coordinateSystem: "geo",
            zlevel: 10,
            data: [seriesData],
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
                    accent,
                    mobile,
                    product,
                    want,
                    status,
                    action,
                    handle,
                    problem
                  } = params.data;
                  let a = "";
                  if (accent) {
                    a = `{a|口音识别}{b|${accent}}{a|产品}{b|${product}}\n{a|客户意图}{b|${want}}\n{a|手机号码}{b|${mobile}}`;
                  } else {
                    a = `{a|行为}{b|${action}}{a|状态}{b|${status[0].value}}\n{a|问题定位}{b|${problem}}{c|智能处理}{b|${handle}}\n{a|手机号码}{b|${mobile}}`;
                  }
                  return a;
                },
                position: [0, 0],
                distance: 0,
                width: 340,
                height: 170,
                backgroundColor: {
                  image: require("@/assets/images/p3/map-modal.png")
                },
                padding: [30, 50],
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
                    align: "left"
                  }
                }
              }
            }
          },
          {
            name: "Top 5",
            type: "effectScatter",
            coordinateSystem: "geo",
            data: convertData(
              data
                .sort(function(a, b) {
                  return b.value - a.value;
                })
                .slice(0, 10)
            ),
            symbolSize: function(val) {
              return val[2] / 8;
            },
            showEffectOn: "render",
            rippleEffect: {
              brushType: "stroke"
            },
            hoverAnimation: true,
            label: {
              normal: {
                formatter: "{b}",
                position: "left",
                show: false
              }
            },
            itemStyle: {
              normal: {
                color: "yellow",
                shadowBlur: 10,
                shadowColor: "yellow"
              }
            },
            zlevel: 1
          }
        ]
      };
      myChart.setOption(option);
    },
    transLateData() {
      let obj = {
        type: "bar",
        yAxis: [],
        series: []
      };
      this.tableData.forEach(el => {
        obj.series.push(el.num);
        obj.yAxis.push(el.categroy);
      });
      this.lineData = obj.series;
      this.drawLineG(this.id.id, obj);
    },
    DrawVideo() {
      this.$emit("func", { value: false });
      var atx = new (window.AudioContext || webkitAudioContext)();
      // var audio = document.getElementById("audio");
      let audio = new Audio();
      audio.src = require("../../assets/video/shanxi.wav");
      var canvas = document.getElementById("audio-art");
      var ctx = canvas.getContext("2d");

      var source = atx.createMediaElementSource(audio);
      var analyser = atx.createAnalyser();
      source.connect(analyser);
      analyser.connect(atx.destination);

      analyser.fftSize = 2048;
      var draw = () => {
        var cWidth = canvas.width,
          cHeight = canvas.height * 1.5,
          // frequencyBinCount的值固定为fftSize的一半
          audioArray = new Uint8Array(analyser.frequencyBinCount);
        // 解析频率数据，放入audioArray数组中
        analyser.getByteFrequencyData(audioArray);
        // 填充为柱状图
        ctx.clearRect(0, 0, cWidth, cHeight);
        ctx.fillStyle = "#60D7FB";
        for (var i = 0; i < audioArray.length; i++) {
          ctx.fillRect(i * 3, cHeight - audioArray[i], 2, cHeight);
        }
        // 刷新
        requestAnimationFrame(draw);
      };
      draw();
      audio.play();
      let audioTime = this.$refs.audio.duration;
      setTimeout(() => {
        this.$emit("func", { value: true });
        this.$nextTick(() => {
          this.showAudio = true;
        });
      }, audioTime * 1000);
    }
  },
  components: {
    RiskPortraitChart
  }
};
</script>
<style lang="less" scoped>
.brain-content {
  font-size: 12px;
  background-size: 40%;
  position: relative;
  height: 100%;
  .brain-main {
    height: 85%;
    position: relative;
    #ChinaMap {
      height: 100%;
    }
  }
  #robot-img {
    width: 100px;
    position: absolute;
    bottom: 17%;
    left: 9%;
  }
  .light-spot {
    width: 92%;
    margin: 0 auto;
    min-height: 400px;
    min-height: 450px;
    position: absolute;
    top: 4%;
    .spot-box {
      div {
        display: inline-block;
        position: absolute;
        height: 54%;
        width: 30%;
        padding: 2.4% 3%;
        z-index: 2;
      }
    }
    audio {
      display: none;
    }
    i {
      font-style: normal;
    }
    .a-address {
      width: 80%;
      display: inline-block;
      height: 30px;
      background: url("../../assets/images/p4icon1.png") no-repeat center;
      background-position: 4px 0;
      background-size: 10%;
      padding: 2px 38px;
      line-height: 26px;
    }
    .e-address {
      width: 80%;
      display: inline-block;
      height: 30px;
      background: url("../../assets/images/p4icon4.png") no-repeat center;
      background-position: 4px 0;
      background-size: 8%;
      padding: 2px 0 0px 40px;
      line-height: 26px;
    }
    .f-address {
      width: 80%;
      display: inline-block;
      height: 30px;
      background: url("../../assets/images/p4icon1.png") no-repeat center;
      background-position: 4px 0;
      background-size: 10%;
      padding: 2px 0 0 38px;
      line-height: 26px;
    }
    .a-tel {
      display: inline-block;
      width: 80%;
      margin-top: 4%;
    }
    .a-want {
      color: #4dc4d3;
    }
    .e-want {
      color: #ffe96f;
    }
    .a {
      top: 338px;
      left: 446px;
      background: url("../../assets/images/popup-p.png") no-repeat center;
      background-size: 100%;
    }
    .spot-box .b {
      top: 396px;
      left: 190px;
      background: url("../../assets/images/popup-l.png") no-repeat center;
      background-size: 100%;
    }
    .spot-box .c {
      top: 730px;
      left: 511px;
      background: url("../../assets/images/popup-p.png") no-repeat center;
      background-size: 100%;
    }
    .spot-box .d {
      top: 469px;
      left: 936px;
      background: url("../../assets/images/popup-o.png") no-repeat center;
      background-size: 100%;
      padding-left: 126px;
    }
    .spot-box .e {
      top: 236px;
      right: 374px;
      background: url("../../assets/images/popup-r.png") no-repeat center;
      background-size: 100%;
      padding-left: 128px;
    }
    .spot-box .f {
      top: 616px;
      right: 284px;
      background: url("../../assets/images/popup-o.png") no-repeat center;
      background-size: 100%;
      padding-left: 128px;
    }
  }
  .brain-foot {
    position: absolute;
    bottom: 0px;
    width: 100%;
    text-align: center;
    .foot-ai {
      display: inline-block;
      width: 30%;
      height: 60px;
      background: url("../../assets/images/ai-foot.png") no-repeat center;
      background-size: 50%;
    }
    ul {
      display: flex;
      justify-content: center;
      width: 90%;
      margin: 0 auto;
      li {
        list-style: none;
        display: inline-block;
        width: 10%;
        margin-right: 30px;
        text-align: center;
        img {
          width: 100%;
          margin-bottom: 10px;
        }
      }
    }
  }
  .video {
    display: none;
  }
  #audio-art {
    position: absolute;
    width: 60%;
    top: 130px;
    left: 23%;
    height: 172%;
    margin: 0 auto;
    z-index: 1;
  }
}
.topCity {
  position: absolute;
  height: 30%;
  bottom: -18%;
  width: 30%;
  .person-columnar {
    height: 100%;
  }
}
</style> 