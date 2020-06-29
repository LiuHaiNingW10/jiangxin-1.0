<template>
  <div class="second-screen">
    <!-- 头部 -->
    <div class="title-frame">
      <div class="title-left"></div>
      <div class="global-title">累计服务金额/当日服务金额</div>
      <div class="right-time">
        <weather-com />
      </div>
    </div>
  </div>
</template>

<script>
import WeatherCom from "../../components/weather.vue";
export default {
  mounted() {},
  data() {
    return {};
  },
  computed: {},
  methods: {},
  components: {
    "weather-com": WeatherCom
  }
};
</script>

<style lang="less">
.second-screen {
  width: 4224px;
  height: 1536px;
  // width: 100%;
  // height: 100%;
  background: url("../../assets/images/background-second.png") no-repeat;
  background-size: 100% 100%;
  padding-top: 1%;
  font-size: 28px;
  color: #fff;

  // 标题样式
  .title-frame {
    width: 100%;
    height: 5.7%;
    background: url("../../assets/images/header.png") no-repeat;
    background-size: 100% 100%;
    display: flex;
    justify-content: space-between;
    padding: 0 1%;
    .title-left {
      width: 33%;
    }
    .global-title {
      width: 33%;
      text-align: center;
      font-size: 40px;
      font-weight: bold;
    }
    .right-time {
      width: 33%;
      text-align: right;
    }
    .time-span {
      display: inline-block;
      margin-left: 5%;
    }
  }
}
</style>
