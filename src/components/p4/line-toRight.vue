<template>
  <div class="lineToRight">
    <div class="line-title">
      <span>{{id.title}}</span>
    </div>
    <div class="line-action">
      <span>{{id.action}}</span>
    </div>
    <div class="line-body">
      <div :id="id.id"></div>
      <div class="line-data">
        <span v-for="(item) in lineDatas" :key="item">{{item}}</span>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
export default {
  name: "tableAuto",
  props: ["tableDatas", "ids", "columns", "heights"],
  data() {
    return {
      tableData: this.tableDatas,
      id: this.ids,
      height: this.heights,
      lineData: []
    };
  },
  computed: {
    lineDatas() {
      return this.lineData.reverse()
    }
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.transLateData();
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
    drawLineG(ele, data) {
      var _this = this;
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById(ele));
      // 绘制图表
      var a = data.chartName || "信贷客户数——表内";
      var b = "2018年12月31日";
      myChart.setOption({
        color: ["#C98531", "#0177a9"],
        grid: {
          top: "0",
          left: "44%",
          right: "10%"
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
        yAxis: {
          type: "category",
          axisLabel: {
            //刻度标签文字的颜色
            show: true,
            color: "#7397C4"
          },
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          },
          nameGap: 30,
          data: data.yAxis
        },
        series: [
          {
            name: "折人民币余额",
            label: {
              show: false,
              color: "#4DD5FF",
              position: "right",
              right: 0
            },
            roundCap: true,
            type: "bar",
            data: data.series,
            barWidth: 6,
            barCategoryGap: "0",
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
<style lang="less">
#echarts02 {
  width: 85%;
  height: 260px;
  color: #fff;
}
.lineToRight {
  font-size: 12px;
  position: relative;
  overflow: hidden;
  width: 100%;
  background: url("../../assets/images/p4-table-title.png") no-repeat center;
  background-size: 100% 100%;
  padding: 14px 10px;
  .line-title {
    text-align: left;
    padding-left: 24px;

  }
  .line-action {
    color: #ccc;
    margin: 16px 0;
  }
}
.line-body {
  display: flex;
  .line-data {
    span {
      display: inline-block;
      color: #0298e2;
      font-size: 18ox;
      font-weight: 600;
      margin: 6.4px 0 4px 4px;
    }
  }
}
</style>