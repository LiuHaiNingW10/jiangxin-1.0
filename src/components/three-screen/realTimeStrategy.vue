<template>
  <div class="real-time-strategy" id='realTimeStrategy'>
    <div class="title">{{title}}</div>
    <div class="num">
      <ul>
        <li v-for="(item, index) in numArr" :key="index">{{item}}</li>
      </ul>
    </div>
    <div class="unit">万次</div>
  </div>
</template>

<script>
export default {
  props: ['title', 'num'],
  data() {
    return {
    };
  },
  computed: {
    numArr: function() {
      return this.num ? String(this.num).split('') : []
    }
  },
  mounted () {
  },
  methods: {
  },
  components: {}
};
</script>

<style lang="less" scoped>
.real-time-strategy{
  margin: 40px;
  display: inline-block;
  .title{
    text-align: left;
  }
  ul{
    display: flex;
    list-style: none;
    li{
      padding: 10px;
      margin: 0 2px;
      background: url("../../assets/images/num-bg.png") no-repeat;
      background-size: 100% 100%;
    }

  }
  .num, .unit{
    display: inline-block;
  }
  .unit{
    font-size: 20px;
    position: relative;
    top: 10px;
    margin: 20px 0 0 10px
  }
}
</style>
