
<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-04 15:30:26
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-11 09:49:00
 * @FilePath: \test-com\src\components\bsg-base-web265.vue
 * @Description: 
 * 
-->

<template>
  <div class="bsg-base-web265">
    <div
      v-show="curDialogVisible"
      class="base_web265_wrap"
      @click.self="handleDialogMask"
    >
      <div class="base_web265_wrap_container">
        <!-- S 头部操作面板 -->
        <div class="base_web26_header">
          <!-- S 视频信息 -->
          <slot name="name">
            <div class="base_web26_header_name">
              <span>{{ curName }}</span>
            </div>
          </slot>
          <!-- E 视频信息 -->

          <!-- S 头部操作按钮面板 -->
          <div class="base_web26_header_button">
            <!-- S 全屏按钮 -->
            <img
              class="web26_header_button_item"
              style="height: 20px; width: 20px"
              src="./assets/full.svg"
              @click="handleFullIcon"
            />
            <!-- E 全屏按钮 -->
            <!-- / 关闭按钮 -->
            <img
              class="web26_header_button_item"
              style="height: 20px; width: 20px"
              src="./assets/close.svg"
              @click="handleCloseIcon"
            />
            <!-- / 关闭按钮 -->
          </div>

          <!-- E 头部操作按钮面板 -->
        </div>
        <!-- E 头部操作面板 -->
        <!-- S 播放内容 -->
        <div id="glplayer" class="glplayer">
          <!-- / loading 动态效果 -->
          <BsgBaseWeb265Loading :loading="curLoading" />
          <!-- / loading 动态效果 -->

          <!-- / 数值显示 -->
          <div class="base_web26_container_hint" v-if="curHint">
            {{ curHint }}
          </div>

          <!-- / 数值显示 -->
        </div>
        <!-- E 播放内容 -->
        <!-- S 底部操作面板 -->
        <div class="base_web26_footer">
          <!-- S 进度控制条 -->
          <div class="base_web26_footer_audio">
            <!-- / 进度条 -->
            <div class="web26_footer_audio_progress">
              <BsgBaseProgress
                flag="progress"
                ref="BASE_PROGRESS_PLAY_EL"
                :sliderWidth="8"
                :progressHeight="2"
                :cacheScale="curCacheScale"
                :progressScale="curProgressScale"
                @on-up="triggerProgressUp"
              />
            </div>
            <!-- / 进度条 -->
            <!-- / 音量 -->
            <div class="web26_footer_audio_volume">
              <BsgBaseWeb265Volume @on-change="changeVolume" />
            </div>
            <!-- / 音量 -->
          </div>
          <!-- E 进度控制条 -->
          <!-- / 分隔条 -->
          <div class="base_web26_footer_gap"></div>
          <!-- / 分隔条 -->
          <!-- / 视频控制按钮 -->
          <div class="base_web26_footer_control">
            <BsgBaseWeb265Control
              @on-play="play"
              @on-pause="pause"
              @on-previous="triggerPrevious"
              @on-next="triggerNext"
              :totalTime="totalTimeGet"
              :playTime="playingTimeGet"
            />
          </div>
          <!-- / 视频控制按钮 -->
        </div>
        <!-- E 底部操作面板 -->
      </div>
    </div>
  </div>
</template>

<script>
import { Player } from "./assets/executor";
import BsgBaseWeb265Volume from "./bsg-base-web265-volume";
import BsgBaseProgress from "./bsg-base-progress";
import BsgBaseWeb265Control from "./bsg-base-web265-control";
import BsgBaseWeb265Loading from "./bsg-base-web265-loading";

