<template>
<a-layout class="layout" id="components-layout-demo-side" style="min-height: 100vh">
  <a-layout-sider class="left-nav" v-if="showNav">
    <LaftNav />
  </a-layout-sider>
  <a-layout class="content">
    <a-layout-content>
        <transition>
          <keep-alive>
            <router-view></router-view>
          </keep-alive>
        </transition>
    </a-layout-content>
    <!-- <a-layout-footer style="text-align: center">
        Ant Design ©2018 Created by Ant UED
      </a-layout-footer> -->
  </a-layout>
</a-layout>
</template>

<script>
import LaftNav from './laft-nav.vue'
export default {
  data() {
    return {
      collapsed: false,
      showNav: false // false --> closed leftNav
    };
  },
  mounted() {
    this.keydown = document.addEventListener('keydown',(event) => {
        let e = event || window.event || arguments.callee.caller.arguments[0];
        e.preventDefault();
        let l = this._.split(window.location.href,'manage/P',2),targetUrl = 0;
        if (e && e.keyCode == 13 && window.location.href.indexOf('P4') > 0) {
          if (!this.showAudio) {
            return;
          }
          this.$nextTick(() => {
            this.showAudio = false;
          });
          this.DrawVideo();
        }else if(e && e.keyCode == 38) {
          if(l[1] == '1') {
            targetUrl =  l[0] + 'manage/P4';
          }else {
            targetUrl = l[0] + 'manage/P' + (Number(l[1]) - 1)
          }
          window.open(targetUrl,"_self")
        }else if(e && e.keyCode == 40) {
          if(l[1] == '4') {
            targetUrl =  l[0] + 'manage/P1';
          }else {
            targetUrl = l[0] + 'manage/P' + (Number(l[1]) + 1)
          }
          window.open(targetUrl,"_self")
        }
      })
  },
  components: {
    LaftNav
  }
};
</script>

<style lang="less">
.layout {
  
  .left-nav {
    // position: fixed;
    // height: 100%;
    // overflow: auto;
    .ant-layout-sider-trigger{
      // display: none;
    }
  }
  .content{
    background: #f1f0ef;
  }
}
</style>
