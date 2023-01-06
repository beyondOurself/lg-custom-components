<template>
  <div id="app">
    {{ curValue }} {{ curCacheScale }}
    <HelloWorld v-model="curValue" :cacheScale="curCacheScale" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
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
