<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-11 08:45:20
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-11 09:08:51
 * @FilePath: \test-com\src\components\bsg-base-web265-loading.vue
 * @Description: 视频的 loading 效果
 * 
-->

<template>
  <div class="bsg-base-web265-loading">
    <div class="base_web265_loading" v-show="curLoading"></div>
  </div>
</template>
<script>
export default {
  name: "BsgBaseWeb265Loading",
  props: {
    loading: {
      type: Boolean,
      default: false,
    },
  },
  components: {},
  watch: {
    loading(v) {
      this.curLoading = v;
    },
  },
  data() {
    return {
      curLoading: this.loading,
    };
  },
  methods: {},
  mounted() {},
};
</script>
<style  scoped>
/* 自定义样式
---------------------------------------------------------------- */
.base_web265_loading {
  width: 8px;
  height: 8px;
  border-radius: 100%;
  color: rgba(0, 0, 0, 0.4);
  box-shadow: 0 -20px rgba(0, 0, 0, 0.9), 20px 0px, 0 20px,
    -20px 0 rgba(0, 0, 0, 0.7), -14px -14px rgba(0, 0, 0, 0.8),
    14px -14px rgba(0, 0, 0, 1), 14px 14px, -14px 14px;
  animation: spin 1s steps(8) infinite;

  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
<style >
/* 覆盖样式
---------------------------------------------------------------- */
</style>