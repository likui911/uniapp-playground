<template>
  <view class="u-demo">
    <view class="u-config-wrap">
      <view class="u-config-item">
        <u-button @click="onClick">打开Picker</u-button>
      </view>
    </view>
    <sub-select
      v-model="value"
      @change="onChange"
      :list="menuList"
    ></sub-select>
  </view>
</template>

<script>
import subSelect from "@/components/sub-select";
export default {
  components: {
    subSelect,
  },
  data() {
    return {
      height: 30,
      bgColor: this.$u.color.bgColor,
      marginTop: 30,
      marginBottom: 30,
      value: false,
      input: "",
      menuList: [
        {
          label: "菜单一",
          value: "1",
        },
        {
          label: "菜单二",
          value: "2",
          children: [
            { label: "子菜单21", value: "21" },
            { label: "子菜单22", value: "22" },
            { label: "子菜单23", value: "23" },
          ],
        },
        { label: "菜单三", value: "3" },
      ],
    };
  },
  methods: {
    onChange(e) {
     console.log(e)
    },
    onClick() {
      this.value = true;
    },
  },
};
</script>

<style scoped>
.btn-wrap {
  margin: 100rpx 30rpx;
}
</style>