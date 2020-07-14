<template>
  <div class="table-auto">
    <div class="p4-table-title">
      <span style="font-size: 32px;font-weight:bold">{{id.title}}</span>
    </div>
    <div class="scroll-box">
      <ul>
        <li v-for="item in columns" :key="item.key" :class="item.class">{{item.title}}</li>
      </ul>
      <table>
        <vue-seamless-scroll :data="tableData" :class-option="optionHover" class="seamless-warp">
        <tbody>
          <tr v-for="it in tableData" :key="it.key">
            <td v-for="item in columns" :key="item.key" :class="item.class">{{it[item.key] || '--'}}</td>
          </tr>
        </tbody>
        </vue-seamless-scroll>
      </table>
    </div>
  </div>
</template>
<script>
import vueSeamlessScroll from "vue-seamless-scroll";
export default {
  name: "tableAuto",
  props: ["tableDatas", "ids", "columns", "heights", "needPoint"],
  data() {
    return {
      todos: [],
      initData: this.tableDatas,
      tableData: this.tableDatas,
      id: this.ids,
      column: this.columns,
      activeIndex: 0,
      view: "",
      height: this.heights,
      timeouts: this.timeout,
      animate: false,
    };
  },
  components: {
    vueSeamlessScroll
  },
  created() {
    setTimeout( () => {
      this.animate = true
    },2000)
  },
  computed: {
    top() {
      return -this.activeIndex * 25 + "px" + `;height:${this.height / 2}`;
    },
    optionHover() {
      return {
        autoPlay: this.animate,
        hoverStop: false, // 鼠标悬停停止滚动
        direction: 1, // 向下/上滚动
        step: 0.8, // 滚动速度
        singleHeight: 78*4, // 滚动单行
        waitTime: 2000 // 单行停顿时间
      };
    }
  },
  
  methods: {
    scroll() {
      setTimeout(() => {
        //  这里直接使用了es6的箭头函数，省去了处理this指向偏移问题，代码也比之前简化了很多
        if (this.activeIndex < this.tableData.length-1) {
          this.activeIndex += 1;
        } else {
          this.activeIndex = 0;
        }
        this.needPoint && this.$emit("func", this.tableData[0]);
        this.tableData.push(this.initData[this.activeIndex]); // 将数组的第一个元素添加到数组的
        this.tableData.shift(); //删除数组的第一个元素
        this.animate = false; // margin-top 为0 的时候取消过渡动画，实现无缝滚动
      }, 500);
    }
  },
  beforeDestroy() {
    clearInterval(this.timer);
  }
};
</script>
<style lang="less" scoped>
.table-auto {
  overflow: hidden;
  padding: 0 4%;
  height: 32%;
  .scroll-box {
    background-size: 94% 98%;
    padding: 0 4%;
    ul {
      width: 100%;
      padding: 3%;
      li {
        list-style: none;
        display: inline-block;
        width: 20%;
        text-align: center;
        color:#85BEFC;
        font-weight: bold;
        font-size: 22px;
      }
      :nth-child(2) {
        padding-left: 4%;
      }
      :nth-child(3) {
        padding-right: 1%;
      }
      :nth-child(4) {
        padding-right: 1%;
      }
      :nth-child(5) {
        padding-left: 5%;
      }
    }
    table {
      tbody {
        tr {
          border-bottom: 1px solid rgba(140, 141, 142, 0.38);
          height: 78px;
          text-align: center;
          padding: 10px;
          width: 100%;
        }
      }
      td {
        color: #fff;
        text-align: center;
        padding: 2% 0;
        background: transparent;
        border: none;
        font-size: 20px;
      }
      .table-mobile {
        min-width: 240px
      }
      .table-status {
        min-width: 120px
      }
      .table-problem {
        min-width: 260px
      }
      .table-action {
        min-width: 200px;
        margin-left: -2%;
      }
      .table-handle {
        min-width: 200px
      }
    }
  }
}
.p4-table-title {
  text-align: left;
  padding-left: 2%;
}
.seamless-warp {
  height: 40%;
  overflow: hidden;
}
</style>