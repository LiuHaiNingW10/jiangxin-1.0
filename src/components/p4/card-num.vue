<template>
  <div id="num-and-coin">
    <div class="top-left top-single">
      <div class="portrait">
        <img class="portrait-img" src="../../assets/images/icon2.png" alt />
      </div>
      <div class="server-num num-span">
        <div class="num-span-title">{{tableData[0].name}}</div>
        <div class="middle-rec"></div>
        <div class="num-span-data">{{tableData[0].value}}</div>
      </div>
    </div>
    <div class="top-middle"></div>
    <div class="top-right top-single">
      <div class="portrait">
        <img class="portrait-img" src="../../assets/images/icon1.png" alt />
      </div>
      <div class="shouxin-num num-span">
        <div class="num-span-title">{{tableData[1].name}}</div>
        <div class="middle-rec"></div>
        <div class="num-span-data">{{tableData[1].value}}</div>
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
  mounted() {
    console.log(this.tableData);
  }
};
</script>
<style lang="less" scoped>
#num-and-coin {
  width: 96%;
  height: 8%;
  margin: 0 auto;
  background: url("../../assets/images/group-7.png") no-repeat;
  background-size: 100% 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  .top-single {
    display: flex;
    align-items: center;
    width: 49%;
    height: 100%;
    padding: 0.5% 6%;
    .portrait {
      height: 70%;
      width: 22%;
      background: url("../../assets/images/portrait-left.svg") no-repeat;
      background-size: 100% 100%;
      text-align: center;
      .portrait-img {
        padding-top: 16%;
      }
    }
    .num-span {
      width: 88%;
      height: 88%;
      .num-span-title {
        font-size: 21px;
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
  .top-middle {
    width: 0.3%;
    height: 66%;
    background: url("../../assets/images/rectangle.png") no-repeat;
    background-size: 100% 100%;
  }
}
</style>