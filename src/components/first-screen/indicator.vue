<template>
  <div class="num-group">
    <!-- <div class="echarts w100 axis"></div> -->
    <div class="single-num" v-for="(item, index) in indicatorData" :key="index" :id='index'>{{item}}</div>
  </div>
</template>

<script>
export default {
  name: "axis",
  props: ["chartData"],
  mounted() {
    this.getData(this.chartData);
  },
  data() {
    return {
      todos: [],
      echartState: {}, // 标识 图表是否已经下钻
      indicatorData: []
    };
  },
  methods: {
    getData(chartData) {
      this.indicatorData = [...['¥'].concat(chartData.toString().split(""))];
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
    &:first-child {
      margin-left: 0px;
    }
  }
}
</style>
