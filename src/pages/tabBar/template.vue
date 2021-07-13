<template>
  <view class="content">
    <navigator class="content--item" url="/pages/template/f2-chart">
      <button type="default">AntV F2</button>
    </navigator>
      <navigator class="content--item" url="/pages/template/ucharts">
      <button type="default">uCharts</button>
    </navigator>
  </view>
</template>

<script>
export default {
  data() {
    return {};
  },
  onLoad() {},
  methods: {},
};
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 30rpx;
}

.content--item {
  width: 100%;
  padding: 20rpx;
}
</style>
