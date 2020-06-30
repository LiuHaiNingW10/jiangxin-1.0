<template>
  <div class="num-group" :style="styleData">
    <!-- <div class="echarts w100 axis"></div> -->
    <div
      class="single-num"
      v-for="(item, index) in indicatorData"
      :key="index"
      :id="index"
      :style="styleSingle"
    >{{item}}</div>
  </div>
</template>

<script>
export default {
  name: "axis",
  props: ["chartData", "styleData", "styleSingle", "type"],
  mounted() {
    this.getData(this.chartData, this.type);
  },
  data() {
    return {
      todos: [],
      echartState: {}, // 标识 图表是否已经下钻
      indicatorData: []
    };
  },
  methods: {
    getData(chartData, type) {
      if (chartData !== undefined) {
        if (type === "person") {
          this.indicatorData = [...chartData.toString().split("")];
        } else {
          this.indicatorData = [
            ...["¥"].concat(chartData.toString().split(""))
          ];
        }
      }
    }
  }
};
</script>

<style lang="less">
.num-group {
  min-width: 50%;
  height: 2%;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  margin-top: 2%;
  .single-num {
    margin-left: 3%;
    width: 2%;
    height: 100%;
    background: url("../../assets/images/text-no-white.png") no-repeat;
    // background: url("../../assets/images/text-bg.png") no-repeat;
    background-size: 100% 100%;
    text-align: center;
    &:first-child {
      margin-left: 0px;
    }
  }
}
</style>
