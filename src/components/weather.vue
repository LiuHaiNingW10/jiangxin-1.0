<template>
  <div class="weather-module">
    <img :src="currentImg" alt />
    <span class>{{currentWeather.high}} ℃ ~ {{currentWeather.low}} ℃</span>
    <span class="time-span">{{currentTime.date}}</span>
    <span class="time-span">{{currentTime.time}}</span>
  </div>
</template>

<script>
import Yin from "../assets/images/yin.png";
import Yu from "../assets/images/yu.png";

export default {
  name: "",
  props: [],
  mounted() {
    this.getTime();
  },
  data() {
    return {
      // 天气
      localweather: [
        { high: "29", low: "22" },
        { high: "28", low: "23" },
        { high: "27", low: "23" },
        { high: "25", low: "23" },
        { high: "26", low: "22" },

        { high: "29", low: "22" },
        { high: "29", low: "22" }
      ],
      weatherImg: [Yin, Yu, Yu, Yu, Yu, Yu, Yu],

      currentWeather: {},
      currentImg: "",

      // 时间
      currentTime: {},
      currentDate: undefined
    };
  },
  computed: {},
  methods: {
    // 获取右上角当前时间
    getTime() {
      if (timeInterval) clearInterval(timeInterval);
      var timeInterval = setInterval(this.nowTime, 1000);
    },

    nowTime() {
      var myDate = new Date();
      var y = myDate.getFullYear();
      var M = myDate.getMonth() + 1; //获取当前月份(0-11,0代表1月)
      var d = myDate.getDate(); //获取当前日(1-31)
      var h = myDate.getHours(); //获取当前小时数(0-23)
      var m = myDate.getMinutes(); //获取当前分钟数(0-59)
      var s = myDate.getSeconds(); //获取当前秒数(0-59)

      //检查是否小于10
      M = this.check(M);
      d = this.check(d);
      h = this.check(h);
      m = this.check(m);
      s = this.check(s);
      var dateStr = y + "/" + M + "/" + d;
      var timeStr = h + ":" + m + ":" + s;
      this.currentTime = Object.assign({}, { date: dateStr, time: timeStr });
      // 获取当前天气
      this.getCurrentWeather(myDate.getDate());
      // document.getElementById("nowtime").innerHTML = "当前时间：" + timestr;
    },

    //时间数字小于10，则在之前加个“0”补位。
    check(i) {
      var num = i < 10 ? "0" + i : i;
      return num;
    },

    // 写死天气
    getCurrentWeather(d) {
      if (this.currentDate === d) {
        return;
      }
      var index = 0;
      if (d > 6) {
        this.currentWeather = this.localweather[index];
        this.currentImg = this.weatherImg[index];
      } else {
        this.currentWeather = this.localweather[d];
        this.currentImg = this.weatherImg[d];
      }
    }
  },
  components: {}
};
</script>

<style lang="less">
</style>
