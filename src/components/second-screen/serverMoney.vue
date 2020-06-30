<template>
  <div class="server-money">
    <div class="total-money">
      <span>累计服务金额</span>
      <!-- <indicator-chart :chartData="totalMoney" :styleData="styleObj" :styleSingle="singleStyle" /> -->
      <indicator-chart
        v-if="totalMoney"
        :chartData="totalMoney"
        :styleData="styleObj"
        :styleSingle="singleStyle"
        chartId="total-server-money"
      />
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
  mounted() {
    this.rollTimelyData();
  },
  data() {
    return {
      totalMoney: "",
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
  methods: {
    // 格式化千分位
    thousandFormat(value, fixed) {
      fixed = fixed !== undefined ? fixed : 2;
      if (value === null || value === undefined || isNaN(parseFloat(value))) {
        return;
      }
      // 将数字进行千分位格式化
      function toThousands(num) {
        num = (num || 0).toString();
        var parts = num.split(".");
        var bigZeroPart = parts[0];
        var result = "";
        while (bigZeroPart.length > 3) {
          result = "," + bigZeroPart.slice(-3) + result;
          bigZeroPart = bigZeroPart.slice(0, bigZeroPart.length - 3);
        }
        if (bigZeroPart) {
          result = bigZeroPart + result;
        }
        if (parts.length > 1) {
          result += "." + parts[1].toString();
        }
        return result;
      }

      value = parseFloat(value).toFixed(fixed);
      value = toThousands(value);
      return value;
    },
    rollTimelyData() {
      this.getTotalMoney();
      var _that = this;
      var index = 0;
      if (coinInterval) clearInterval(coinInterval);
      var coinInterval = setInterval(
        () => {
          this.getTotalMoney();
        },
        30000,
        _that,
        index
      );
    },

    getTotalMoney() {
      let that = this;
      this.axios({
        url: "/api/p2/accServiceAmount",
        method: "get",
        data: "",
        type: "json"
      }).then(data => {
        if (data.data.code === 100) {
          var tData = data.data.data;
          that.totalMoney = that.thousandFormat(tData, 2);
        }
      });
    }
  },
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
