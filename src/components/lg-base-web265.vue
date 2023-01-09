
<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-04 15:30:26
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-07 10:50:28
 * @FilePath: \test-com\src\components\lg-base-web265.vue
 * @Description: 
 * 
-->

<template>
  <div class="bsg-base-web265">
    <div v-show="curDialogVisible" class="base_web265_wrap" @click.self="handleDialogMask">
      <div class="base_web265_wrap_container">
        <!-- S 头部操作面板 -->
        <div class="base_web26_header">
          <!-- S 视频信息 -->
          <slot name="name">
            <div class="base_web26_header_name">
              <span>{{ curName }}</span>
              <el-button @click="handleTest"></el-button>
            </div>
          </slot>
          <!-- E 视频信息 -->
          <!-- / 关闭按钮 -->
          <img class="base_web26_header_close" style="height: 15px; width: 15px" src="./assets/close.svg" @click="handleCloseIcon" />
          <!-- / 关闭按钮 -->
        </div>
        <!-- E 头部操作面板 -->
        <!-- S 播放内容 -->
        <div
          v-loading="playingStatusGet"
          element-loading-text="拼命加载中"
          element-loading-spinner="el-icon-loading"
          element-loading-background="rgba(0, 0, 0, 0.8)"
          id="glplayer"
          class="glplayer"
        ></div>
        <!-- E 播放内容 -->
        <!-- S 底部操作面板 -->
        <div class="base_web26_footer">
          <!-- S 音频控制条 -->
          <div class="base_web26_footer_audio">
            <!-- / 进度条 -->
            <div class="web26_footer_audio_progress"></div>
            <!-- / 进度条 -->
            <!-- / 音量 -->
            <div class="web26_footer_audio_volume">
                 <BsgBaseWeb265Volume />
            </div>
            <!-- / 音量 -->
          </div>
          <!-- E 音频控制条 -->
          <!-- / 分隔条 -->
          <div class="base_web26_footer_gap"></div>
          <!-- / 分隔条 -->
          <!-- / 视频控制按钮 -->
          <div class="base_web26_footer_control">
            <!-- / 操作按钮 -->

            <!-- / 操作按钮 -->

            <!-- / 播放时间显示 -->
            <div class="web26_footer_control_time"></div>
            <!-- / 播放时间显示 -->
          </div>
          <!-- / 视频控制按钮 -->
        </div>
        <!-- E 底部操作面板 -->
      </div>
    </div>
  </div>
</template>

