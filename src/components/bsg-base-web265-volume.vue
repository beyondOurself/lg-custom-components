<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-07 10:39:39
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-10 16:29:37
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
      <div class="base_web265_volume_scale">
        <BsgBaseProgress2
          flag="volume"
          :sliderWidth="8"
          :progressHeight="2"
          @on-up="triggerVolumeUp"
        />
      </div>
    </div>
  </div>
</template>
<script>
import BsgBaseProgress2 from "./bsg-base-progress.vue";
export default {
  name: "BsgBaseWeb265Volume",
  props: {},
  components: {
    BsgBaseProgress2,
  },
  data() {
    return {};
  },
  methods: {
    /**
     * @Author: canlong.shen
     * @description:
     * @default: 改变进度值
     * @return {*}
     */
    triggerVolumeUp(v,flag) {
      
      this.$emit("on-change", v);
      console.log("triggerVolumeUp",flag);
    },
  },
  mounted() {},
};
</script>
<style  scoped>
/* 自定义样式
---------------------------------------------------------------- */
.bsg-base-web265-volume {
  width: 100%;
}
.base_web265_volume {
  width: inherit;
  display: flex;
  align-items: center;
  margin-right: 10px;
}
.base_web265_volume_icon {
  flex-basis: 20px;
}
.base_web265_volume_scale {
  flex: 1;
  margin-right: 5px;
}
</style>
<style >
/* 覆盖样式
---------------------------------------------------------------- */
</style>