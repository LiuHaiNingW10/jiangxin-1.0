<template>
  <div class="table-auto">
    <div class="p4-table-title">
      <span style="font-size: 32px;font-weight:bold">{{id.title}}</span>
    </div>
    <div class="scroll-box">
      <a-table
        class="table-scroll"
        :style="{top}"
        :columns="columns"
        :rowKey="(record,index) => record.mobile+index"
        :dataSource="tableData"
        :pagination="false"
        :showHeader="true"
      >
        <span slot="status" slot-scope="status, record">
          <a-tag
            v-for="tag in status"
            :key="tag.id"
            :class="status-tag"
            :color="tag.value === '正常' ? '#24C768' : tag.value === '异常'? '#D70907' : '#F8B551'"
          ></a-tag>
          {{record.status[0].value}}
        </span>
      </a-table>
    </div>
  </div>
</template>
<script>
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
      timeouts: this.timeout
    };
  },
  computed: {
    top() {
      return -this.activeIndex * 25 + "px" + `;height:${this.height / 2}`;
    }
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.timer = setInterval(() => {
        this.autoScroll();
      }, 3000);
    },
    autoScroll() {
      let time = this.activeIndex % 4 === 0 ? 6000 : 3000;
      let a = setInterval(_ => {
        this.tableData = this.tableData.concat(
          this.initData[this.activeIndex] || []
        );
        if (this.activeIndex < this.tableData.length) {
          this.activeIndex += 1;
        } else {
          this.activeIndex = 0;
        }
        this.tableData.shift();
        clearInterval(a);
        this.needPoint && this.$emit("func", this.tableData[0]);
      }, time);
    }
  },
  beforeDestroy() {
    clearInterval(this.timer);
  }
};
</script>
<style lang="less">
.table-auto {
  overflow: hidden;
  padding: 4%;
  height: 32%;
  .scroll-box {
    background-size: 94% 98%;
    padding: 3% 4%;
    .table-scroll {
      overflow: hidden;
      height: 30%;
    }
  }
  tr {
    border-bottom: 1px solid rgba(140, 141, 142, 0.38);
  }
  .table-td {
    color: #fff;
    padding: 8px 4px;
    background: transparent;
    border: none;
    font-size: 20px;
  }
  th.table-td {
    color:#85BEFC;
    font-weight: bold;
    font-size: 22px;
  }
  .ant-tag {
    height: 4px;
    border-radius: 0px;
  }
}
.p4-table-title {
  text-align: left;
  padding-left: 2%;
}
</style>