<!--
 * @Author: canlong.shen 562172151@qq.com
 * @Date: 2023-01-13 16:49:22
 * @LastEditors: canlong.shen 562172151@qq.com
 * @LastEditTime: 2023-01-14 11:49:24
 * @FilePath: \test-com\src\components\bsg-base-layout.vue
 * @Description:  自适应内容 下拉加载
 * 
-->

<template>
  <div class="bsg-base-layout">
    <div class="base_layout" ref="BASE_LAYOUT_EL" @scroll="scroll($event)">
      <slot></slot>
      <i></i><i></i><i></i><i></i><i></i>
    </div>
  </div>
</template>
<script>
export default {
  name: "BsgBaseLayout",
  props: {
    width: {
      type: [Number, String],
      default: 0,
    },
    height: {
      type: [Number, String],
      default: 0,
    },
  },
  components: {},
  computed: {},
  data() {
    return {};
  },
  created() {
    this.$nextTick(() => {
      const { width, height } = this;
      const layoutEl = this.$refs.BASE_LAYOUT_EL;
      const itemNodeList = layoutEl.querySelectorAll(".base_layout > div");
      const iNodeList = layoutEl.querySelectorAll(".base_layout > i");
      console.log("itemNodeList", itemNodeList);
      if (width && itemNodeList && itemNodeList.length) {
        itemNodeList.forEach((itemnode) => {
          console.log("itemnode", itemnode);
          if (parseInt(width)) {
            itemnode.style.width = `${width}px`;
          } else {
            itemnode.style.width = `${width}`;
          }
          if (parseInt(height)) {
            itemnode.style.height = `${height}px`;
          } else {
            itemnode.style.height = `${height}`;
          }
        });
      }

      iNodeList.forEach((iNode) => {
        if (parseInt(width)) {
          iNode.style.width = `${width}px`;
        } else {
          iNode.style.width = `${width}`;
        }
      });
      console.log("itemNodeList", itemNodeList);
    });
  },
  mounted() {},
  methods: {
    scroll: function () {
      const scrollEl = this.$refs.BASE_LAYOUT_EL;
      if (
        scrollEl.scrollTop + 1 >=
        scrollEl.scrollHeight - scrollEl.clientHeight
      ) {
        alert("滑动到了底部");
      }
    },
  },
};
</script>
<style  scoped>
/* 自定义样式
---------------------------------------------------------------- */
.base_layout {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  height: inherit;
  overflow: auto;
}
.base_layout > div {
  width: 20px;
  height: 20px;
  background-color: skyblue;
}
.base_layout > div {
  margin-bottom: 10px;
}
</style>
<style >
/* 覆盖样式
---------------------------------------------------------------- */
</style>
