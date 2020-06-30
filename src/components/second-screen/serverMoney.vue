<template>
  <div class="server-money">
    <div class="total-money">
      <span>累计服务金额</span>
      <indicator-chart :chartData="totalMoney" :styleData="styleObj" :styleSingle="singleStyle" />
    </div>
    <div class="current-money">
      <span>当日小微信贷服务金额</span>
      <span class="current-money-span">¥ {{currentMoney}}</span>
    </div>
  </div>
</template>

<script>
import IndicatorChart from "../../components/first-screen/indicator.vue";
export default {
  name: "",
  props: [],
  mounted() {},
  data() {
    return {
      totalMoney: "278,978,889,860.08",
      currentMoney: "19,289,386,378.97",
      styleObj: {
        height: "45%",
        fontSize: "64px"
      },
      singleStyle: {
        width: "5%",
        marginLeft: "1%"
      }
    };
  },
  computed: {},
  methods: {},
  components: {
    "indicator-chart": IndicatorChart
  }
};
</script>

<style lang="less">
.server-money {
  padding: 2.74%;
}
.total-money {
  height: 60%;
  font-weight: bold;
}
.current-money {
  height: 40%;
  width: 100%;
  .current-money-span {
    display: block;
    font-size: 64px;
    letter-spacing: 16px;
    font-weight: bold;
  }
}
</style>
