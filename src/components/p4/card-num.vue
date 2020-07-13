<template>
  <div id="num-and-coin">
    <div class="top-left top-single">
      <div class="portrait">
        <img class="portrait-img" src="../../assets/images/icon2.png" alt />
      </div>
      <div class="server-num num-span">
        <span>{{tableData[0].name}}</span>
        <span>{{tableData[0].value}}</span>
      </div>
    </div>
    <div class="top-right top-single">
      <div class="portrait">
        <img class="portrait-img" src="../../assets/images/icon2.png" alt />
      </div>
      <div class="shouxin-num num-span">
        <span>{{tableData[1].name}}</span>
        <span>{{tableData[1].value}}</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "card-num",
  props: ["tableDatas"],
  data() {
    return {
      tableData: this.tableDatas
    };
  },
  mounted() {}
};
</script>
<style lang="less" scoped>
#num-and-coin {
  width: 96%;
  margin: 0 auto;
  background-size: 100% 100%;
  justify-content: space-around;
  align-items: center;
  padding: 2.8%;
  .top-single {
    display: flex;
    align-items: center;
    height: 100%;
    padding-bottom: 3%;
    .portrait {
      width: 14%;
      background: url("../../assets/images/portrait-left.svg") no-repeat;
      background-size: 100% 100%;
      text-align: center;
      .portrait-img {
        padding: 16%;
      }
    }
    .num-span {
      width: 88%;
      height: 88%;
      display: flex;
      align-items: center;
      :nth-child(1) {
        font-size: 32px;
        font-weight: bold;
      }
      :nth-child(2) {
        display: inline-block;
        margin-left: 10%;
        font-size: 48px;
        color: #04bbff;
        font-weight: bold;
      }
    }
    .middle-rec {
      background: url("../../assets/images/num-cut-off.png") no-repeat;
      background-size: 100% 100%;
      width: 100%;
      height: 10%;
    }
    .num-span-data {
      font-size: 36px;
      height: 100%;
      color: #04bbff;
      display: inline-block;
      overflow: hidden;

      .total-money-span {
        height: 40%;
      }
    }
  }
}
</style>