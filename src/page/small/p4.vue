<template>
  <div class="container">
    <div :class="headValue ? 'brain-bg':'brain-bgg'">
      <div class="header">
        <span class="header-bg">百信智能聚变引擎</span>
        <div class="right-time">
          <weather-com />
        </div>
      </div>
      <div class="main-content" v-if="showMain">
        <div class="left">
          <table-auto
            :tableDatas="tableDataAs"
            :ids="idA"
            :columns="columnA"
            :heights="clientHeight"
          />
          <line-right :tableDatas="tableDataB" :ids="idB" :heights="clientHeight" />
        </div>
        <div class="center">
          <big-head :tableDatas="tableDataCs" :ids="idC" :heights="clientHeight" @func="fromHead" />
        </div>
        <div class="right">
          <table-auto
            :tableDatas="tableDataC"
            :ids="idC"
            :columns="columnC"
            :heights="clientHeight"
          />
          <pop-custom :tableDatas="tableDataD" :ids="idD" :heights="clientHeight" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import Yin from "../../assets/images/yin.png";
import Yu from "../../assets/images/yu.png";
import tableAuto from "../../components/p4/autoScroll-table.vue";
import lineToRight from "../../components/p4/line-toRight.vue";
import bigHeadVue from "../../components/p4/big-head.vue";
import popCustom from "../../components/p4/pop-custom.vue";
import WeatherCom from "../../components/weather.vue";

