<template>
  <div class="enterprise-loan">
    <div class="cloud-content enterprise-div">
      <div class="content-title">
        <span class="first-text-span">小微企业贷款</span>
        <loan-chart />
      </div>
    </div>
    <div class="enterprise-atlas enterprise-div enterprise-content">
      <div class="second-title">
        <span class="text-span">企业图谱</span>
      </div>
      <div id="graph"></div>
    </div>
    <div class="enterprise-credit enterprise-div enterprise-content">
      <div class="second-title">
        <span class="text-span">企业信用</span>
        <credit-chart />
      </div>
    </div>
  </div>
</template>

<script>
import LoanChart from "@/components/second-screen/loanChart";
import CreditChart from "@/components/second-screen/creditChart";
import * as basebox from "@/assets/base64.js";
export default {
  name: "",
  props: [],
  data() {
    return {};
  },
  computed: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.getGraphData();
    },
    getGraphData() {
      this.axios({
        url: "/api/p2/cmpGraph",
        method: "get",
        params: {
          company: "海宁大红马新材料股份有限公司"
        },
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var graphData = data.data.data.graph;
          var root = data.data.data.company;
          this.drawGraph(root, graphData);
        }
      });
    },
    getY(x) {
      let y = Math.sqrt((1 - Math.pow(x / 38, 2)) * Math.pow(30, 2));
      return y;
    },
    getOutY(x) {
      let y = Math.sqrt((1 - Math.pow(x / 50, 2)) * Math.pow(42, 2));
      return y;
    },
    translate(root, list) {
      let arr = [],
        length = list.vertices.length;
      list.vertices.forEach((it, i) => {
        arr.push({
          name: it.name,
          itemStyle: {
            normal: {
              color: it.type === "person" ? "#395C97" : "#6C3A61"
            }
          },
          x: it.root ? 50 : this.getRondan('x'),
          y: it.root ? 100 : this.getRondan('y')
        });
      });
      console.log(arr);
      return arr;
    },
    getRondan(type) {
      let n = 0
      var num =  () => {
        n  = Math.round(-200 * Math.random()) + 200;
      }
      if(type === 'x' && Math.abs(n-50) < 100) {
        num()
      }else if (type === 'y' && Math.abs(n-100) < 400) {
        num()
      }
      return n;
    },
    dataLink(list) {
      let arr = [];console.log(list)
      list.edges.forEach((it, i) => {
        arr.push({
          source: it.from,
          value: it.type,
          target: it.to,
          itemStyle: {
            normal: {
              color: '#fff'
            }
          }
        });
      });
      console.log(arr);
      return arr;
    },
    drawGraph(root, graphData) {
      let a = this.translate(root, graphData);
      let b = this.dataLink(graphData);
      let myChart = this.$echarts.init(document.getElementById("graph"));
      console.log(a,b);
      let option = {
        xAxis: {
          show: false,
          type: "value"
        },
        yAxis: {
          show: false,
          type: "value"
        },
        tooltip: {
          formatter: "{b}"
        },
        series: [
          {
            type: "graph",
            layout: "none",
            edgeSymbol: ["circle", "arrow"],
            edgeSymbolSize: [0, 20],
            draggable: true,
            edgeLabel: {
              normal: {
                show: true,
                position: "middle",
                textStyle: {
                  fontSize: 14
                },
                formatter: "{c}"
              }
            },
            toam: true,
            zoom: 0.9,
            focusNodeAdjacency: true, // 指定的节点以及其所有邻接节点高亮
            roam: false, // 是否可拖拽

            lineStyle: {
              normal: {
                width: 2,
                shadowColor: "none",
                color: "#fff",
                curveness: 0.1
              }
            },
            symbolSize: [30, 30],
            symbol: "circle",
            label: {
              show: true,
              position: "bottom",
              color: "#ccc",
              formatter: function(params) {
                let c = params.data.username || "";
                let b = params.data.name;
                var str = "";
                if (c === "") {
                  str = "{p|" + b + "}";
                } else {
                  str = "{n|" + c + "}\n{p|" + b + "}";
                }
                return str;
              },
              rich: {
                p: {
                  fontSize: 14,
                  color: "#ccc",
                  align: "center"
                },
                n: {
                  height: 25,
                  fontSize: 16,
                  color: "#ccc",
                  align: "center"
                }
              }
            },
            itemStyle: {
              normal: {
                color: "#F2F2F2",
                borderColor: "#707070",
                borderWidth: 2
              }
            },
            data: a,
            links: b 
          }
        ]
      };
      myChart.setOption(option);
    }
  },
  components: {
    LoanChart,
    CreditChart
  }
};
</script>

<style lang="less">
.enterprise-loan {
  .enterprise-div {
    width: 100%;
    height: 32%;
    margin-bottom: 1%;
    padding: 1.7% 1.97%;
    .second-title {
      width: 100%;
      height: 9.22%;
      font-size: 24px;
      background: url("../../assets/images/title4备份 4.png") no-repeat;
      background-size: 100% 100%;
    }
  }
  .cloud-content {
    .content-title {
      width: 100%;
      height: 12.96%;
      background: url("../../assets/images/bg-content-title.png") no-repeat;
      background-size: 100% 100%;
      padding-left: 4.64%;
      margin-bottom: 2%;
      line-height: 220%;
    }
  }
  .enterprise-content {
    padding: 0 4%;
  }
  .text-span {
    display: inline-block;
    margin-left: 4%;
  }
  #graph {
    height: 80%;
  }
}
</style>
