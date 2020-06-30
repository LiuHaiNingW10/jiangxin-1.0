<template>
  <div class="form-chart">
    <div class="single-form-chart" v-for="(item) in ids" :key="item.id">
      <div class="form-chart-title">{{item.title}}</div>
      <table>
        <tbody>
          <tr v-for="(items, index) in chartData[item.id]" :key="index">
            <th class="ranking">Top{{index + 1}}</th>
            <th>{{items.xid}}</th>
            <th>{{items.score}}</th>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "",
  props: ["ids", "chartData"],
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
.form-chart {
  padding-top: 6%;
  display: flex;
  .single-form-chart {
    width: 50%;
    table {
      width: 100%;
      tr {
        height: 60px;
        line-height: 60px;
      }
    }
    .ranking {
      color: #00ffff;
    }
  }
}
</style>
