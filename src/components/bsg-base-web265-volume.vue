<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-07 10:39:39
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-07 10:51:15
 * @FilePath: \test-com\src\components\bsg-base-web265-volume.vue
 * @Description: 音量控制器组件
 * 
-->

<template>
  <div class="bsg-base-web265-volume">
    <div class="base_web265_volume">
      <div class="base_web265_volume_icon">
        <img
          style="width: 20px; height: 12px"
          src="./assets/volume.svg"
          alt=""
          srcset=""
        />
      </div>
      <div class="base_web265_volume_scale"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "BsgBaseWeb265Volume",
  props: {},
  components: {},
  data() {
    return {};
  },
  methods: {},
  mounted() {},
};
</script>
<style  scoped>
/* 自定义样式
---------------------------------------------------------------- */
</style>
<style >
/* 覆盖样式
---------------------------------------------------------------- */
</style>