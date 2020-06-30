<template>
  <div class="popToRight">
    <div class="pop-title">
      <span>{{id.title}}</span>
    </div>
    <div class="pop-content">
      <div class="pop-item pop-a">
          <span>未拨通</span><br>
          <i>67<br>人</i>
      </div>
      <div class="pop-item pop-b">
          <span>最近承诺还款</span><br>
          <i>2637<br>人</i>
      </div>
      <div class="pop-item pop-c">
          <span>未接听</span><br>
          <i>12<br>人</i>
      </div>
      <div class="pop-item pop-d">
          <span>延迟还款</span><br>
          <i>67<br>人</i>
      </div>
      <div class="pop-item pop-e">
          <span>通话中断</span><br>
          <i style="color:#09C937">37<br>人</i>
      </div>
      <div class="pop-item pop-f">
          <span>承诺马上还款</span><br>
          <i>1237<br>人</i>
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
    },
    
  }
};
</script>
<style lang="less" scoped>

.popToRight {
  font-size: 12px;
  position: relative;
  overflow: hidden;
  width: 100%;
  height: 356px;
  background: url("../../assets/images/p4-table-title.png") no-repeat center;
  background-size: 100% 100%;
  padding: 14px 10px;
  .pop-title {
    text-align: left;
    padding-left: 24px;
  }
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
      height: 140px;
      width: 33%;
      background: url('../../assets/images/circle/1-lv.png') no-repeat center;
      background-size: 100%;
      padding-top: 38px;
      i {
        color: #E19513;
      } 
      
    }
    .pop-b {
      height: 120px;
      width: 30%;
      background: url('../../assets/images/circle/oval.png') no-repeat center;
      background-size: 100%;
      padding-top: 38px;
      i {
        color: #17D9F2;
      }
    }
    .pop-c {
      height: 120px;
      width: 30%;
      background: url('../../assets/images/circle/1-lv.png') no-repeat center;
      background-size: 100%;
      padding-top: 38px;
      i {
        color: #E19513;
      }
    }
    .pop-d {
      height: 140px;
      width: 33%;
      background: url('../../assets/images/circle/oval.png') no-repeat center;
      background-size: 80%;
      padding-top: 38px;
      i {
        color: #ABD043;
      } 
      
    }
    .pop-e {
      height: 120px;
      width: 30%;
      background: url('../../assets/images/circle/1-lv.png') no-repeat center;
      background-size: 100%;
      padding-top: 38px;
      i {
        color: #17D9F2;
      }
    }
    .pop-f {
      height: 120px;
      width: 30%;
      background: url('../../assets/images/circle/oval.png') no-repeat center;
      background-size: 95%;
      padding-top: 38px;
      i {
        color: #17D9F2;
      }
    }
  }
  
}

</style>