export default {
  name: "BsgBaseWeb265",
  model: {
    prop: "visible",
    event: "change",
  },
  props: {
    /**
     * 显示
     */
    visible: {
      type: Boolean,
      default: false,
    },
    /**
     *  视频地址的 URl
     */
    url: {
      type: String,
      default: "",
    },
    /**
     * 视频名称
     */
    name: {
      type: String,
      default: "",
    },
  },

  components: {
    BsgBaseWeb265Volume,
    BsgBaseProgress,
    BsgBaseWeb265Control,
    BsgBaseWeb265Loading,
  },
  data() {
    return {
      curUrl:
        "http://bsgoalsmartcloud.oss-cn-shenzhen.aliyuncs.com/AccessControl/7ebac135579e4892b5e7daa8b6f8b0fa.mp4",
      curInstance: null,
      curDialogVisible: this.visible,
      curPlayer: null,
      curPlaying: false,
      curName: this.name, //视频点名称
      curDuration: 0, // 视频时常
      curCacheScale: 0, // 缓存器进度条
      curProgressScale: 0, // 播放器进度条
      curPlayingTime: 0, //正在播放时间
      curLoading: false, // 缓存加载的loading
      curHint: "", // 提示内容
      curHintTimeout: null, //提示内容定时器
    };
  },
  computed: {
    /**
     * 播放状态
     */
    playingStatusGet() {
      return !this.curPlaying;
    },
    durationSecondGet() {
      const { convertInt, curDuration } = this;
      return convertInt(curDuration);
    },
    /**
     * 视频总时常
     */
    totalTimeGet() {
      const { durationSecondGet = 0 } = this;
      return this.transTime(durationSecondGet);
    },
    /**
     * 当前播放时间
     */
    playingTimeGet() {
      const {
        curProgressScale = 0,
        curPlayingTime = 0,
        totalTimeGet = "",
        transTime,
      } = this;
      return curProgressScale === 1 ? totalTimeGet : transTime(curPlayingTime);
    },
  },
  watch: {
    visible(v) {
      this.curDialogVisible = v;
    },
    name(v) {
      this.curName = v;
    },
    curHint(v) {
      if (v) {
        const { curHintTimeout: hintTimeout } = this;
        if (hintTimeout) {
          clearTimeout(hintTimeout);
        }
        this.curHintTimeout = setTimeout(() => {
          this.curHint = "";
        }, 3000);
      }
    },
  },
  created() {
    this.init();
  },
  mounted() {},
  beforeDestroy() {
    this.closeDialog();
  },
  methods: {
    /**
     * @Author: canlong.shen
     * @description: 调整 seek 节点
     * @default:
     * @return {*}
     */
    adjustSeekPoint(offset = 0) {
      const {
        curInstance: playerObj,
        curPlayingTime: playingTime,
        durationSecondGet,
      } = this;
      let playSecond = playingTime + offset;

      if (playSecond <= 0) {
        playSecond = 0;
      }

      if (playSecond >= durationSecondGet) {
        playSecond = durationSecondGet;
      }
      this.curLoading = true;
      playerObj.seek(playSecond);
    },
    /**
     * @Author: canlong.shen
     * @description: 快退
     * @default:
     * @return {*}
     */
    triggerPrevious() {
      this.adjustSeekPoint(-10);
    },
    /**
     * @Author: canlong.shen
     * @description: 快进
     * @default:
     * @return {*}
     */
    triggerNext() {
      this.adjustSeekPoint(10);
    },
    /**
     * @Author: canlong.shen
     * @description: 转为整数
     * @default:
     * @return {*}
     */
    convertInt(v = 0) {
      return parseInt(v / 1000, 10);
    },
    /**
     * @Author: canlong.shen
     * @description:
     * @default:
     * @return {*}
     */
    transTime(secondDur = 0) {
      const oriHour = secondDur / (60 * 60);
      const hour = parseInt(oriHour, 10);
      const oriMinute = (oriHour % 1) * 60;
      const minute = parseInt(oriMinute, 10);
      const second = parseInt((oriMinute % 1) * 60, 10);
      return `${hour ? (hour < 10 ? `0${hour}` : hour) : "00"}:${
        minute ? (minute < 10 ? `0${minute}` : minute) : "00"
      }:${second ? (second < 10 ? `0${second}` : second) : "00"}`;
    },
    /**
     * @Author: canlong.shen
     * @description: 调节视频进度
     * @default:
     * @return {*}
     */
    triggerProgressUp(v = 0, flag = "") {
      const { durationSecondGet: durationSecond = 0, curInstance: playerObj } =
        this;
      const scale = v ? v / 100 : 0;
      const playSecond = durationSecond * scale;
      if (playerObj) {
        this.curLoading = true;
        playerObj.seek(playSecond);
      }

      console.log("triggerProgressUp", flag);
    },
    /**
     * @Author: canlong.shen
     * @description: 调节音量
     * @default:
     * @return {*}
     */
    changeVolume(v) {
      const playerObj = this.curInstance;
      const volumeNum = v / 100;
      if (playerObj) {
        playerObj.setVoice(volumeNum);
        this.curHint = `音量${v}%`;
      }
      console.log("音量", volumeNum);
    },
    /**
     * @Author: canlong.shen
     * @description: 全屏
     * @default:
     * @return {*}
     */
    handleFullIcon() {
      this.curInstance.fullScreen();
    },

    /**
     * @Author: canlong.shen
     * @description: 关闭 窗口
     * @default:
     * @return {*}
     */
    handleCloseIcon() {
      this.closeDialog();
    },
    /**
     * @Author: canlong.shen
     * @description: 初始化播放器的事件
     * @default:
     * @return {*}
     */
    initPlayerEvent() {
      const playerObj = this.curInstance;
      // Seek完成
      playerObj.onSeekFinish = () => {
        console.log("onSeekFinish", "Seek完成");
        // 播放结束重新播放
        this.play();
        this.$refs.BASE_PROGRESS_PLAY_EL.setSeekFinish(true);
        this.curLoading = false;
      };
      // YUV帧数据渲染
      playerObj.onRender = (param) => {
        console.log("onRender ", "YUV帧数据渲染", param);
      };
      // 媒体文件加载完成事件
      // 媒体文件当前加载成功，可以进行播放
      playerObj.onLoadFinish = () => {
        console.log("onLoadFinish ", "媒体文件加载完成事件");
        this.play();
        this.curPlaying = true;
      };
      // 播放器当前播放PTS时刻更新
      playerObj.onPlayTime = (pts) => {
        // console.log("onPlayTime ", "播放器当前播放PTS时刻更新", pts);
        const playTotalSecond = this.durationSecondGet;
        if (pts >= playTotalSecond) {
          this.curProgressScale = 1;
        } else {
          this.curProgressScale = pts / playTotalSecond;
        }
        this.curPlayingTime = pts;
      };
      // 播放器媒体播放结束事件
      playerObj.onPlayFinish = () => {
        console.log("onPlayFinish ", "播放器媒体播放结束事件");
      };
      // 播放器缓冲进度回调
      playerObj.onCacheProcess = (cPts) => {
        // 当前缓冲进度时间
        const playTotalSecond = this.durationSecondGet;

        if (cPts >= playTotalSecond) {
          this.curCacheScale = 1;
        } else {
          this.curCacheScale = cPts / playTotalSecond;
        }

        // console.log("onCacheProcess", "播放器缓冲进度回调", cPts);
      };

      // 播放器封面图加载完成
      playerObj.onReadyShowDone = () => {
        console.log("onReadyShowDone ", "播放器封面图加载完成");
        this.getMediaInfo();
      };

      // 当前正在缓存帧数据
      playerObj.onLoadCache = () => {
        console.log("onLoadCache  ", "当前正在缓存帧数据");
      };

      // 帧数据缓存完成
      playerObj.onLoadCacheFinshed = () => {
        console.log("onLoadCacheFinshed ", "帧数据缓存完成");
      };

      // 开启全屏事件
      playerObj.onOpenFullScreen = () => {
        console.log("onOpenFullScreen ", "开启全屏事件");
      };

      // 关闭全屏事件
      playerObj.onCloseFullScreen = () => {
        console.log("onCloseFullScreen ", "关闭全屏事件");
      };
      // 播放器播放状态
      playerObj.onPlayState = (state) => {
        console.log("onPlayState ", "播放器播放状态", state);
      };
    },
    /**
     * @Author: canlong.shen
     * @description: 设置 visible 的值
     * @default:
     * @return {*}
     */
    setVisibleValue() {
      // 释放点资源
      if (this.curInstance) {
        this.curInstance.release();
      }
      this.curDialogVisible = false;
      this.$emit("change", false);
    },
    /**
     * @Author: canlong.shen
     * @description: 点击了遮罩
     * @default:
     * @return {*}
     */
    handleDialogMask() {
      this.closeDialog();
    },
    /**
     * @Author: canlong.shen
     * @description: 初始化视频
     * @default:
     * @return {*}
     */
    initPlayer(url = "") {
      console.log("initPlayer", url);
      this.curDialogVisible = true;
      this.$nextTick(() => {
        const backVideoUrl = url || this.curUrl;
        // 初始化事件
        this.curPlayer.init(backVideoUrl);
        this.curInstance = this.curPlayer.instance;
        this.initPlayerEvent();
        this.curInstance.setRenderScreen(true);
        this.curPlayer.instance.do();
      });
    },
    /**
     * @Author: canlong.shen
     * @description: 关闭弹窗
     * @default:
     * @return {*}
     */
    closeDialog() {
      this.setVisibleValue();
    },
    /**
     * @Author: canlong.shen
     * @description: 播放
     * @default:
     * @return {*}
     */
    play() {
      const playerObj = this.curInstance;
      // 开始播放
      if (playerObj.isPlaying()) {
        // 正在播放中
        console.log("正在播放中");
      } else {
        // 当前是暂停状态
        console.log("当前是暂停状态");
        playerObj.play();
      }
    },
    /**
     * @Author: canlong.shen
     * @description: 暂停
     * @default:
     * @return {*}
     */
    pause() {
      const playerObj = this.curInstance;
      console.log("触发暂停了");
      console.log("playerObj.isPlaying()", playerObj.isPlaying());
      this.curInstance.pause();
    },

    /**
     * @Author: canlong.shen
     * @description: 获取媒体信息
     * @default:
     * @return {*}
     */
    getMediaInfo() {
      //  meta:
      // 	audioNone: false // 是否不包含音频轨
      // 	durationMs: 600000 // 时长 毫秒级
      // 	fps: 25 // 帧率
      // 	sampleRate: 44100 // 音频采样率
      // 	size: // 视频分辨率
      // 	height: 720
      // 	width: 1280
      // 	videoCodec: 0 // 0:HEVC/H.265 1:其他编码
      // 	isHEVC: true // 是否是H265编码视频
      // videoType: "vod" // 点播vod 直播live
      const { meta: { durationMs } = {} } = this.curInstance.mediaInfo();
      this.curDuration = durationMs;
      console.log("info", this.curDuration);
    },
    /**
     * @Author: canlong.shen
     * @description: 初始化
     * @default:
     * @return {*}
     */
    init() {
      this.curPlayer = new Player();
    },
  },
};
</script>

