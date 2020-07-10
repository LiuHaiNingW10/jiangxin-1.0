<template>
  <div class="new-form-chart">
    <div class="single-form-chart" v-for="(item) in ids" :key="item.id">
      <!-- <div class="form-chart-title">{{item.title}}</div> -->
      <table>
        <thead v-if="tableTitle">
          <tr class="table-head">
            <th colspan="4" style="text-align: center;">{{tableTitle}}</th>
          </tr>
        </thead>
        <tbody>
          <!-- <tr v-for="(items, index) in chartData[item.id]" :key="index">
            <th
              v-for="(thItem, thIndex) in items"
              :key="thIndex"
              :class="thItem.style"
            >{{thItem.name}}</th>
          </tr>-->
          <tr v-if="tableHeader">
            <th v-for="(headerItem, headerIndex) in tableHeader" :key="headerIndex">{{headerItem}}</th>
          </tr>
          <tr v-for="(items, index) in chartData[item.id]" :key="index">
            <td
              v-for="(thItem, thIndex) in tableIndex"
              :key="thIndex"
              :class="thItem['style']"
            >{{items[thItem['dataIndex']]}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "",
  props: ["ids", "chartData", "tableTitle", "tableIndex", "tableHeader"],
  mounted() {},
  data() {
    return {};
  },
  computed: {},
  methods: {},
  components: {}
};
</script>

<style lang="less" scoped>
.new-form-chart {
  display: flex;
  margin-bottom: 3%;
  min-height: 200px;
  .single-form-chart {
    width: 100%;
    table {
      width: 100%;
      font-size: 32px;
      // .table-head {
      //   font-size: 32px;
      // }
      tr {
        width: 100%;
        height: 60px;
        line-height: 60px;
        
        .img-span {
          width: 30px;
        }
        th {
          width: 33%;
        }
      }
    }
    .ranking {
      color: #00ffff;
    }
    .number-div {
      color: #00ffff;
    }
  }
}
</style>