<script>
import { Player } from './assets/executor'
import  BsgBaseWeb265Volume from './bsg-base-web265-volume'
export default {
  name: 'BsgBaseWeb265',
  model: {
    prop: 'visible',
    event: 'change',
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
      default: '',
    },
    /**
     * 视频名称
     */
    name: {
      type: String,
      default: '',
    },
  },

  components:{
      BsgBaseWeb265Volume
  },
  data() {
    return {
      curUrl: 'http://bsgoalsmartcloud.oss-cn-shenzhen.aliyuncs.com/AccessControl/7ebac135579e4892b5e7daa8b6f8b0fa.mp4',
      curInstance: null,
      curDialogVisible: this.visible,
      curPlayer: null,
      curPlaying: false,
      curName: this.name, //视频点名称
    }
  },
  computed: {
    /**
     * 播放状态
     */
    playingStatusGet() {
      return !this.curPlaying
    },
  },
  watch: {
    visible(v) {
      this.curDialogVisible = v
    },
    name(v) {
      this.curName = v
    },
  },
  created() {
    this.curPlayer = new Player()
  },
  mounted() {},
  beforeDestroy() {
    this.closeDialog()
  },
  methods: {
    /**
     * @Author: canlong.shen
     * @description: 关闭 窗口
     * @default:
     * @return {*}
     */
    handleCloseIcon() {
      this.closeDialog()
    },
    /**
     * @Author: canlong.shen
     * @description: 初始化播放器的事件
     * @default:
     * @return {*}
     */
    initPlayerEvent() {
      const playerObj = this.curInstance
      console.log('初始化事件', playerObj)

      // Seek完成
      playerObj.onSeekFinish = () => {
        console.log('onSeekFinish', 'Seek完成')
        // 播放结束重新播放
        this.play()
      }
      // YUV帧数据渲染
      playerObj.onRender = () => {
        console.log('onRender ', 'YUV帧数据渲染')
      }
      // 媒体文件加载完成事件
      // 媒体文件当前加载成功，可以进行播放
      playerObj.onLoadFinish = () => {
        console.log('onLoadFinish ', '媒体文件加载完成事件')
        this.play()
        this.curPlaying = true
      }
      // 播放器当前播放PTS时刻更新
      playerObj.onPlayTime = () => {
        // console.log('onPlayTime ', '播放器当前播放PTS时刻更新')
      }
      // 播放器媒体播放结束事件
      playerObj.onPlayFinish = () => {
        console.log('onPlayFinish ', '播放器媒体播放结束事件')
      }
      // 播放器缓冲进度回调
      playerObj.onCacheProcess = () => {
        // 当前缓冲进度时间
        // console.log('onCacheProcess', '播放器缓冲进度回调', cPts)
      }

      // 播放器封面图加载完成
      playerObj.onReadyShowDone = () => {
        console.log('onReadyShowDone ', '播放器封面图加载完成')
      }

      // 当前正在缓存帧数据
      playerObj.onLoadCache = () => {
        console.log('onLoadCache  ', '当前正在缓存帧数据')
      }

      // 帧数据缓存完成
      playerObj.onLoadCacheFinshed = () => {
        console.log('onLoadCacheFinshed ', '帧数据缓存完成')
      }

      // 开启全屏事件
      playerObj.onOpenFullScreen = () => {
        console.log('onOpenFullScreen ', '开启全屏事件')
      }

      // 关闭全屏事件
      playerObj.onCloseFullScreen = () => {
        console.log('onCloseFullScreen ', '关闭全屏事件')
      }
      // 播放器播放状态
      playerObj.onPlayState = (state) => {
        console.log('onPlayState ', '播放器播放状态', state)
      }
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
        this.curInstance.release()
      }
      this.curDialogVisible = false
      this.$emit('change', false)
    },
    /**
     * @Author: canlong.shen
     * @description: 点击了遮罩
     * @default:
     * @return {*}
     */
    handleDialogMask() {
      this.closeDialog()
    },
    /**
     * @Author: canlong.shen
     * @description: 初始化视频
     * @default:
     * @return {*}
     */
    initPlayer(url = '') {
      console.log('initPlayer', url)
      this.curDialogVisible = true;
      this.$nextTick(() => {
        const backVideoUrl = url || this.curUrl
        // 初始化事件
        this.curPlayer.init(backVideoUrl)
        this.curInstance = this.curPlayer.instance
        this.initPlayerEvent()
        this.curPlayer.instance.do()
      })
    },
    /**
     * @Author: canlong.shen
     * @description: 关闭弹窗
     * @default:
     * @return {*}
     */
    closeDialog() {
      this.setVisibleValue()
    },
    /**
     * @Author: canlong.shen
     * @description: 播放
     * @default:
     * @return {*}
     */
    play() {
      const playerObj = this.curInstance
      // 开始播放
      if (playerObj.isPlaying()) {
        // 正在播放中
        console.log('正在播放中')
      } else {
        // 当前是暂停状态
        console.log('当前是暂停状态')
        playerObj.play()
      }
    },
    /**
     * @Author: canlong.shen
     * @description: 暂停
     * @default:
     * @return {*}
     */
    pause() {
      this.curInstance.pause()
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
    //   const { durationMs } = this.curInstance.mediaInfo()
    },

    handleTest() {
      console.log('点击了')
      this.play()
    },
  },
}
</script>

<style scoped>
.base_web265_wrap {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 9999999;

  display: flex;
  align-items: center;
  justify-content: center;

  background-color: rgba(0, 0, 0, 0.3);
}

.base_web265_wrap_container {
  position: relative;
}

.base_web26_header {
  position: absolute;
  top: -40px;
  width: 100%;
  height: 40px;
  z-index: 1;
  display: flex;
  align-items: center;
  background-color: rgba(66, 66, 66, 1);
}

.base_web26_header_close {
  position: absolute;
  position: absolute;
  right: 0;
  margin-right: 10px;
}
.base_web26_header_name {
  color: #cdcdcd;
  margin-left: 10px;
}

.base_web26_footer {
  position: absolute;
  bottom: -50px;
  width: 100%;
  height: 50px;
  background: rgba(66, 66, 66, 1);
}
.base_web26_footer_gap {
  height: 1px;
  background-color: black;
}

.base_web26_footer_audio {
  display: flex;
}
.web26_footer_audio_progress {
  flex: 1;
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

.base_web26_footer_control {
  display: flex;
}

.footer_audio_volume_scale {
  height: 4px;
  background-color: black;
}
</style>
