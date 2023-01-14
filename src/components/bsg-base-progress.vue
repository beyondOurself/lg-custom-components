<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-06 18:10:58
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-11 14:13:22
 * @FilePath: \test-com\src\components\bsg-base-progress.vue
 * @Description: 进度条组件
 * 
-->



<template>
  <div class="bsg-base-progress">
    <div class="base_progress_wrap">
      <div class="base_progress" ref="BASE_PROGRESS_EL" :style="progressStyleGet">
        <!-- S 缓存点 -->
        <div class="base_progress_cache" ref="BASE_PROGRESS_CACHE_EL" :style="cacheStyleGet"></div>
        <!-- E 缓存点 -->

        <!-- S 进展刻度条 -->
        <div class="base_progress_evolve" ref="BASE_PROGRESS_EVOLVE_EL" :style="evolveStyleGet"></div>
        <!-- E 进展刻度条 -->
      </div>

      <!-- S 滑块 -->
      <div class="base_progress_slider" ref="BASE_PROGRESS_SLIDER_EL" :style="dragStyleGet"></div>
      <!-- E 滑块 -->
    </div>
  </div>
</template>
<script>
export default {
  name: 'BsgBaseProgress',
  props: {
    /**
     * 数值
     */
    value: {
      type: [String, Number],
      default: 0,
    },
    /**
     * 滑块的宽度
     */
    sliderWidth: {
      type: Number,
      default: 12,
    },
    /**
     *  进度条的高度
     */
    progressHeight: {
      type: [String, Number],
      default: 4,
    },
    /**
     * 缓存条进度值
     */
    cacheScale: {
      type: [String, Number],
      default: 0,
    },
    /**
     * 播放条进度值
     */
    progressScale: {
      type: [String, Number],
      default: 0,
    },
    /**
     * 是否完成 seek
     */
    seekFinish: {
      type: Boolean,
      default: true,
    },
    /**
     * @Author: canlong.shen
     * @description: 组件标识
     * @default:
     * @return {*}
     */
    flag: {
      type: String,
      default: '',
    },
  },
  model: {
    prop: 'value',
    event: 'change',
  },
  components: {},

  data() {
    return {
      curCacheEl: null, // 缓存进度条
      curEvolveEl: null, // 时刻进度条元素
      curProgressEl: null, // 主题进度条元素
      curSliderEl: null, // 拖拽主体元素
      curIsMove: false, // 是否可拖动
      curDifferTop: 0, // 初始鼠标点 y 值 与 拖拽元素 top值的 差值
      curDifferLeft: 0, // 初始鼠标点 x 值 与 拖拽元素 let值的 差值
      curTop: 0, // 当前的top值
      curCacheScale: this.cacheScale, // 缓存条进度值
      curProgressScale: this.progressScale, // 播放进度值
      curLeft: this.value, // 当前left 值
      curSliderWidth: this.sliderWidth, // 滑块的宽度
      curValue: this.value, // 刻度的值
      curProgressWidth: 0, // 进度条的总体长度
      curSeekFinish: this.seekFinish, // 是否 seek 完成
    }
  },
  computed: {
    dragStyleGet() {
      const style = {
        position: 'absolute',
      }
      const { curLeft, curSliderWidth } = this
      const isVaildLeft = parseInt(curLeft, 10)
      const sliderWidth = parseInt(curSliderWidth, 10)
      if (isVaildLeft) {
        style.left = `${isVaildLeft}px`
        style.right = 'initial'
      }
      if (sliderWidth) {
        style.width = `${sliderWidth}px`
        style.height = `${sliderWidth}px`
      }

      return style
    },
    progressStyleGet() {
      const style = {}
      const { progressHeight } = this
      if (progressHeight) {
        style.height = `${progressHeight}px`
      }

      return style
    },
    evolveStyleGet() {
      const style = {}
      const { curLeft } = this
      if (curLeft) {
        style.width = `${curLeft}px`
      }
      return style
    },
    cacheStyleGet() {
      const style = {}
      const { curCacheScale, curProgressWidth } = this
      if (curCacheScale && curCacheScale <= 1) {
        style.width = `${parseInt(curProgressWidth * curCacheScale, 10)}px`
      }
      // console.log("cacheStyleGet", style);
      return style
    },
  },
  watch: {
    cacheScale(v) {
      this.curCacheScale = v
    },
    progressScale(scale) {
      const { curProgressWidth, curIsMove, curSeekFinish } = this
      if (scale && scale <= 1 && !curIsMove && curSeekFinish) {
        this.curLeft = `${parseInt(curProgressWidth * scale, 10)}`
      }
      this.curProgressScale = scale
    },
    sliderWidth(v) {
      this.curSliderWidth = v
    },
    seekFinish(v) {
      this.curSeekFinish = v
    },
  },
  methods: {
    /**
     * @Author: canlong.shen
     * @description: 设置是否完成seek
     * @default:
     * @return {*}
     */
    setSeekFinish(v) {
      this.curSeekFinish = v
    },
    /**
     * @Author: canlong.shen
     * @description:  移除事件
     * @default:
     * @return {*}
     */
    releaseEvent() {
      const evolveEl = this.curSliderEl
      const { mousedownEvent, mouseupEvent, mousemoveEventWindow } = this
      evolveEl.removeEventListener('mousedown', mousedownEvent, {
        capture: true,
        passive: true,
      })
      window.removeEventListener('mousemove', mousemoveEventWindow, {
        capture: true,
        passive: true,
      })
      evolveEl.removeEventListener('mouseup', mouseupEvent, {
        capture: true,
        passive: true,
      })
    },

    /**
     * @Author: canlong.shen
     * @description: 添加事件
     * @default:
     * @return {*}
     */
    addEvent() {
      this.$nextTick(() => {
        const evolveEl = this.$refs.BASE_PROGRESS_SLIDER_EL
        this.curSliderEl = evolveEl
        const { mousedownEvent, mouseupEvent, resizeEventWindow } = this
        evolveEl.addEventListener('mousedown', mousedownEvent, {
          capture: true,
          passive: true,
        })

        evolveEl.addEventListener('mouseup', mouseupEvent, {
          capture: true,
          passive: true,
        })

        window.addEventListener('resize', resizeEventWindow, {
          capture: true,
          passive: true,
        })
      })
    },

    /**
     * @Author: canlong.shen
     * @description: 视口发送变动了
     * @default:
     * @return {*}
     */
    resizeEventWindow() {
      // this.setProgressWidth();
    },
    /**
     * @Author: canlong.shen
     * @description: 设置滚动条的长度
     * @default:
     * @return {*}
     */
    setProgressWidth() {
      this.$nextTick(() => {
        const { width } = this.curProgressEl.getBoundingClientRect()
        this.curProgressWidth = width
        console.log('  this.curProgressWidth', this.curProgressWidth)
      })
    },

    /**
     * @Author: canlong.shen
     * @description: 鼠标按下事件
     * @param {*} e
     * @return {*}
     */
    mousedownEvent(e) {
      const { mousemoveEventWindow, mouseupEventWindow } = this
      this.setProgressWidth()
      this.curSeekFinish = false
      window.addEventListener('mousemove', mousemoveEventWindow, {
        capture: true,
        passive: true,
      })

      window.addEventListener('mouseup', mouseupEventWindow, {
        capture: true,
        passive: true,
      })

      const curEl = this.curSliderEl
      // 点击节点和目标节点一致
      this.curIsMove = true
      const mx = e.pageX || e.clientX
      const my = e.pageY || e.clientY
      const ex = curEl.offsetLeft
      const ey = curEl.offsetTop
      this.curDifferLeft = mx - ex
      this.curDifferTop = my - ey
    },
    /**
     * @Author: canlong.shen
     * @description: 鼠标移动事件
     * @param {*} e
     * @return {*}
     */
    mousemoveEventWindow(e) {
      const isMove = this.curIsMove
      if (isMove) {
        const mx = e.pageX || e.clientX
        const my = e.pageY || e.clientY
        this.curTop = my - this.curDifferTop
        const progressWidth = this.curProgressWidth
        let left = mx - this.curDifferLeft
        const startLeft = 0
        const endLeft = progressWidth
        if (left <= startLeft) {
          left = startLeft
        } else if (left >= endLeft) {
          left = endLeft
        }
        this.curLeft = left
        this.curValue = parseInt((left / progressWidth) * 100)
        this.$emit('change', this.curValue)
        this.$emit('on-change', this.curValue)
      }
    },
    /**
     * @Author: canlong.shen
     * @description: 鼠标松开事件
     * @param {*} e
     * @return {*}
     */
    mouseupEvent() {
      // this.curIsMove = false;
    },
    /**
     * @Author: canlong.shen
     * @description: 鼠标松开事件
     * @param {*} e
     * @return {*}
     */
    mouseupEventWindow() {
      this.curIsMove = false
      const { mousemoveEventWindow, curValue, flag, mouseupEventWindow } = this
      window.removeEventListener('mousemove', mousemoveEventWindow, {
        capture: true,
        passive: true,
      })

      window.removeEventListener('mouseup', mouseupEventWindow, {
        capture: true,
        passive: true,
      })
      this.$emit('on-up', curValue, flag)
    },
    /**
     * @Author: canlong.shen
     * @description: 初始化 目标元素
     * @default:
     * @return {*}
     */
    initEl() {
      this.$nextTick(() => {
        this.curProgressEl = this.$refs.BASE_PROGRESS_EL
        this.curEvolveEl = this.$refs.BASE_PROGRESS_EVOLVE_EL
        this.curCacheEl = this.$refs.BASE_PROGRESS_CACHE_EL
      })
    },
    /**
     * @Author: canlong.shen
     * @description: 初始化
     * @default:
     * @return {*}
     */
    init() {
      this.addEvent()
      this.initEl()
      this.setProgressWidth()
    },
  },
  created() {
    this.init()
  },
  mounted() {},
  beforeDestroy() {
    this.releaseEvent()
  },
}
</script>
<style scoped>
/* 自定义样式
---------------------------------------------------------------- */
.bsg-base-progress {
  width: 100%;
}
.base_progress_wrap {
  display: flex;
  position: relative;
  align-items: center;
}

.base_progress {
  width: 100%;
  position: relative;
  border-radius: 10px;
  background-color: rgba(212, 205, 205, 1);
}

.base_progress_slider {
  position: absolute;
  border-radius: 50%;
  /* cursor: move;
  cursor: grabbing; */
  /* border: 10px solid transparent;
  background-clip: content-box ; */
  background-color: rgb(255, 250, 250);
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.5);
}
.base_progress_cache {
  height: inherit;
  position: absolute;
  border-radius: inherit;
  /* background-color: rgba(230, 228, 228, 1); */
  background-color: rgb(24, 236, 95);
}

.base_progress_evolve {
  height: inherit;
  position: absolute;
  border-radius: 10px;
  background-color: rgba(250, 225, 0, 1);
}
</style>
<style >
/* 覆盖样式
---------------------------------------------------------------- */
</style>