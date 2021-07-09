<template>
  <view class="f2-chart" :style="customStyle" v-if="canvasId">
    <canvas
      class="f2-chart__canvas"
      v-if="use2dCanvas"
      type="2d"
      :id="canvasId"
      :style="'width:' + width + 'px;height:' + height + 'px'"
      :disable-scroll="isDisableScroll"
      @touchstart="touchStart"
      @touchmove="touchMove"
      @touchend="touchEnd"
    />
    <canvas
      class="f2-chart__canvas"
      v-else
      :width="nodeWidth"
      :height="nodeHeight"
      :style="'width:' + width + 'px;height:' + height + 'px'"
      :canvas-id="canvasId"
      :id="canvasId"
      :disable-scroll="isDisableScroll"
      @touchstart="touchStart"
      @touchmove="touchMove"
      @touchend="touchEnd"
    />
  </view>
</template>
<script>
import extendContext from "./canvas";
import { compareVersion, wrapEvent, pixelRatio } from "./utils";

export default {
  name: "f2-chart",
  props: {
    // #ifdef MP-WEIXIN || MP-TOUTIAO
    type: {
      type: String,
      default: "2d",
    },
    // #endif
    customStyle: String,
    source: {
      type: Array,
      default: () => [],
    },
    isAutoPlay: Boolean,
    isDisableScroll: Boolean,
    onInit: {
      type: [Function, Object],
      default: () => {},
    },
  },
  data() {
    return {
      // #ifdef MP-WEIXIN || MP-TOUTIAO
      use2dCanvas: true,
      // #endif
      // #ifndef MP-WEIXIN || MP-TOUTIAO
      use2dCanvas: false,
      // #endif
      width: null,
      height: null,
      nodeWidth: null,
      nodeHeight: null,
      isInited: false,
      imageData: null,
    };
  },
  computed: {
    canvasId() {
      return `f2-chart${this._uid}`;
    },
  },
  watch: {
    isAutoPlay(val) {
      if (val) {
        this.changeData(this.source);
      }
    },
    source: {
      handler: function (data) {
        if (this.isAutoPlay) {
          this.changeData(data);
        }
      },
      deep: true,
    },
  },
  beforeDestroy() {
    this.clear();
    this.destroy();
  },
  created() {
    this.config = {};
    //this.isMask = this.isCloud && this.imageMask;
    // #ifdef MP-WEIXIN || MP-TOUTIAO
    const { SDKVersion, version, platform, environment } =
      uni.getSystemInfoSync();
    // #endif
    // #ifdef MP-WEIXIN
    this.use2dCanvas =
      this.type === "2d" &&
      compareVersion(SDKVersion, "2.9.2") >= 0 &&
      !(
        (/ios/i.test(platform) && /7.0.20/.test(version)) ||
        /wxwork/i.test(environment)
      ) &&
      !/windows/i.test(platform);
    // #endif
    // #ifdef MP-TOUTIAO
    this.use2dCanvas =
      this.type === "2d" && compareVersion(SDKVersion, "1.78.0") >= 0;
    // #endif
  },

  async mounted() {
    if (this.onInit) {
      this.init(this.onInit);
    }
  },
  methods: {
    async init(func, params = null) {
      let config = await this.getContext(this.canvasId);
      const chart = await func(config);
      if (chart) {
        this.chart = chart;
        this.canvasEl = chart.get("el");
        this.isInited = true;
      }
    },
    changeData(data) {
      if (this.chart) {
        this.chart.changeData(data || this.source);
      }
    },
    clear() {
      if (this.chart) {
        this.chart.clear();
      }
    },
    destroy() {
      if (this.chart) {
        this.chart.destroy();
      }
    },
    repaint() {
      this.changeData(this.source);
    },
    reset(func, params = null) {
      this.$watch("isInited", (v) => v && func(this.chart), {
        immediate: true,
      });
    },
    canvasToTempFilePath(args = {}) {
      const { use2dCanvas, canvasId, config } = this;
      return new Promise((resolve, reject) => {
        const copyArgs = Object.assign(
          {
            canvasId,
            success: resolve,
            fail: reject,
          },
          args
        );
        if (use2dCanvas) {
          let { canvas } = config[canvasId];
          delete copyArgs.canvasId;
          copyArgs.canvas = canvas;
        }
        uni.canvasToTempFilePath(copyArgs, this);
      });
    },
    getContext(canvasId) {
      const { use2dCanvas, type = "2d", config } = this;
      if (config[canvasId]?.context) {
        return Promise.resolve(config[canvasId]);
      }
      if (use2dCanvas) {
        return new Promise((resolve) => {
          uni
            .createSelectorQuery()
            .in(this)
            .select(`#${canvasId}`)
            .fields({
              node: true,
              size: true,
            })
            .exec((res) => {
              let { node, width, height } = res[0];
              width = width || 300;
              height = height || 300;
              const context = node.getContext(type);
              if (!canvasId.includes("_cloud")) {
                this.width = width;
                this.height = height;
              }
              node.width = width * pixelRatio;
              node.height = height * pixelRatio;
              this.config[canvasId] = {
                context,
                width,
                height,
                pixelRatio,
                canvas: node,
              };
              resolve(this.config[canvasId]);
            });
        });
      }
      return new Promise((resolve) => {
        uni
          .createSelectorQuery()
          .in(this)
          .select(`#${canvasId}`)
          .boundingClientRect()
          .exec((res) => {
            if (res) {
              let { width, height } = res[0];
              width = width || 300;
              height = height || 300;
              const context = uni.createCanvasContext(canvasId, this);
              if (!canvasId.includes("_cloud")) {
                this.width = width;
                this.height = height;
                // #ifdef MP-ALIPAY
                this.nodeWidth = width * pixelRatio;
                this.nodeHeight = height * pixelRatio;
                // #endif
              }
              this.config[canvasId] = {
                context: extendContext(context),
                width,
                height,
                // #ifdef H5 || APP-PLUS
                pixelRatio: 1,
                oPixelRatio: pixelRatio,
                // #endif
                // #ifndef H5
                pixelRatio,
                // #endif
              };
              resolve(this.config[canvasId]);
            }
          });
      });
    },
    touchStart(e) {
      if (this.canvasEl) {
        this.canvasEl.dispatchEvent("touchstart", wrapEvent(e));
      }
    },
    touchMove(e) {
      if (this.canvasEl) {
        this.canvasEl.dispatchEvent("touchmove", wrapEvent(e));
      }
    },
    touchEnd(e) {
      if (this.canvasEl) {
        this.canvasEl.dispatchEvent("touchend", wrapEvent(e));
      }
    },
  },
};
</script>
<style scoped>
.f2-chart {
  width: 100%;
  height: 100%;
  position: relative;
}

.f2-chart__canvas {
  width: 100%;
  height: 100%;
}
</style>