const columnA = [
  {
    dataIndex: "mobile",
    key: "mobile",
    class: "table-td",
    title: "手机号",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "action",
    key: "action",
    class: "table-td",
    title: "状态",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "status",
    key: "status",
    scopedSlots: { customRender: "status" },
    class: "table-td",
    title: "问题定位",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "handle",
    key: "handle",
    class: "table-td",
    title: "智能处理",
    align: "center",
    width: "20%"
  }
];
const columnC = [
  {
    dataIndex: "mobile",
    key: "mobile",
    class: "table-td",
    title: "手机号",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "accent",
    key: "accent",
    class: "table-td",
    title: "口音识别",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "product",
    key: "product",
    class: "table-td",
    title: "产品",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "want",
    key: "want",
    class: "table-td",
    title: "客户意图",
    align: "center",
    width: "24%"
  }
];
export default {
  data() {
    return {
      headValue: true,
      showMain: false,
      // 时间
      currentTime: {},
      currentDate: undefined,
      weatherImg: [Yin, Yu, Yu, Yu, Yu, Yu, Yu],
      currentWeather: {},
      currentImg: "",
      // 天气
      localweather: [
        { high: "29", low: "22" },
        { high: "28", low: "23" },
        { high: "27", low: "23" },
        { high: "25", low: "23" },
        { high: "26", low: "22" },

        { high: "29", low: "22" },
        { high: "29", low: "22" }
      ],
      tableDataA: [],
      idA: {
        id: "echarts01",
        title: "智能运营"
      },
      columnA: columnA,
      tableDataB: [
        { categroy: "还款用户数", num: 21044 },
        { categroy: "用信成功用户数", num: 23618 },
        { categroy: "授信通过用户数", num: 24396 },
        { categroy: "授信申请用户数", num: 25903 },
        { categroy: "好会花朴充信息页面浏览", num: 26258 },
        { categroy: "好会花申请按钮点击", num: 26758 },
        { categroy: "好会花Banner点击", num: 29632 }
      ],
      idB: {
        id: "echarts02",
        title: "用户转化",
        action: "活动：信贷MGM"
      },
      tableDataC: [],
      idC: {
        id: "echarts03",
        title: "智能客服"
      },
      columnC: columnC,
      idD: {
        title: "客户意愿"
      },
      tableDataD: [
        { desire: "未拔通", num: 21044 },
        { desire: "承诺近期还款", num: 23618 },
        { desire: "未接听", num: 24396 },
        { desire: "延期还款", num: 25903 },
        { desire: "通话中断", num: 26258 },
        { desire: "承诺马上还款", num: 26758 }
      ],
      clientHeight: document.body.clientHeight
    };
  },
  created() {
    this.getTableDataA(() => {
      this.getTableDataC(() => {
        this.$nextTick(() => {
          this.showMain = true;
        });
      });
    });
  },
  computed: {
    tableDataAs() {
      return this.tableDataA;
    },
    tableDataCs() {
      return this.tableDataC;
    }
  },
  mounted() {
    this.init();
    this.getTime();
  },
  methods: {
    init() {},
    getTableDataA(cb) {
      this.axios({
        url: "/api/p4/smartCall",
        method: "get",
        type: "json"
      }).then(data => {
        let arr = data.data.data;
        this.tableDataC = arr;
        cb();
      });
    },
    getTableDataC(cb) {
      this.axios({
        url: "/api/p4/smartOperation",
        method: "get",
        type: "json"
      }).then(data => {
        let arr = data.data.data;
        arr.forEach((el, i) => {
          el.status = [
            {
              id: i,
              value: el.status
            }
          ];
        });
        this.tableDataA = arr;
        cb();
      });
    },
    // 获取右上角当前时间
    getTime() {
      if (timeInterval) clearInterval(timeInterval);
      var timeInterval = setInterval(this.nowTime, 1000);
    },
    nowTime() {
      var myDate = new Date();
      var y = myDate.getFullYear();
      var M = myDate.getMonth() + 1; //获取当前月份(0-11,0代表1月)
      var d = myDate.getDate(); //获取当前日(1-31)
      var h = myDate.getHours(); //获取当前小时数(0-23)
      var m = myDate.getMinutes(); //获取当前分钟数(0-59)
      var s = myDate.getSeconds(); //获取当前秒数(0-59)

      //检查是否小于10
      M = this.check(M);
      d = this.check(d);
      h = this.check(h);
      m = this.check(m);
      s = this.check(s);
      var dateStr = y + "/" + M + "/" + d;
      var timeStr = h + ":" + m + ":" + s;
      this.currentTime = Object.assign({}, { date: dateStr, time: timeStr });
      // 获取当前天气
      this.getCurrentWeather(myDate.getDate());
      // document.getElementById("nowtime").innerHTML = "当前时间：" + timestr;
    },
    getCurrentWeather(d) {
      if (this.currentDate === d) {
        return;
      }
      var index = 0;
      if (d > 6) {
        this.currentWeather = this.localweather[index];
        this.currentImg = this.weatherImg[index];
      } else {
        this.currentWeather = this.localweather[d];
        this.currentImg = this.weatherImg[d];
      }
    },
    check(i) {
      var num = i < 10 ? "0" + i : i;
      return num;
    },
    fromHead(obj) {
      this.headValue = obj.value;
    }
  },
  components: {
    "table-auto": tableAuto,
    "line-right": lineToRight,
    "big-head": bigHeadVue,
    "pop-custom": popCustom,
    "weather-com": WeatherCom
  }
};
</script>
<style lang="less" scoped>
.container {
  width: 4224px;
  height: 1536px;
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  color: #fff;
  background: url("../../assets/images/p4-background.png") no-repeat 100% 100%;
  background-size: 100%;
  .brain-bg {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    color: #fff;
    background: url("../../assets/images/danao.png") no-repeat center;
    background-size: 50%;
    background-position: 50% 94%;
    padding-top: 1%;
  }
  .brain-bgg {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    color: #fff;
    background-size: 70%;
    padding-top: 1%;
  }
  .time-span {
    margin-left: 5%;
  }
  .header {
    height: 7%;
    text-align: center;
    background: url("../../assets/images/header.png") no-repeat 100% 100%;
    background-size: 100% 100%;
    position: relative;
    .header-bg {
      display: inline-block;
      background-size: 40%;
      font-size: 40px;
      font-weight: bold;
      width: 30%;
      height: 100%;
    }
    .right-time {
      width: 13%;
      text-align: right;
      position: absolute;
      top: 1%;
      right: 1%;
    }
  }
  .main-content {
    padding: 0 30px;
    height: 93%;
    display: flex;
    div {
      display: inline-block;
      width: 100%;
    }
    .left {
      width: 21%;
      vertical-align: top;
    }
    .center {
      width: 58%;
      position: relative;
      height: 93%;
    }
    .right {
      width: 21%;
      vertical-align: top;
    }
  }
}
.common-box {
  font-size: 12px;
  position: relative;
  overflow: hidden;
  height: 48%;
  background-size: 96%;
  padding: 3.2%;
  
}
</style>
