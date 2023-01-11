<template>
  <div id="app">
    <!-- S 进度条 -->
    <!-- <BsgBaseProgress v-model="curValue" :cacheScale="curCacheScale" /> -->
    <!-- E 进度条 -->
    <!-- S 播放组件 -->
     <!-- <BsgBaseWeb265Loading /> -->
    <el-button @click="handlePlay">播放</el-button>
    <BsgBaseWeb265 ref="BSG_BASE_WEB265_EL" />
    <!-- E 播放组件 -->
  </div>
</template>

<script>
// import BsgBaseProgress from "./components/bsg-base-progress.vue";
import BsgBaseWeb265 from "./components/bsg-base-web265.vue";
// import BsgBaseWeb265Loading  from "./components/bsg-base-web265-loading"
export default {
  name: "App",
  components: {
    // BsgBaseProgress,
    BsgBaseWeb265,
    // BsgBaseWeb265Loading
  },
  data() {
    return {
      curValue: 10,
      curCacheScale: 0,
    };
  },
  created() {
    // let i = 0;
    // setInterval(() => {
    //   i++;
    //   this.curCacheScale = i / 100;
    //   if (this.curCacheScale === 1) {
    //     i = 0;
    //   }
    // }, 1000);
    
  },
  mounted(){
    // this.$refs.BSG_BASE_WEB265_EL.initPlayer()
    this.handlePlay()
  },
  methods:{
    handlePlay(){

      console.log('播放了');
      this.$refs.BSG_BASE_WEB265_EL.initPlayer()

    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
