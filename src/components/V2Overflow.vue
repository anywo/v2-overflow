<template>
  <ResizeObserver
    class="v2-overflow"
    ref="box"
    limiter="debounce"
    :wait="100"
    @resize="handleResize"
  >
    <template v-for="(node, i) in nodesOfNeedsShow">
      <component
        :style="{ marginRight: `${gap}px` }"
        ref="components"
        :is="node"
        :key="i"
      ></component>
    </template>
    <div class="other" ref="other">
      <slot name="other" :startIndex="otherStartIndex" />
    </div>
  </ResizeObserver>
</template>

<script>
import ResizeObserver from "v-resize-observer";

export default {
  name: "V2Overflow",
  props: {
    gap: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      isMounted: false,
      otherStartIndex: -1,
      isComputing: false,
    };
  },
  computed: {
    els() {
      if (this.isMounted) {
        return this.$slots.default.map((x) => x.elm);
      }
      return [];
    },
    nodesOfNeedsShow() {
      let res;
      if (this.otherStartIndex == -1) {
        res = [...this.$slots.default];
      } else {
        res = this.$slots.default.slice(0, this.otherStartIndex);
      }
      return res.map((x) => ({
        render(h) {
          return x;
        },
      }));
    },
  },
  mounted() {
    window.test = this;
    this.isMounted = true;
  },
  methods: {
    handleResize() {
      if (this.isComputing) return; // 重新计算需要恢复全部显示
      this.isComputing = true;
      this.otherStartIndex = -1;
      this.$nextTick(() => {
        const box = this.$refs.box.$el;
        const mainW = box.offsetWidth;
        let maxW = 0;
        // 计算出需要隐藏的元素的开始index
        let otherStartIndex = this.$slots.default.findIndex((x) => {
          maxW += x.elm.offsetWidth;
          if (maxW > mainW) {
            return true;
          } else {
            maxW += this.gap;
            return false;
          }
        });
        // 非全部显示时，空出other的位置
        if (otherStartIndex != -1) {
          const other = this.$refs.other;
          let otherW = mainW - maxW;
          for (otherStartIndex; otherStartIndex > 0; otherStartIndex--) {
            const node = this.$slots.default[otherStartIndex];
            otherW += node.elm.offsetWidth;
            if (otherW > other.offsetWidth) break;
            else otherW += this.gap;
          }
        }
        this.otherStartIndex = otherStartIndex;
        this.isComputing = false;
      });
    },
  },
  components: {
    ResizeObserver: ResizeObserver.component,
  },
};
</script>

<style scoped>
.v2-overflow {
  white-space: nowrap;
  text-align: left;
  overflow: hidden;
}

.v2-overflow > .other {
  display: inline-block;
}
</style>
