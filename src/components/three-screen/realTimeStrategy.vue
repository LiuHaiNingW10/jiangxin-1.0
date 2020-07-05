<template>
  <div class="real-time-strategy" id="realTimeStrategy">
    <div class="title">{{title}}（万次）</div>
    <div class="num">
      <ul>
        <li v-for="(item, index) in numArr" :key="index">{{item||''}}</li>
        <!-- <indicator-chart
          :chartData="this.formatMoney(this.num)"
          :styleData="styleObj"
          marginTop="-120px"
          :styleSingle="singleStyle"
          :chartId="'cheat-money' + this.blank"
        /> -->
      </ul>
    </div>
  </div>
</template>

<script>
import IndicatorChart from "../first-screen/indicator.vue";
export default {
  props: ["title", "num", "blank"],
  data() {
    return {
      styleObj: {
        height: "45%",
        fontSize: "64px"
      },
      singleStyle: {
        width: "5%",
        marginLeft: "1%",
        lineHeight: "150%"
      }
    };
  },
  computed: {
    numArr: function() {
      if (this.num) {
        var arr = String(this.formatMoney(this.num)).split("");
        for (var i = 0; i < this.blank; i++) {
          arr.unshift("");
        }
        return arr;
      }
      return [];
    }
  },
  methods: {
    formatMoney(num, bool = false) {
      if (!num && num !== 0) return 0;
      if (num == 0) return 0;
      num = parseFloat(num);
      // 如果是金额统一保留两位小数，bool为false，不是金额，整数不保留两位
      if (bool) {
        return num
          ? (num.toFixed(2) + "").replace(
              /\d{1,3}(?=(\d{3})+(\.\d*)?$)/g,
              "$&,"
            )
          : "0";
      } else {
        const reg = /.*\..*/;
        if (reg.test(num)) {
          return num
            ? (num.toFixed(2) + "").replace(
                /\d{1,3}(?=(\d{3})+(\.\d*)?$)/g,
                "$&,"
              )
            : "0";
        } else {
          return num
            ? (num + "").replace(/\d{1,3}(?=(\d{3})+(\.\d*)?$)/g, "$&,")
            : "0";
        }
      }
    }
  },
  components: {
    "indicator-chart": IndicatorChart
  }
};
</script>

<style lang="less" scoped>
.real-time-strategy {
  margin: 30px;
  display: inline-block;
  .title {
    text-align: left;
    font-size: 32px;
    margin-bottom: 22px;
  }
  ul {
    display: flex;
    list-style: none;
    li {
      width: 72px;
      height: 140px;
      line-height: 140px;
      text-align: center;
      font-size: 64px;
      margin-right: 10px;
      background: url("../../assets/images/num-bg.png") no-repeat;
      background-size: 100% 100%;
    }
  }
}
</style>
