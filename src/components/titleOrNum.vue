<template>
  <div class="title-num">
    <div class="title">{{title}}</div>
    <div class="number">{{num}}</div>
  </div>
</template>

<script>
export default {
  name: "",
  props: ['title','num'],
  mounted() {
  },
  data() {
    return {
      
    };
  },
  computed: {},
  methods: {

  },
    
  beforeDestroy() {
  },
};
</script>

<style lang="less" scoped>
  .title-num {
    padding: 40px 0 20px 40px;
    .title {
      color: #FFFFFF;
      font-family: Microsoft YaHei;
      font-weight: bold;
      font-size: 32px;
      line-height: normal;
      letter-spacing: 0px;
      text-align: left;
    }
    .number {
      color: #FFFFFF;
      font-family: Microsoft YaHei;
      font-weight: bold;
      font-size: 80px;
      line-height: normal;
      letter-spacing: 0px;
      text-align: left;
    }
  }
</style>
