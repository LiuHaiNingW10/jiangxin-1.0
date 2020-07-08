<template>
  <div class="container">
    <div :class="headValue ? 'brain-bg':'brain-bgg'">
      <div class="header">
        <span class="header-bg">智能服务</span>
        <div class="right-time">
          <weather-com />
        </div>
      </div>
      <div class="main-content" v-if="showMain">
        <div class="left">
          <card-num :tableDatas="tableDataTop"/>
          <table-auto
            :tableDatas="tableDataAs"
            :ids="idA"
            :columns="columnA"
            :heights="clientHeight"
            :needPoint="true"
            @func="getPoint"
          />
          <!-- <line-right :tableDatas="tableDataB" :ids="idB" :heights="clientHeight" /> -->
          <pop-custom :tableDatas="tableDataBs" :ids="idB" :heights="clientHeight" />
        </div>
        <div class="center">
          <big-head
            :tableDatas="tableDataCs"
            :ids="idC"
            :heights="clientHeight"
            @func="fromHead"
            :bigPoint="bigPoint"
          />
        </div>
        <div class="right">
          <card-num :tableDatas="tableDataRight"/>
          <table-autoB
            :tableDatas="tableDataC"
            :ids="idC"
            :columns="columnC"
            :heights="clientHeight"
            :needPoint="true"
            @func="getPoint"
          />
          <pop-custom :tableDatas="tableDataDs" :ids="idD" :heights="clientHeight" />
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
import tableAutoB from "../../components/p4/autoScroll-tableB.vue";
import lineToRight from "../../components/p4/line-toRight.vue";
import bigHeadVue from "../../components/p4/big-head.vue";
import popCustom from "../../components/p4/pop-custom.vue";
import WeatherCom from "../../components/weather.vue";
import cardNum from "../../components/p4/card-num.vue"
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
    title: "行为",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "status",
    key: "status",
    scopedSlots: { customRender: "status" },
    class: "table-td",
    title: "状态",
    align: "center",
    width: "20%"
  },
  {
    dataIndex: "problem",
    key: "problem",
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
      tableDataTop: [
        {
          name: '在线智能处置率',
          value: '89%'
        },
        {
          name: '客服智能处置率',
          value: '99%'
        },
      ],
      tableDataRight: [
        {
          name: '智能外呼数量',
          value: 'M0'
        },
        {
          name: '单通交互次数',
          value: '7'
        },
      ],
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
        title: "智能客服（当日数据）"
      },
      columnA: columnA,
      tableDataB: [],
      idB: {
        id: "echarts02",
        title: "用户转化",
        action: "活动：信贷MGM"
      },
      tableDataC: [],
      idC: {
        id: "echarts03",
        title: "智能催收（当日数据）"
      },
      columnC: columnC,
      idD: {
        id: "echarts04",
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
      clientHeight: document.body.clientHeight,
      bigPoints: {}
    };
  },
  created() {},
  computed: {
    tableDataAs() {
      return this.tableDataA;
    },
    tableDataBs() {
      return this.tableDataB;
    },
    tableDataCs() {
      return this.tableDataC;
    },
    tableDataDs() {
      return this.tableDataD;
    },
    bigPoint() {
      return this.bigPoints;
    }
  },
  mounted() {
    this.init();
    this.timer = setInterval(() => {
      this.init();
    }, 60000);
    this.tB();
    this.tD();
  },
  methods: {
    init() {
      this.getTableDataA(() => {
        this.getTableDataC(() => {
          this.$nextTick(() => {
            this.showMain = true;
          });
        });
      });
    },
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
        this.bigPoints = arr[0];
        this.tableDataA = arr;
        cb();
      });
    },
    tB() {
      this.getTableDataB();
      this.timerB = setInterval(() => {
        this.getTableDataB();
      }, 6000);
    },
    tD() {
      this.getTableDataD();
      this.timerD = setInterval(() => {
        this.getTableDataD();
      }, 6000);
    },
    getTableDataB() {
      this.axios({
        url: "/api/p4/smartOperationSummary",
        method: "get",
        type: "json"
      }).then(data => {
        let arr = data.data.data;
        arr.forEach(it => {
          it.want = it.action;
        });
        this.tableDataB = arr;
      });
    },
    getTableDataD() {
      this.axios({
        url: "/api/p4/smartCallSummary",
        method: "get",
        type: "json"
      }).then(data => {
        this.tableDataD = data.data.data;
      });
    },

    fromHead(obj) {
      this.headValue = obj.value;
    },
    getPoint(par) {
      this.bigPoints = par;
    }
  },
  beforeDestroy() {
    clearInterval(this.timer);
    clearInterval(this.timerB);
    clearInterval(this.timerD);
  },
  components: {
    "card-num": cardNum,
    "table-auto": tableAuto,
    "table-autoB": tableAutoB,
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
    height: 5%;
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
      width: 27.2%;
      vertical-align: top;
    }
    .center {
      width: 45.4%;
      position: relative;
      height: 93%;
    }
    .right {
      width: 27.2%;
      vertical-align: top;
    }
  }
}
.common-box {
  font-size: 12px;
  position: relative;
  overflow: hidden;
  height: 44%;
  background-size: 96% 98%;
  padding: 2.8%;
}
</style>
