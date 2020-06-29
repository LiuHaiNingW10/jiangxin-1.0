<template>
  <div class="microfinance-chart">
    <div class="mc-header">当日小微信贷服务金额<span>1232次/小时</span></div>
    <div class="mc-content">
      <div class="mc-l">
        <ul>
          <li>
            <img src="../../assets/images/p3/frm-cricle1.png" alt="">
            <div>
              <div class="title">固态层</div>
              <div class="desc">已知风险防控策略调用量<span>700</span></div>
            </div>
          </li>
          <li>
            <img src="../../assets/images/p3/frm-cricle2.png" alt="">
            <div>
              <div class="title">固态层</div>
              <div class="desc">已知风险防控策略调用量<span>700</span></div>
            </div>
          </li>
          <li>
            <img src="../../assets/images/p3/frm-cricle1.png" alt="">
            <div>
              <div class="title">固态层</div>
              <div class="desc">已知风险防控策略调用量<span>700</span></div>
            </div>
          </li>
          <li>
            <img src="../../assets/images/p3/frm-cricle2.png" alt="">
            <div>
              <div class="title">固态层</div>
              <div class="desc">已知风险防控策略调用量<span>700</span></div>
            </div>
          </li>
        </ul>
      </div>
      <div class="mc-c"></div>
      <div class="mc-r">
        <div class="title">FRM命中分布</div>
        <ul>
          <li>5%</li>
          <li>60%</li>
          <li>10%</li>
          <li>15%</li>
        </ul>
      </div>
    </div>
    <div class="mc-footer">当日小微信贷服务金额<span>0.00004%</span></div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  mounted () {
    
  },
  methods: {
   
  },
  components: {}
};
</script>

<style lang="less" scoped>
.microfinance-chart{
  width: 736px;
  margin: 0 auto;
  height: 30%;
  .mc-header, .mc-footer{
    font-size: 18px;
    color: rgba(255,255,255,0.72);
    text-align: center;
    &>span{
      font-size: 24px;
      color: #fff;
      margin-left: 8px;
    }
  }
  .mc-content{
    display: flex;
    overflow: hidden;
    .mc-l{
      margin-top: 40px;
      ul{
        list-style: none;
        li{
          width: 291px;
          height: 68px;
          background: url('../../assets/images/p3/frm-l.png') no-repeat;
          overflow: hidden;
          margin-bottom: 28px;
          img{
            float: left;
            width: 38px;
            height: 38px;
            margin: 15px;
          }
          .title{
            font-size: 20px;
            line-height: 1;
            margin-top: 15px;
          }
          .desc{
            font-size: 14px;
            line-height: 1;
            &>span{
              font-size: 24px;
              margin-left: 7px;
            }
          }
        }
      }
    }
    .mc-c{
      width: 212px;
      height: 412px;
      background: url('../../assets/images/p3/frm-c.png') no-repeat;
      margin: 0 46px 0 27px;
    }
    .mc-r{
      margin-top: 15px;
      width: 160px;
      height: 410px;
      background: url('../../assets/images/p3/frm-r.png') no-repeat;
      padding: 12px 24px;
      .title{
        font-size: 18px;
        color: rgba(255,255,255,.7);
        margin-bottom: 10px;
      }
      ul>li{
        list-style: none;
        font-size: 20px;
        color: #fff;
        margin-bottom: 65px;
      }
    }
  }
}
</style>
