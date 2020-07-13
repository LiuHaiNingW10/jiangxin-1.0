<template>
  <div class="indicator-div">
    <div :style="titleStyle" class="title-content">{{titleContent}}</div>
    <div :style="digitalStyle" class="digital-data">{{digital || 0}}</div>
  </div>
</template>

<script>
export default {
  name: "axis",
  props: ["titleContent", "titleStyle", "digital", "digitalStyle"],
  created() {},
  mounted() {},
  data() {
    return {};
  },
  methods: {},
  watch: {}
};
</script>

<style lang="less" scroped>
.indicator-div {
  width: 100%;
  .title-content {
    color: rgba(255, 255, 255, 1);
    font-family: Microsoft YaHei;
    font-weight: bold;
    font-size: 32px;
    line-height: normal;
    letter-spacing: 0px;
    // text-align: left;
  }
  .digital-data {
    color: rgba(255, 255, 255, 1);
    font-family: Microsoft YaHei;
    font-weight: bold;
    font-size: 80px;
    line-height: normal;
    letter-spacing: 0px;
    // text-align: left;
    // margin-top: 34px;
  }
}
</style>
