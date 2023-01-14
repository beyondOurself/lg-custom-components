<template>
  <div id="app">
    <!-- <div class="container">
      <div class="list"></div>
      <div class="list"></div>
      <div class="list"></div>
      <div class="list"></div>
      <div class="list"></div>
      <div class="list"></div>
      <div class="list"></div>
      <i></i><i></i><i></i><i></i><i></i>
    </div> -->
    <BsgBaseLayout
      style="width: 500px; height: 1000px; background-color: red"
      width="200"
      height="200"
    >
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
      <div class="item"></div>
    </BsgBaseLayout>
  </div>
</template>

<script>
// import BsgBaseProgress from "./components/bsg-base-progress.vue";
// import BsgBaseWeb265 from "./components/bsg-base-web265.vue";
// import BsgBaseWeb265Loading  from "./components/bsg-base-web265-loading"
import BsgBaseLayout from "./components/bsg-base-layout";
export default {
  name: "App",
  components: {
    // BsgBaseProgress,
    // BsgBaseWeb265,
    // BsgBaseWeb265Loading
    BsgBaseLayout,
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
  mounted() {
    // this.$refs.BSG_BASE_WEB265_EL.initPlayer()
    // this.handlePlay();
  },
  methods: {
    handlePlay() {
      console.log("播放了");
      this.$refs.BSG_BASE_WEB265_EL.initPlayer();
    },
  },
};
</script>

<style >
</style>
