<template>
  <div class="common-box popToRight">
    <div class="p4-table-title">
      <span>{{id.title}}</span>
    </div>
    <div class="pop-content">
      <div class="pop-item pop-a">
        <span>未拨通</span>
        <br />
        <i>
          67
          <br />人
        </i>
      </div>
      <div class="pop-item pop-b">
        <span>最近承诺还款</span>
        <br />
        <i>
          2637
          <br />人
        </i>
      </div>
      <div class="pop-item pop-c">
        <span>未接听</span>
        <br />
        <i>
          12
          <br />人
        </i>
      </div>
      <div class="pop-item pop-d">
        <span>延迟还款</span>
        <br />
        <i>
          67
          <br />人
        </i>
      </div>
      <div class="pop-item pop-e">
        <span>通话中断</span>
        <br />
        <i style="color:#09C937">
          37
          <br />人
        </i>
      </div>
      <div class="pop-item pop-f">
        <span>承诺马上还款</span>
        <br />
        <i>
          1237
          <br />人
        </i>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
export default {
  name: "tableAuto",
  props: ["tableDatas", "ids", "columns", "heights"],
  data() {
    return {
      tableData: this.tableDatas,
      id: this.ids,
      height: this.heights,
      lineData: []
    };
  },
  computed: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {
      // this.transLateData();
    },
    transLateData() {
      let obj = {
        type: "bar",
        yAxis: [],
        series: []
      };
      this.tableData.forEach(el => {
        obj.series.push(el.num);
        obj.yAxis.push(el.categroy);
      });
      this.lineData = obj.series;
      this.drawLineG(this.id.id, obj);
    }
  }
};
</script>
<style lang="less" scoped>
.popToRight {
  background: url("../../assets/images/p4-table-title.png") no-repeat center;
  .pop-content {
    text-align: center;
    padding: 30px 0;
    .pop-item {
      text-align: center;
      display: inline-block;
      i {
        font-style: normal;
        font-size: 14px;
      }
    }
    .pop-a {
      width: 21%;
      background: url("../../assets/images/circle/1-lv.png") no-repeat center;
      background-size: 116%;
      padding: 4%;
      i {
        color: #e19513;
      }
    }
    .pop-b {
      width: 30%;
      background: url("../../assets/images/circle/oval.png") no-repeat center;
      background-size: 90%;
      padding: 7%;
      i {
        color: #17d9f2;
      }
    }
    .pop-c {
      width: 30%;
      background: url("../../assets/images/circle/1-lv.png") no-repeat center;
      background-size: 100%;
      padding: 8%;
      i {
        color: #e19513;
      }
    }
    .pop-d {
      width: 34%;
      background: url("../../assets/images/circle/oval.png") no-repeat center;
      background-size: 64%;
      padding: 8%;
      i {
        color: #abd043;
      }
    }
    .pop-e {
      width: 30%;
      background: url("../../assets/images/circle/1-lv.png") no-repeat center;
      background-size: 100%;
      padding: 8%;
      i {
        color: #17d9f2;
      }
    }
    .pop-f {
      width: 25%;
      background: url("../../assets/images/circle/oval.png") no-repeat center;
      background-size: 95%;
      padding: 4%;
      i {
        color: #17d9f2;
      }
    }
  }
}
</style>