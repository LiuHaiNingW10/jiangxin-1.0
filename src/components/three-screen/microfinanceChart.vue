<template>
  <div class="microfinance-chart">
    <div class="mc-header">
      近一小时攻击数
      <span>{{ 537 || attckTimes}}次/小时</span>
    </div>
    <div class="mc-content">
      <div class="mc-l">
        <span class="left-title">FRIM命中分布</span>
        <ul>
          <li :style="moveLocaiton === 1 ? 'color: #fff' : 'color: gray'">
            <img src="../../assets/images/p3/frm-cricle1.png" alt />
            <div>
              <div class="title">{{IDFdatas[0].flag}} {{IDFdatas[0].percent || 0}}%</div>
            </div>
          </li>
          <li :style="moveLocaiton === 2 ? 'color: #fff' : 'color: gray'">
            <img src="../../assets/images/p3/frm-cricle2.png" alt />
            <div>
              <div class="title">{{IDFdatas[1].flag}} {{IDFdatas[1].percent || 0}}%</div>
            </div>
          </li>
          <li :style="moveLocaiton === 3 ? 'color: #fff' : 'color: gray'">
            <img src="../../assets/images/p3/frm-cricle1.png" alt />
            <div>
              <div class="title">{{IDFdatas[2].flag}} {{IDFdatas[2].percent || 0}}%</div>
            </div>
          </li>
          <li :style="moveLocaiton === 4 ? 'color: #fff' : 'color: gray'">
            <img src="../../assets/images/p3/frm-cricle2.png" alt />
            <div>
              <div class="title">{{IDFdatas[3].flag}} {{IDFdatas[3].percent || 0}}%</div>
            </div>
          </li>
        </ul>
      </div>
      <div class="mc-c" :style="'background:url('+ frm_cc+');background-size: 100% 100%;'"></div>
      <div class="mc-r">
        <div class="title">F.R.I.M命中次数</div>
        <div class="frim-chart" id="frimChart"></div>
      </div>
    </div>
    <div class="mc-footer">
      欺诈损失率
      <span>{{footerData}}% ，行业平均水平为0.0001%</span>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import * as base64 from "@/assets/base64.js";
