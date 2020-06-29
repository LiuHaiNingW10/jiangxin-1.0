<template>
  <div class="pie-chart" id='pieChart'>
    <div class="pie-item" v-for="(item,index) in data" :key="index">
        <img :src="require(`../../assets/images/p3/pie${index+1}.png`)" alt="">  
        <p class="pie-value">{{item.value}}</p>
        <p class='pie-label'>{{item.label}}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
        data: [
            {
                label: '反欺诈模型决策占比',
                value: 40
            },
            {
                label: '反欺诈策略决策占比',
                value: 60
            },
            {
                label: '反欺诈人工审核占比',
                value: 25
            }
        ]
    };
  },
  mounted () {
    
    
  },
  methods: {
  },
  components: {}
};
</script>

<style lang="less" scoped>
.pie-chart{
  position: absolute;
  left: 0;
  bottom: 30px;
  width: 757px;
  height: 200px;
  background: url('../../assets/images/p3/pie-bg.png') no-repeat;
  display: flex;
  justify-content: space-between;
  padding: 0 40px;
  .pie-item{
    // width: 120px;
    // height: 200px;
    position: relative;
    text-align: center;
    img{
        width: 120px;
        height: 120px;
        margin-top: 20px;
    }
    .pie-value{
        position: absolute;
        top: 65px;
        left: 50%;
        transform: translateX(-50%);
        font-size: 24px;
    }
    .pie-label{
        margin-top: 10px;
        font-size: 20px;
    }
  }
}
</style>
