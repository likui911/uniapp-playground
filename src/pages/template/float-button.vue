<template>
  <view class="u-demo">
    <view class="u-config-wrap">
      <view class="u-config-item"> value:{{ value }} </view>
    </view>
    <float-button
      openText="打开"
      closeText="关闭"
      background="#18b566"
      bottom="50"
      :list="menuList"
      @click="onClick"
    />
  </view>
</template>

<script>
import floatButton from "@/components/float-button/float-button.vue";
export default {
  components: {
    floatButton,
  },
  data() {
    return {
      value: "",
      menuList: [
        {
          name: "菜单一",
          value: 0,
          background: "#fa3534",
        },
        {
          name: "菜单二",
          value: 1,
          background: "#dd6161",
        },
        {
          name: "菜单三",
          value: 2,
          background: "#ff9900",
        },
        {
          name: "菜单四",
          value: 3,
          background: "#f29100",
        },
      ],
    };
  },
  methods: {
    onClick(e) {
      this.value = e;
    },
  },
};
</script>

<style scoped>
.btn-wrap {
  margin: 100rpx 30rpx;
}

.u-config-item {
  padding: 10px;
  text-align: center;
  font-size: 30px;
}
</style>