<template>
  <div class="microfinance-chart">
    <div class="mc-header">近一小时攻击数<span>{{attckTimes}}次/小时</span></div>
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
              <div class="title">聚集层</div>
              <div class="desc">批量风险防控策略调用量<span>1200</span></div>
            </div>
          </li>
          <li>
            <img src="../../assets/images/p3/frm-cricle1.png" alt="">
            <div>
              <div class="title">场景层</div>
              <div class="desc">异常行为识别策略调用量<span>500</span></div>
            </div>
          </li>
          <li>
            <img src="../../assets/images/p3/frm-cricle2.png" alt="">
            <div>
              <div class="title">动态层</div>
              <div class="desc">未知风险识别策略调用量<span>300</span></div>
            </div>
          </li>
        </ul>
      </div>
      <div class="mc-c"></div>
      <div class="mc-r">
        <div class="title">FRIM命中分布</div>
        <div class="frim-chart" id='frimChart'></div>
        <!-- <ul>
          <li>5%</li>
          <li>60%</li>
          <li>10%</li>
          <li>15%</li>
        </ul> -->
      </div>
    </div>
    <div class="mc-footer">欺诈损失率<span>{{footerData}}</span></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      footerdata:'',
      attcktimes:'',
      showAttack:false,
    };
  },
  computed: {
    footerData() {
      return this.footerdata
    },
    attckTimes() {
      return this.attcktimes
    },
  },
  mounted () {
    this.getLossRate()
    this.getAttackRecent()
    this.drawChart()
  },
  methods: {
    getLossRate () {
      //欺诈损失率
      this.axios.get('/api/p3/lossRate')
      .then( (response) => {
        this.footerdata = response.data.data
      })
      .catch(function (error) {
          console.log(error);
      });
    },
    getAttackRecent () {
      //近一小时攻击数P3-4
      this.axios.get('/api/p3/attackRecent')
      .then( (response) => {
        this.attcktimes = response.data.data
      })
      .catch(function (error) {
          console.log(error);
      });
    },
    drawChart() {
      let myChart = this.$echarts.init(document.getElementById('frimChart'));
      // 绘制图表
      myChart.setOption({
        color: [
            {
                type: 'linear',
                x: 0,
                y: 0,
                x2: 1,
                y2: 0,
                colorStops: [{
                    offset: 0, color: '#1F7CFF' // 0% 处的颜色
                }, {
                    offset: 0.9, color: '#2BF1FF' 
                }, {
                    offset: 1, color: '#FFFFFF ' // 100% 处的颜色
                }],
                global: false // 缺省为 false
            }
        ],
        tooltip: {
            trigger: 'axis',
            axisPointer: {
                type: 'shadow'
            },
            formatter: function(params) {
            return params[0].name + '<br/>' +
                "<span style='display:inline-block;margin-right:5px;border-radius:10px;width:9px;height:9px;background-color:rgba(36,207,233,0.9)'></span>" +
                params[0].seriesName + ' : ' + Number((params[0].value.toFixed(4) / 10000).toFixed(2)).toLocaleString() + ' 万元<br/>'
            }
        },
        grid: {
          top: "10%",
          left: "10%",
          right: "10%",
          bottom: "10%",
          containLabel: true
        },
        xAxis: {
            show: false,
            type: 'value',
            boundaryGap: [0, 0.01],
            axisLabel: {
              color: 'rgba(255,255,255)'
            },
            splitLine: {
              show: false
            }
        },
        yAxis:[
          {
            type: 'category',
            show: false
          },
        ],
        series: [
            {
              name: '拦截金融',
              type: 'bar',
              zlevel: 1,
              itemStyle: {
                // barBorderRadius: 6,
                color:  {
                  type: 'linear',
                  x: 0,
                  y: 0,
                  x2: 1,
                  y2: 0,
                  colorStops: [{
                      offset: 0, color: '#1F7CFF' // 0% 处的颜色
                  }, {
                      offset: 0.9, color: '#2BF1FF' 
                  }, {
                      offset: 1, color: '#FFFFFF ' // 100% 处的颜色
                  }],
                  global: false // 缺省为 false
                }
              },
              label: {
                show: true,
                position: 'right',
                color: 'rgba(255,255,255)',
                formatter: function(data){
                  return data.value + '%'
                }
              },
              barWidth: 30,
              data: [15, 10, 60, 5]
            },
            {
              name: '背景',
              type: 'bar',
              barWidth: 30,
              barGap: '-100%',
              data: [100, 100, 100, 100],
              itemStyle: {
                color: 'rgba(255,255,255,0)',
                // barBorderRadius: 30,
                borderWidth: 1,
                borderColor: 'rgba(255,255,255,.4)'
              },
            }
        ]
      });
    }
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
      margin-top: 55px;
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
      height: 450px;
      background: url('../../assets/images/p3/frm-c.png') no-repeat;
      background-size: 100% 100%;
      margin: 0 46px 0 27px;
    }
    .mc-r{
      margin-top: 15px;
      width: 160px;
      height: 410px;
      background: url('../../assets/images/p3/frm-r.png') no-repeat;
      padding: 12px 0;
      text-align: center;
      position: relative;
      .title{
        font-size: 18px;
        color: rgba(255,255,255,.7);
        margin-bottom: 10px;
      }
      .frim-chart{
        position: absolute;
        top: -25px;
        left: 15px;
        width: 140px;
        height: 480px;
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
