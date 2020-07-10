<template>
  <div class="verification">
    <ul>
      <li v-for="item in tableData" :key="item.title">
        <div>{{item.title}}</div>
        <img :src="item.img" alt="">
        <div><i>{{item.times}}</i>次</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'verification',
  props: ['tableDatas',],
  data() {
    return {
      id: this.ids,
      tableData: this.tableDatas
    }
  },
  mounted() {
    // console.log(this.tableData)
  },
  methods: {
  },
  beforeDestroy() {
    clearInterval(this.timer);
    this.timer = null;
  },
  components: {}
};
</script>

<style lang="less" scoped>
.verification {
  height: 16%;
  ul {
    width: 80%;
    margin: 0 auto;
    display: flex;
    color: #ccc;
    justify-content: space-around;
    li {
      list-style: normal;
      display: inline-block;
      width: 24%;
      text-align: center;
      padding-top: 20px;
      i {
        font-style: normal;
        color: #587D5D;
      }
      img {
        width: 122px;
      }
    }
  }
}
</style>
