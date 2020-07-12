<template>
  <div class="common-box" :style="id.style">
      <div :id="id.id"></div>
  </div>
</template>
<script>
import Vue from "vue";
export default {
  name: "tableAuto",
  props: ["tableDatas", "ids", "columns", "heights","isShowPercent"],
  data() {
    return {
      tableData: this.tableDatas,
      id: this.ids,
      height: this.heights,
      lineData: []
    };
  },
  computed: {},
  mounted() {
    this.transLateData();
  },
  methods: {
    transLateData() {
      let obj = {
        type: "bar",
        yAxis: [],
        series: []
      };
      let arr = Vue.filter('sortByValue')(this.tableData,'percent')
      arr.forEach(el => {
        obj.series.push(el.percent.toFixed(2));
        obj.yAxis.push(el.indexname);
      });
      this.drawLineG(this.id, obj);
    },
    drawLineG(ele, data) {
      var _this = this;
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById(ele.id));
      // 绘制图表
      myChart.setOption({
        title: {
          text: [`{b|${ele.title}}`].join(""),
          triggerEvent: true,
          textStyle: {
            rich: {
              b: {
                color: "#fff",
                fontSize: "20"
              }
            }
          },
          itemGap: 10,
          margin: 10,
          left: '5%',
          top: 10
        },
        color: ["#C98531", "#0177a9"],
        grid: {
          top: "15%",
          left: "20%",
          right: "10%",
          bottom: '4%'
        },
        xAxis: {
          type: "value",
          nameTextStyle: {
            color: "#7397C4"
          },
          axisLabel: {
            show: false
          },
          axisTick: {
            show: false
          },
          axisLine: {
            show: false
          },
          nameGap: 25,
          boundaryGap: [0, 0.01],
          splitLine: {
            show: false
          }
        },
        yAxis: [
          {
            name: "活动：信贷MGM",
            type: "category",
            axisLabel: {
              //刻度标签文字的颜色
              show: true,
              color: "#ccc",
              fontSize: 16
            },
            axisLine: {
              show: true,
              color: "#fff",
            },
            axisTick: {
              show: false
            },
            interval: 2,
            data: data.yAxis
          },
        ],
        series: [
          {
            name: "折人民币余额",
            label: {
              show: true,
              color: "#ccc",
              position: "right",
              right: 0,
              fontSize: 22,
              /* 7月10日wk修改：修改p3页面右侧欺诈风险画像为% */
              formatter: data => {
                if(this.isShowPercent) {
                  return data.value + '%';
                }
                return data.value;
              }
              /* end */
            },

            roundCap: true,
            type: "bar",
            data: data.series.reverse(),
            barWidth: 12,
            barCategoryGap: "2",
            itemStyle: {
              normal: {
                barBorderRadius: [10, 10, 10, 10],
                color: new this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  {
                    offset: 0,
                    color: "#40BEFF"
                  },
                  {
                    offset: 1,
                    color: "#5BEEFF"
                  }
                ])
              }
            }
          }
        ]
      });
    }
  }
};
</script>
<style lang="less" scoped>
#echarts03, #echarts04, #echarts05 {
  width: 98%;
  height: 100%;
  color: #fff;
}

.line-body {
  display: flex;
  .line-data {
    padding: 17px;
    text-align: right;
    span {
      display: inline-block;
      color: #0298e2;
      font-size: 18px;
      font-weight: 600;
      margin-bottom: 30%;
    }
  }
}
</style>