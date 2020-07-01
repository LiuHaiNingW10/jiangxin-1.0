<template>
  <div class="brain-content" @keyup.enter="keyEnter">
    <div class="brain-main">
      <div class="light-spot">
        <div class="a" v-if="showImg">
          <span class="a-address">
            <i>{{tableData[0].accent}}</i>
          </span>
          <span class="a-tel">
            <i>电话：{{tableData[0].mobile}}</i>
            <br />
            <i class="a-want">{{tableData[0].want}}</i>
          </span>
        </div>
        <div class="b" v-if="showImg">
          <span class="a-address">
            <i>{{tableData[1].accent}}</i>
          </span>
          <span class="a-tel">
            <i>电话：{{tableData[1].mobile}}</i>
            <br />
            <i class="a-want">{{tableData[1].want}}</i>
          </span>
        </div>
        <div class="c" v-if="showImg">
          <span class="a-address">
            <i>{{tableData[2].accent}}</i>
          </span>
          <span class="a-tel">
            <i>电话：{{tableData[2].mobile}}</i>
            <br />
            <i class="a-want">{{tableData[2].want}}</i>
          </span>
        </div>
        <div class="d" v-if="showImg">
          <span class="a-address">
            <i>{{tableData[3].accent}}</i>
          </span>
          <span class="a-tel">
            <i>电话：{{tableData[3].mobile}}</i>
            <br />
            <i class="a-want">{{tableData[3].want}}</i>
          </span>
        </div>
        <div class="e" v-if="showImg">
          <span class="e-address">
            <i>{{tableData[4].accent}}</i>
          </span>
          <span class="a-tel">
            <i>电话：{{tableData[4].mobile}}</i>
            <br />
            <i class="a-want">{{tableData[4].want}}</i>
          </span>
        </div>
        <div class="f" v-if="showImg">
          <span class="f-address">
            <i>{{tableData[5].accent}}</i>
          </span>
          <span class="a-tel">
            <i>电话：{{tableData[5].mobile}}</i>
            <br />
            <i class="a-want">{{tableData[5].want}}</i>
          </span>
        </div>
        <canvas id="audio-art"></canvas>
        <audio id="audio" :src="require('../../assets/video/shanxi.wav')" ref="audio"></audio>
      </div>
    </div>
    <div class="brain-foot">
      <span class="foot-ai"></span>
      <ul>
        <li>
          <img :src="require('../../assets/images/brain-e.png')" alt />
          <span>智能运营</span>
        </li>
        <li class>
          <img :src="require('../../assets/images/brain-g.png')" alt />
          <span>智能办公</span>
        </li>
        <li class>
          <img :src="require('../../assets/images/brain-c.png')" alt />
          <span>智能机具</span>
        </li>
        <li class>
          <img :src="require('../../assets/images/brain-f.png')" alt />
          <span>智能运维</span>
        </li>
        <li class>
          <img :src="require('../../assets/images/brain-a.png')" alt />
          <span>智能审计</span>
        </li>
        <li class>
          <img :src="require('../../assets/images/brain-d.png')" alt />
          <span>智能设备</span>
        </li>
        <li class>
          <img :src="require('../../assets/images/brain-b.png')" alt />
          <span>智能客服</span>
        </li>
      </ul>
      <audio id="audio" :src="require('../../assets/video/shanxi.wav')"></audio>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import a from "../../assets/images/brain-a.png";