<style scoped>
.base_web265_wrap {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 2;

  display: flex;
  align-items: center;
  justify-content: center;

  background-color: rgba(0, 0, 0, 0.3);
}

.base_web265_wrap_container {
  border: solid 1px black;
  background-color: black;
}

.base_web26_header {
  width: 100%;
  height: 40px;
  z-index: 1;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  outline: solid 1px black;
  background-color: rgba(66, 66, 66, 1);
}

.base_web26_header_button {
  height: 100%;
  display: flex;
  border-left: solid 1px black;
  align-items: center;
}

.base_web26_header_close {
  /* position: absolute;
  right: 0;
  margin-right: 10px; */
}
.base_web26_header_name {
  color: #cdcdcd;
  margin-left: 10px;
}
.web26_header_button_item {
  margin: 0 10px;
}

.base_web26_footer {
  width: 100%;
  height: 80px;
  display: flex;
  flex-direction: column;
  border-top: solid 1px black;
  background: rgba(66, 66, 66, 1);
}
.base_web26_footer_gap {
  flex-basis: 1px;
  background-color: black;
}

.base_web26_footer_audio {
  display: flex;
  flex-basis: 20px;
}
.web26_footer_audio_progress {
  flex: 1;
  padding: 0 5px;
}

.web26_footer_audio_volume {
  flex-basis: 300px;
  display: flex;
  align-items: center;
}
.footer_audio_volume_scale {
  flex: 1;
  margin-right: 10px;
  border-radius: 5px;
}

.footer_audio_volume_scale {
  height: 4px;
  background-color: black;
}

.web26_footer_audio_progress {
  display: flex;
  align-items: center;
}
.base_web26_footer_control {
  flex: 1;
}
.base_web26_container_hint {
  position: absolute;
  top: 10%;
  left: 5%;
  font-weight: bold;
  font-size: 28px;
  color: rgb(136 170 247);
  -webkit-text-stroke: 1px black;
}
</style>
