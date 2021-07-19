<template>
  <u-popup
    v-model="value"
    mode="bottom"
    :popup="false"
    :mask="true"
    :closeable="true"
    :safe-area-inset-bottom="true"
    close-icon-color="#ffffff"
    :z-index="uZIndex"
    :maskCloseAble="maskCloseAble"
    @close="close"
  >
    <view class="area-box">
      <view class="u-flex">
        <view class="area-item">
          <view class="u-padding-10 u-bg-gray" style="height: 100%">
            <scroll-view :scroll-y="true" style="height: 100%">
              <u-cell-group>
                <u-cell-item
                  class="cellitem"
                  v-for="(item, index) in list"
                  :title="item.label"
                  :arrow="false"
                  :index="index"
                  :key="index"
                  @click="provinceChange"
                  :title-style="{ width: '100%', 'text-align': 'center' }"
                >
                </u-cell-item>
              </u-cell-group>
            </scroll-view>
          </view>
        </view>
        <view class="area-item" v-if="hasChildren">
          <view class="u-padding-10 u-bg-gray" style="height: 100%">
            <scroll-view :scroll-y="true" style="height: 100%">
              <u-cell-group>
                <u-cell-item
                  v-for="(item, index) in children"
                  :title="item.label"
                  :arrow="false"
                  :index="index"
                  :key="index"
                  @click="cityChange"
                  :title-style="{ width: '100%', 'text-align': 'center' }"
                >
                </u-cell-item>
              </u-cell-group>
            </scroll-view>
          </view>
        </view>
      </view>
    </view>
  </u-popup>
</template>

<script>
export default {
  name: "sub-select",
  props: {
    list: {
      type: Array,
      default: [],
    },
    value: {
      type: Boolean,
      default: false,
    },
    maskCloseAble: {
      type: Boolean,
      default: true,
    },
    zIndex: {
      type: [String, Number],
      default: 0,
    },
  },
  data() {
    return {
      children: [],
    };
  },

  computed: {
    hasChildren() {
      return this.children.length > 0;
    },
    uZIndex() {
      // 如果用户有传递z-index值，优先使用
      return this.zIndex ? this.zIndex : this.$u.zIndex.popup;
    },
  },
  methods: {
    close() {
      this.$emit("input", false);
    },
    provinceChange(index) {
      let result = this.list[index];
      this.children = result.children || [];
      if (this.children.length == 0) {
        this.$emit("change", result);
        this.close();
      }
    },
    cityChange(index) {
      let result = this.children[index];
      this.$emit("change", result);
      this.close();
    },
  },
};
</script>
<style lang="scss" scoped>
.area-box {
  width: 100%;
  overflow: hidden;
  height: 800rpx;
  > view {
    width: 100%;
    transition: transform 0.3s ease-in-out 0s;
    transform: translateX(0);
  }
  .area-item {
    width: 100%;
    height: 800rpx;
  }
}
</style>