import b from "../../assets/images/brain-b.png";
import c from "../../assets/images/brain-c.png";
import d from "../../assets/images/brain-d.png";
import e from "../../assets/images/brain-e.png";
import f from "../../assets/images/brain-f.png";
import g from "../../assets/images/brain-g.png";
export default {
  name: "tableAuto",
  props: ["tableDatas", "ids", "heights"],
  data() {
    return {
      tableData: this.tableDatas,
      id: this.ids,
      height: this.heights,
      lineData: [],
      showImg: true,
      analyser: {},
      want: {
        承诺还款: "color: #4DC4D3",
        未接听: "color: #4DC4D3",
        承诺还款: "color: #4DC4D3",
        承诺还款: "color: #4DC4D3",
        承诺还款: "color: #4DC4D3"
      }
    };
  },
  computed: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {
      document.onkeydown = event => {
        let e = event || window.event || arguments.callee.caller.arguments[0];
        if (e && e.keyCode == 13) {
          if (!this.showImg) {
            return;
          }
          this.DrawVideo();
        }
      };
    },
    transLateData() {
      let obj = {
        type: "bar",
        yAxis: [],
        series: []
      };
      this.tableData.forEach(el => {
        obj.series.push(el.num);
        obj.yAxis.push(el.categroy);
      });
      this.lineData = obj.series;
      this.drawLineG(this.id.id, obj);
    },
    playVideo() {
      // let audio = new Audio();
      // audio.src = require("../../assets/video/shanxi.wav");
      // audio.play();
      this.DrawVideo();
    },
    DrawVideo() {
      this.showImg = false;
      this.$emit("func", { value: false });
      var atx = new (window.AudioContext || webkitAudioContext)();
      // var audio = document.getElementById("audio");
      let audio = new Audio();
      audio.src = require("../../assets/video/shanxi.wav");
      var canvas = document.getElementById("audio-art");
      var ctx = canvas.getContext("2d");

      var source = atx.createMediaElementSource(audio);
      var analyser = atx.createAnalyser();
      source.connect(analyser);
      analyser.connect(atx.destination);

      analyser.fftSize = 2048;
      var draw = () => {
        var cWidth = canvas.width,
          cHeight = canvas.height,
          // frequencyBinCount的值固定为fftSize的一半
          audioArray = new Uint8Array(analyser.frequencyBinCount);
        // 解析频率数据，放入audioArray数组中
        analyser.getByteFrequencyData(audioArray);
        // 填充为柱状图
        ctx.clearRect(0, 0, cWidth, cHeight);
        ctx.fillStyle = "#60D7FB";
        for (var i = 0; i < audioArray.length; i++) {
          ctx.fillRect(i * 3, cHeight - audioArray[i], 2, cHeight);
        }
        // 刷新
        requestAnimationFrame(draw);
      };
      draw();
      audio.play();
      let audioTime = this.$refs.audio.duration;
      setTimeout(() => {
        this.$emit("func", { value: true });
        this.$nextTick(() => {
          this.showImg = true;
        });
      }, audioTime * 1000);
    }
  }
};
</script>
<style lang="less" scoped>
.brain-content {
  font-size: 12px;
  background-size: 40%;
  position: relative;
  height: 100%;
  .light-spot {
    width: 92%;
    margin: 0 auto;
    min-height: 400px;
    min-height: 450px;
    position: relative;
    div {
      display: inline-block;
      position: absolute;
      height: 54%;
      width: 22%;
      padding: 2.2% 2%;
      z-index: 2;
    }
    audio {
      display: none;
    }
    i {
      font-style: normal;
    }
    .a-address {
      display: inline-block;
      height: 30px;
      background: url("../../assets/images/p4icon1.png") no-repeat center;
      background-position: 4px 0;
      background-size: 24%;
      padding: 2px 38px;
      line-height: 26px;
    }
    .e-address {
      display: inline-block;
      height: 30px;
      background: url("../../assets/images/p4icon1.png") no-repeat center;
      background-position: 4px 0;
      background-size: 16%;
      padding: 2px 0 0px 34px;
      line-height: 26px;
    }
    .f-address {
      display: inline-block;
      height: 30px;
      background: url("../../assets/images/p4icon1.png") no-repeat center;
      background-position: 4px 0;
      background-size: 31%;
      padding: 2px 0 0 38px;
      line-height: 26px;
    }
    .a-tel {
      display: inline-block;
      width: 80%;
      margin-top: 4%;
    }
    .a-want {
      color: #4dc4d3;
    }
    .a {
      top: 187px;
      left: 363px;
      background: url("../../assets/images/popup-l.png") no-repeat center;
      background-size: 100%;
    }
    .b {
      top: 396px;
      left: 190px;
      background: url("../../assets/images/popup-l.png") no-repeat center;
      background-size: 100%;
    }
    .c {
      top: 730px;
      left: 511px;
      background: url("../../assets/images/popup-p.png") no-repeat center;
      background-size: 100%;
    }
    .d {
      top: 469px;
      left: 936px;
      background: url("../../assets/images/popup-o.png") no-repeat center;
      background-size: 100%;
      padding-left: 126px;
    }
    .e {
      top: 236px;
      right: 374px;
      background: url("../../assets/images/popup-r.png") no-repeat center;
      background-size: 100%;
      padding-left: 128px;
    }
    .f {
      top: 616px;
      right: 284px;
      background: url("../../assets/images/popup-o.png") no-repeat center;
      background-size: 100%;
      padding-left: 128px;
    }
  }
  .brain-foot {
    position: absolute;
    bottom: 0px;
    width: 100%;
    text-align: center;
    .foot-ai {
      display: inline-block;
      width: 30%;
      height: 60px;

      background: url("../../assets/images/ai-foot.png") no-repeat center;
      background-size: 50%;
      margin-bottom: 30px;
    }
    ul {
      display: flex;
      justify-content: center;
      width: 90%;
      margin: 0 auto;
      li {
        list-style: none;
        display: inline-block;
        width: 10%;
        margin-right: 30px;
        text-align: center;
        img {
          width: 100%;
          margin-bottom: 10px;
        }
      }
    }
  }
  .video {
    display: none;
  }
  #audio-art {
    position: absolute;
    width: 100%;
    top: 0;
    height: 60%;
    margin: 0 auto;
    z-index: 1;
  }
}
</style> 