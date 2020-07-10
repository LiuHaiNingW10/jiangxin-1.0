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
          <!-- <tr>
            <th>图标</th>
            <th>指标</th>
            <th>数值</th>
            <th>单位</th>
          </tr>-->
          <tr v-for="(items, index) in chartData[item.id]" :key="index">
            <th
              v-for="(thItem, thIndex) in items"
              :key="thIndex"
              :class="thItem.style"
            >{{thItem.name}}</th>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "",
  props: ["ids", "chartData", "tableTitle"],
  mounted() {},
  data() {
    return {};
  },
  computed: {},
  methods: {},
  components: {}
};
</script>

<style lang="less">
.new-form-chart {
  display: flex;
  margin-bottom: 3%;
  .single-form-chart {
    width: 100%;
    table {
      width: 100%;
      .table-head {
        font-size: 32px;
      }
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
