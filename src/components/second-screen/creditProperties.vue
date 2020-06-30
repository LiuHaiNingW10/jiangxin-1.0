<template>
  <div class="credit-properties">
    <div class="content-title">
      <span class="first-title-span">{{typeData.first}}</span>
    </div>
    <div class="num-and-coin">
      <div class="top-left top-single">
        <div class="portrait">
          <img class="portrait-img" src="../../assets/images/icon2.png" alt />
        </div>
        <div class="server-num num-span">
          <div class="num-span-title">服务人数</div>
          <div class="middle-rec"></div>
          <div class="num-span-data">22,390人</div>
        </div>
      </div>
      <div class="top-middle"></div>
      <div class="top-right top-single">
        <div class="portrait">
          <img class="portrait-img" src="../../assets/images/icon1.png" alt />
        </div>
        <div class="shouxin-num num-span">
          <div class="num-span-title">授信金额</div>
          <div class="middle-rec"></div>
          <div class="num-span-data">22,390,123,123元</div>
        </div>
      </div>
    </div>

    <div class="middle-title">
      <div class="middle-left middle-single">{{typeData.littleTitle[0]}}</div>
      <div class="middle-right middle-single">{{typeData.littleTitle[1]}}</div>
    </div>

    <div class="bottom-graph">
      <div v-if="typeData.type === 1">
        <div></div>
      </div>
      <div v-else-if="typeData.type === 2" class="bottom-graph-div">
        <person-columnar
          class="person-columnar"
          :ids="industryColumnarId"
          :chartData="industryColumnarData"
        />
        <person-columnar
          class="person-columnar"
          :ids="enterpriseColumnarId"
          :chartData="enterpriseColumnarData"
        />
      </div>
    </div>
  </div>
</template>

<script>
import PersonColumnar from "./personColumnar.vue";
export default {
  name: "",
  props: ["typeData"],
  mounted() {},
  data() {
    return {
      industryColumnarData: {},
      industryColumnarId: "industryColumnar",
      enterpriseColumnarData: {},
      enterpriseColumnarId: "enterpriseColumnar"
    };
  },
  computed: {},
  methods: {},
  components: {
    "person-columnar": PersonColumnar
  }
};
</script>

<style lang="less">
.credit-properties {
  padding: 1.97% 3.47%;
  .content-title {
    width: 100%;
    height: 9.55%;
    background: url("../../assets/images/bg-content-title.png") no-repeat;
    background-size: 100% 100%;
    margin-bottom: 2%;
    .first-title-span {
      display: block;
      padding-top: 1.5%;
      margin-left: 5%;
    }
  }
  .num-and-coin {
    width: 100%;
    height: 17.36%;
    background: url("../../assets/images/group-7.png") no-repeat;
    background-size: 100% 100%;
    margin-bottom: 2%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    .top-single {
      display: flex;
      align-items: center;
      width: 49%;
      height: 100%;
      padding: 0.5% 6%;
      .portrait {
        height: 70%;
        width: 22%;
        background: url("../../assets/images/portrait-left.svg") no-repeat;
        background-size: 100% 100%;
        text-align: center;
        .portrait-img {
          padding-top: 16%;
        }
      }
      .num-span {
        width: 88%;
        height: 88%;
        .num-span-title {
          font-size: 21px;
        }
        .middle-rec {
          background: url("../../assets/images/num-cut-off.png") no-repeat;
          background-size: 100% 100%;
          width: 100%;
          height: 10%;
        }
        .num-span-data {
          font-size: 36px;
          color: #04bbff;
        }
      }
    }
    .top-middle {
      width: 0.3%;
      height: 66%;
      background: url("../../assets/images/rectangle.png") no-repeat;
      background-size: 100% 100%;
    }
  }
  .middle-title {
    width: 100%;
    height: 6.36%;
    display: flex;
    justify-content: space-around;
    .middle-single {
      width: 44.18%;
      height: 100%;
      font-size: 24px;
      padding-left: 3%;
      line-height: 170%;
      background: url("../../assets/images/title4.png") no-repeat;
      background-size: 100% 100%;
    }
  }
  .bottom-graph {
    height: 57.7%;
    width: 100%;
    .bottom-graph-div {
      height: 100%;
      width: 100%;
      display: flex;
      flex-wrap: nowrap;
      .person-columnar {
        height: 100%;
        width: 45%;
        &:first-child {
          margin-right: 5%;
        }
      }
    }
  }
}
</style>
