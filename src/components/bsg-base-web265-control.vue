<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-09 16:50:33
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-10 10:57:48
 * @FilePath: \test-com\src\components\bsg-base-web265-control.vue
 * @Description: 控制面板组件
 * 
-->

<template>
  <div class="bsg-base-web265-control">
    <div class="base_web265_control">
      <!-- S 操作按钮 -->
      <div class="base_web265_control_button">
        <div class="web265_control_button_wrap" @click="handlePlay">
          <img class="web265_control_button" src="./assets/play.svg" alt="" />
        </div>
        <div class="web265_control_button_wrap" @click="handlePause">
          <img class="web265_control_button" src="./assets/pause.svg" alt="" />
        </div>
        <div class="web265_control_button_wrap" @click="handlePrevious">
          <img
            class="web265_control_button"
            src="./assets/previous.svg"
            alt=""
          />
        </div>
        <div class="web265_control_button_wrap" @click="handleNext">
          <img class="web265_control_button" src="./assets/next.svg" alt="" />
        </div>
      </div>
      <!-- E 操作按钮 -->
      <!-- S 播放时间 -->
      <div class="base_web265_control_time">
        {{ curPlayTime }} / {{ curTotalTime }}
      </div>
      <!-- E 播放时间 -->
    </div>
  </div>
</template>
<script>
export default {
  name: "BsgBaseWeb265Control",
  props: {
    totalTime: {
      type: [String, Number],
    },
  },
  components: {},
  data() {
    return {
      curPlayTime: "00:00:00",
      curTotalTime: "00:00:00",
    };
  },
  watch: {
    totalTime(v) {
      console.log("totalTime", v);
      this.curTotalTime = v;
    },
  },
  methods: {
    /**
     * @Author: canlong.shen
     * @description: 播放
     * @default:
     * @return {*}
     */
    handlePlay() {
      console.log("播放");
      this.$emit("on-play");
    },
    /**
     * @Author: canlong.shen
     * @description: 暂停
     * @default:
     * @return {*}
     */
    handlePause() {
      console.log("暂停");
      this.$emit("on-pause");
    },
    /**
     * @Author: canlong.shen
     * @description: 前进
     * @default:
     * @return {*}
     */
    handlePrevious() {
      console.log("快进");
      this.$emit("on-previous");
    },
    /**
     * @Author: canlong.shen
     * @description: 后退
     * @default:
     * @return {*}
     */
    handleNext() {
      console.log("快退");
      this.$emit("on-next");
    },
  },

  mounted() {},
};
</script>
<style  scoped>
/* 自定义样式
---------------------------------------------------------------- */
.bsg-base-web265-control {
  height: 100%;
  width: 100%;
}
.base_web265_control {
  height: inherit;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.base_web265_control_button {
  height: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.web265_control_button_wrap {
  height: inherit;
  display: flex;
  padding: 0 10px;
  justify-content: center;
  align-items: center;
  border-right: solid 1px black;
}
.web265_control_button_wrap:hover {
  box-shadow: inset 0 0 100px rgba(255, 255, 255, 0.5);
}
.web265_control_button {
  height: 15px;
  margin: 0 10px;
}

.base_web265_control_time {
  margin: 0 10px;
  color: #f6f6ff;
}
</style>
<style >
/* 覆盖样式
---------------------------------------------------------------- */
</style>