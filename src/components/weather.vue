<template>
  <div class="weather-module">
    <img :src="currentImg" alt class="weather-img" />
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
      currentDate: undefined,
      timeInterval: undefined
    };
  },
  computed: {},
  methods: {
    // 获取右上角当前时间
    getTime() {
      // if (timeInterval) clearInterval(timeInterval);
      this.timeInterval = setInterval(this.nowTime, 1000);
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
    },

    // 获取当前天气
    getWeather() {
      var _this = this;
      this.axios
        .get(
          "https://tianqiapi.com/api?version=v6&appid=71549884&appsecret=XH6bWw5A"
        )
        .then(function(response) {
          //   _this.localweather = response.data;
          //   _this.weatherImg =
          //     "http://tq.daodaoim.com//tianqiapi/skin/pitaya/" +
          //     response.data.wea_img +
          //     ".png";
        })
        .catch(() => {});

      // this.axios({
      //   url: "https://www.tianqiapi.com/api/?version=v1&cityid=101280601",
      //   method: "get",
      //   data: {
      //     results: 10
      //   },
      //   type: "json"
      // })
      //   .then(res => {
      //     // let datas = res.data.data[0];//下标为0即表示当天天气数据
      //     console.log(res.data);
      //   })
      //   .catch(err => {
      //     console.log(err);
      //   });
    }
  },
  beforeDestroy() {
    clearInterval(this.timeInterval);
  },
  components: {}
};
</script>

<style lang="less">
.weather-img {
  width: 3%;
}
.time-span {
  display: inline-block;
  margin-left: 5%;
}
</style>