export default {
  data() {
    return {
      footerdata: "",
      attcktimes: "",
      showAttack: false,
      moveLocaitons: 1,
      IDFdata: [
        { flag: "名单层", callNum: 300, percent: 30 },
        { flag: "聚集层", callNum: 300, percent: 30 },
        { flag: "场景层", callNum: 200, percent: 20 },
        { flag: "动态层", callNum: 200, percent: 20 }
      ],
      frm_cc: base64.frm_cc.value
    };
  },
  computed: {
    footerData() {
      return this.footerdata;
    },
    attckTimes() {
      return this.attcktimes;
    },
    moveLocaiton: {
      get() {
        return this.moveLocaitons;
      }
    },
    IDFdatas() {
      return this.IDFdata;
    }
  },
  mounted() {
    this.getLossRate();
    this.getAttackRecent();
    this.timer = setInterval(() => {
      setTimeout(this.getAttackRecent(), 0);
    }, 60000);
    this.getIDF();
    this.timerIDF = setInterval(() => {
      this.getIDF();
    }, 60000);
    this.timeTomove();
  },
  methods: {
    getIDF() {
      this.axios
        .get("/api/p3/riskIdfMatrix")
        .then(response => {
          // this.IDFdata = response.data.data;
          this.drawChart();
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    getLossRate() {
      //欺诈损失率
      this.axios
        .get("/api/p3/lossRate")
        .then(response => {
          this.footerdata = response.data.data;
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    timeTomove() {
      setTimeout(() => {
        this.timeinterval = setInterval(() => {
          if (this.moveLocaitons === 4) {
            this.moveLocaitons = 1;
          } else {
            this.moveLocaitons += 1;
          }
        }, 2000);
      }, 50);
    },
    getAttackRecent() {
      //近一小时攻击数P3-4
      this.axios
        .get("/api/p3/attackRecent")
        .then(response => {
          this.attcktimes = response.data.data;
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    drawChart() {
      let myChart = this.$echarts.init(document.getElementById("frimChart"));
      let percentData = [];
      this.IDFdata.forEach(el => {
        percentData.push(el.callNum);
      });
      let max = Vue.filter("sortByValue")(this.IDFdata, "callNum")[0].callNum;
      // 绘制图表
      myChart.setOption({
        color: [
          {
            type: "linear",
            x: 0,
            y: 0,
            x2: 1,
            y2: 0,
            colorStops: [
              {
                offset: 0,
                color: "#1F7CFF" // 0% 处的颜色
              },
              {
                offset: 0.9,
                color: "#2BF1FF"
              },
              {
                offset: 1,
                color: "#FFFFFF " // 100% 处的颜色
              }
            ],
            global: false // 缺省为 false
          }
        ],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow"
          },
          formatter: function(params) {
            return (
              params[0].name +
              "<br/>" +
              "<span style='display:inline-block;margin-right:5px;border-radius:10px;width:9px;height:9px;background-color:rgba(36,207,233,0.9)'></span>" +
              params[0].seriesName +
              " : " +
              Number(
                (params[0].value.toFixed(4) / 10000).toFixed(2)
              ).toLocaleString() +
              " 万元<br/>"
            );
          }
        },
        grid: {
          top: "10%",
          left: "10%",
          right: "15%",
          bottom: "10%",
          containLabel: true
        },
        xAxis: {
          show: false,
          type: "value",
          boundaryGap: [0, 0.01],
          axisLabel: {
            color: "rgba(255,255,255)"
          },
          splitLine: {
            show: false
          }
        },
        yAxis: [
          {
            type: "category",
            show: false
          }
        ],
        series: [
          {
            name: "拦截金融",
            type: "bar",
            zlevel: 1,
            label: {
              show: true,
              position: "right",
              fontSize: 16,
              color: "rgba(255,255,255)",
              formatter: function(data) {
                return data.callNum;
              }
            },
            barWidth: 30,
            data: percentData || [15, 10, 60, 5]
          },
          {
            name: "背景",
            type: "bar",
            barWidth: 30,
            barGap: "-100%",
            data: [max * 1.6, max * 1.6, max * 1.6, max * 1.6],
            itemStyle: {
              color: "rgba(255,255,255,0)",
              // barBorderRadius: 30,
              borderWidth: 1,
              borderColor: "rgba(255,255,255,.4)"
            }
          }
        ]
      });
    }
  },
  beforeDestroy() {
    clearInterval(this.timeinterval);
    clearInterval(this.timer);
    clearInterval(this.timerIDF);
  },
  components: {}
};
</script>

<style lang="less" scoped>
.microfinance-chart {
  width: 80%;
  margin: 0 auto;
  height: 30%;
  .mc-header,
  .mc-footer {
    font-size: 18px;
    color: rgba(255, 255, 255, 0.72);
    text-align: center;
    & > span {
      font-size: 24px;
      color: #fff;
      margin-left: 8px;
    }
  }
  .mc-content {
    display: flex;
    overflow: hidden;
    justify-content: space-between;
    .mc-l {
      margin-top: 25px;
      color: gray;
      .left-title {
        color: rgba(255, 255, 255, 0.7);
        font-size: 18px;
      }
      ul {
        list-style: none;
        li {
          width: 291px;
          height: 68px;
          background: url("../../assets/images/p3/frm-l.png") no-repeat;
          overflow: hidden;
          margin-bottom: 28px;
          img {
            float: left;
            width: 38px;
            height: 38px;
            margin: 15px;
          }
          .title {
            font-size: 20px;
            line-height: 1;
            margin-top: 20px;
          }
          .desc {
            font-size: 14px;
            line-height: 1;
            & > span {
              font-size: 24px;
              margin-left: 7px;
            }
          }
        }
      }
    }
    .mc-c {
      width: 212px;
      height: 450px;
      // background: url('../../assets/images/p3/frm-cc.png') no-repeat;
      background-size: 100% 100%;
      margin: 0 46px 0 27px;
    }
    .mc-r {
      margin-top: 40px;
      width: 20%;
      height: 410px;
      background: url("../../assets/images/p3/frm-r.png") no-repeat;
      background-size: 100% 93%;
      padding: 12px 0;
      text-align: center;
      position: relative;
      .title {
        font-size: 18px;
        color: rgba(255, 255, 255, 0.7);
        margin-bottom: 10px;
      }
      .frim-chart {
        position: absolute;
        top: -25px;
        left: 15px;
        width: 140px;
        height: 480px;
      }
      ul > li {
        list-style: none;
        font-size: 20px;
        color: #fff;
        margin-bottom: 65px;
      }
    }
  }
}
</style>
