<template>
  <div class="portrayal-server">
    <div class="exp-title-frame">
      <span class="title-text">画像：服务地域TOP10</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "",
  props: [],
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
.portrayal-server {
  padding-top: 2%;
  .exp-title-frame {
    width: 100%;
    height: 15.85%;
    background: url("../../assets/images/title2.png") no-repeat;
    background-size: 100% 100%;
    .title-text {
      display: inline-block;
      padding-top: 1.3%;
      margin-left: 3.5%;
    }
  }
}
</style>
