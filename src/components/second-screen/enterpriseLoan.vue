<template>
  <div class="enterprise-loan">
    <div class="cloud-content enterprise-div">
      <div class="content-title">
        <span class="first-text-span">小微企业贷款</span>
        <loan-chart />
      </div>
    </div>
    <div class="enterprise-atlas enterprise-div enterprise-content">
      <div class="second-title">
        <span class="text-span">企业图谱</span>

      </div>
    </div>
    <div class="enterprise-credit enterprise-div enterprise-content">
      <div class="second-title">
        <span class="text-span">企业信用</span>
        <credit-chart />
      </div>
    </div>
  </div>
</template>

<script>
import LoanChart from '@/components/second-screen/loanChart'
import CreditChart from '@/components/second-screen/creditChart'
export default {
  name: "",
  props: [],
  mounted() {},
  data() {
    return {};
  },
  computed: {},
  methods: {},
  components: {
    LoanChart,
    CreditChart
  }
};
</script>

<style lang="less">
.enterprise-loan {
  .enterprise-div {
    width: 100%;
    height: 32%;
    margin-bottom: 1%;
    padding: 1.7% 1.97%;
    .second-title {
      width: 100%;
      height: 9.22%;
      font-size: 24px;
      background: url("../../assets/images/title4备份 4.png") no-repeat;
      background-size: 100% 100%;
    }
  }
  .cloud-content {
    .content-title {
      width: 100%;
      height: 12.96%;
      background: url("../../assets/images/bg-content-title.png") no-repeat;
      background-size: 100% 100%;
      padding-left: 4.64%;
      margin-bottom: 2%;
      line-height: 220%;
    }
  }
  .enterprise-content {
    padding: 0 4%;
  }
  .text-span {
      display: inline-block;
      margin-left: 4%;
  }
}
</style>
