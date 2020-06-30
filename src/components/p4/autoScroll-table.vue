<template>
    <div class="table-auto table-bg">
        <div class="table-title">
          <span>{{id.title}}</span>
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
                :key="record.id"
                :class="status-tag"
                :color="tag === '正常' ? '#24C768' : tag === '异常'? '#D70907' : '#F8B551'"
              >
              </a-tag>
              {{record.status[0]}}
            </span>
          </a-table>
        </div>
    </div>
</template>
<script>
export default {
  name: "tableAuto",
  props: ['tableDatas', 'ids','columns','heights'],
  data() {
    return {
      todos: [],
      initData: this.tableDatas,
      tableData: this.tableDatas,
      id: this.ids,
      column: this.columns,
      activeIndex: 0,
      view:'',
      height: this.heights
    };
  },
  computed: {
    top() {
      return -this.activeIndex * 25 + "px"+`;height:${this.height/2}`;
    },

  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      var n = setInterval( () => {
        var circle = this.autoScroll()
      },1000)
      
    },
    autoScroll() {
      let time = this.activeIndex%5 === 0 ? 6000: 1000
      var a = setInterval(_ => {
          let free = Math.floor(Math.random()*12)+1;
          this.tableData = this.tableData.concat(this.initData[free]||[]);
          if (
            this.activeIndex <
            this.tableData.length
          ) {
            this.activeIndex += 1;
          }else {
            this.activeIndex = 0
          }
          this.tableData.shift()
          clearInterval(a)
      }, time);
      
    },
  }
}
</script>
<style lang="less">
  .table-auto {
    font-size: 12px;
    position: relative;
    overflow: hidden;
    .table-title {
      text-align: left;
      padding-left: 24px;
    }
    .scroll-box {
      overflow: hidden;
      height: 280px;
    }
    .table-scroll {
      position: relative;
      transition: top  0.825s;
      tbody {
        tr:nth-child(even) {  
            // background: #;  
        }  
        tr:nth-child(odd) {  
            background: #0E3B8C;  
        }  
      }
    }
    tr {
      border-bottom: 1px dashed #484444 ;
    }
    .table-td {
      color: #7CA3D0;
      padding: 8px 4px;
      background: transparent;
      font-size: 12px;
      border: none;
    }
    .ant-tag {
        height: 4px;
        border-radius: 0px;
    }
  }
  .table-bg {
    background: url('../../assets/images/p4-table-title.png') no-repeat center;
    background-size: 100% 100%;
    padding: 14px 10px;
  }
</style>