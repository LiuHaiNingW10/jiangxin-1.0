<template>
  <div class="common-box popToRight">
    <div class="p4-table-title">
      <span>{{id.title}}</span>
    </div>
    <div class="pop-content">
      <div id="p4-pop"></div>
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
  computed: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.getData()
      this.timer = setInterval( () => {
        this.getData()
      },60000)
    },
    getData() {
      this.axios({
        url: "/api/p4/smartCallSummary",
        method: "get",
        type: "json"
      }).then(data => {
        this.tableData = data.data.data;
        this.drawChart();
      });
    },
    drawChart() {
      let myChart = this.$echarts.init(document.getElementById("p4-pop"));
      let data = this.tableData[0] && this.tableData || [
        {num: '未拨通', want: '10'},
        {num: '承诺近期还款', want: '10'},
      ];
      // 绘制图表
      myChart.setOption({
        title: {
          show: true, //显示策略，默认值true,可选为：true（显示） | false（隐藏）
          x: "center", // 水平安放位置，默认为左对齐，可选为：
          // 'center' ¦ 'left' ¦ 'right'
          // ¦ {number}（x坐标，单位px）
          y: "bottom", // 垂直安放位置，默认为全图顶端，可选为：
          // 'top' ¦ 'bottom' ¦ 'center'
          // ¦ {number}（y坐标，单位px）
          //textAlign: null          // 水平对齐方式，默认根据x设置自动调整
          backgroundColor: "rgba(0,0,0,0)",
          borderColor: "#ccc", // 标题边框颜色
          borderWidth: 0, // 标题边框线宽，单位px，默认为0（无边框）
          padding: 5, // 标题内边距，单位px，默认各方向内边距为5，
          // 接受数组分别设定上右下左边距，同css
          itemGap: 10, // 主副标题纵向间隔，单位px，默认为10，
          textStyle: {
            fontSize: 18,
            fontWeight: "bolder",
            color: "#eee" // 主标题文字颜色
          },
          subtextStyle: {
            color: "#aaa" // 副标题文字颜色
          }
        },
        tooltip: {
          show: false
        },
        animationDurationUpdate: function(idx) {
          // 越往后的数据延迟越大
          return idx * 100;
        },
        animationEasingUpdate: "bounceIn",
        color: ["#fff", "#fff", "#fff"],
        series: [
          {
            type: "graph",
            layout: "force",
            force: {
              repulsion: 400
            },
            roam: true,
            label: {
              show: true,
              color: "#fff",
              fontWeight: "bold",
              fontSize: 18,
              formatter: "{b}\n\n{value|{c}}",
              rich: {
                value: {
                  fontSize: "24",
                  align: "center"
                }
              }
            },
            data: [
              {
                name: data[0].want,
                value: data[0].num + "人",
                symbolSize: 180,
                symbol: `image://${require("@/assets/images/p2/loan1.svg")}`,
                draggable: true,
                label: {
                  align: "center"
                }
              },
              {
                name: data[1].want,
                value: data[1].num+ "人",
                symbolSize: 281,
                symbol: `image://${require("@/assets/images/p2/loan3.svg")}`,
                draggable: true,
                label: {
                  align: "center"
                }
              },
              {
                name: data[2].want,
                value: data[2].num + "人",
                symbolSize: 160,
                symbol: `image://${require("@/assets/images/p2/loan3.svg")}`,
                draggable: true,
                label: {
                  align: "center"
                }
              },
              {
                name: data[3].want,
                value: data[3].num+ "人",
                symbolSize: 150,
                symbol: `image://${require("@/assets/images/p2/loan4.svg")}`,
                draggable: true,
                label: {
                  align: "center"
                }
              },
              {
                name: data[4].want,
                value: data[4].num+ "人",
                symbolSize: 170,
                symbol: `image://${require("@/assets/images/p2/loan5.svg")}`,
                draggable: true,
                label: {
                  align: "center"
                }
              },
              {
                name: data[5].want,
                value: data[5].num+ "人",
                symbolSize: 170,
                symbol: `image://${require("@/assets/images/p2/loan5.svg")}`,
                draggable: true,
                label: {
                  align: "center"
                }
              }
            ]
          }
        ]
      });
    },
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
};
</script>
<style lang="less" scoped>
.popToRight {
  background: url("../../assets/images/p4-table-title.png") no-repeat center;
  .pop-content {
    text-align: center;
    padding: 30px 0;
    height: 90%;
    #p4-pop {
      height: 100%;
    }
  }
}
</style>