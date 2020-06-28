<template>
<div class="container">
    <div class="header">百信智能聚变引擎</div>
    <div class="main-content">
      <div class="left">
          <table-auto :tableDatas='tableDataA' :ids='idA' :columns="columnA" :heights="clientHeight" />
          <line-right :tableDatas='tableDataB' :ids='idB' :heights="clientHeight" />
      </div>
      <div class="center">
        <big-head :tableDatas='tableDataC' :ids='idC' :heights="clientHeight" />
      </div>
      <div class="right">
          <table-auto :tableDatas='tableDataC' :ids='idC' :columns="columnC" :heights="clientHeight" />
          <div style="width: 100%;height:318px">

          </div>
      </div>
    </div>
</div>
</template>

<script>
import Vue from 'vue'
import tableAuto from '../../components/p4/autoScroll-table.vue'
import lineToRight from '../../components/p4/line-toRight.vue'
import bigHeadVue from '../../components/p4/big-head.vue'

const columnA = [
  {
    dataIndex: "telphoneNum",
    key: "telphoneNum",
    class: "table-td",
    title:'手机号',
    align: "center",
    width: '20%'
  },
  {
    dataIndex: "action",
    key: "action",
    class: "table-td",
    title:'状态',
    align: "center",
    width: '20%'
  },
  {
    dataIndex: "status",
    key: 'status',
    scopedSlots: { customRender: 'status' },
    class: "table-td",
    title:'问题定位',
    align: "center",
    width: '20%',
  },
  {
    dataIndex: "response",
    key: "response",
    class: "table-td",
    title:'智能处理',
    align: "center",
    width: '20%'
  }
];
const columnC = [
  {
    dataIndex: "telphoneNum",
    key: "telphoneNum",
    class: "table-td",
    title:'手机号',
    align: "center",
    width: '20%'
  },
  {
    dataIndex: "area",
    key: "area",
    class: "table-td",
    title:'口音识别',
    align: "center",
    width: '20%'
  },
  {
    dataIndex: "status",
    key: 'status',
    class: "table-td",
    title:'产品',
    align: "center",
    width: '20%',
  },
  {
    dataIndex: "response",
    key: "response",
    class: "table-td",
    title:'客户意图',
    align: "center",
    width: '24%'
  }
]
export default {
  data() {
    return {
      tableDataA:[
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status:['-1'],
          response:'四要素鉴权'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status:['0'],
          response:'短信召回'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['0'],
          response:'短信召回'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status: ['1'],
          response:''
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status:['-1'],
          response:'四要素鉴权'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          action:'打开首页',
          status:['0'],
          response:'短信召回'
        },
      ],
      idA:{
        id:'echarts01',
        title: '智能运营'
      },
      columnA: columnA,
      tableDataB:[
        {categroy:'还款用户数',num: 21044},
        {categroy:'用信成功用户数',num: 23618},
        {categroy:'授信通过用户数',num: 24396},
        {categroy:'授信申请用户数',num: 25903},
        {categroy:'好会花朴充信息页面浏览',num: 26258},
        {categroy:'好会花申请按钮点击',num: 26758},
        {categroy:'好会花Banner点击',num: 29632 },
      ],
      idB:{
        id:'echarts02',
        title: '用户转化',
        action: '活动：信贷MGM'
      },
      tableDataC:[
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'北京',
          status: ['好会花'],
          response:'承诺马上还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'安徽',
          status:['好会花'],
          response:'承诺近期还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'合肥',
          status:['好会花'],
          response:'未援酒'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'上海',
          status: ['好会花'],
          response:'延期还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'南京',
          status: ['好会花'],
          response:'未接听'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'苏州',
          status: ['好会花'],
          response:'承诺马上还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'西安',
          status: ['好会花'],
          response:'承诺马上还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'南通',
          status: ['好会花'],
          response:'未接听'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'北京',
          status: ['好会花'],
          response:'延期还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'南阳',
          status: ['好会花'],
          response:'承诺马上还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'吉安',
          status: ['好会花'],
          response:'延期还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'新疆',
          status: ['好会花'],
          response:'未接听'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'洛阳',
          status: ['好会花'],
          response:'延期还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'吉安',
          status:['好会花'],
          response:'承诺马上还款'
        },
        {
          telphoneNum: 188 + '****'+ (Math.random() * 9000 + 1000).toFixed(0),
          area:'吉安',
          status:['好会花'],
          response:'承诺马上还款'
        },
      ],
      idC:{
        id:'echarts03',
        title: '智能客服'
      },
      columnC: columnC,
      clientHeight: document.body.clientHeight
    };
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
    },
  },
  components: {
    "table-auto": tableAuto,
    "line-right": lineToRight,
    'big-head': bigHeadVue
  }
};
</script>
<style lang="less" scoped>
  .container {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    color: #fff;
    background: url('../../assets/images/background.png') 100% 100%;
    .header {
        height: 8%;
        text-align: center;
        line-height: 40px;
        font-size: 18px;
        font-weight: 600;
        font-family: sans-serif;
        letter-spacing: 2px;
        background: linear-gradient(to right, #fff, #ACC5FD);
        -webkit-background-clip: text;
        color: transparent;
    }
    .main-content {
      padding: 0 60px;
      div {
        display: inline-block;
      }
      .left {
        width: 22%;
      }
      .center {
        width: 56%;
      }
      .right {
        width: 22%;
      }
    }
  }
</style>
