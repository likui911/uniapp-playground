<template>
  <view class="content">
    <view class="box">
      <f2Chart ref="chart"></f2Chart>
    </view>
  </view>
</template>

<script>
import F2 from "@antv/f2";
import f2Chart from "@/components/f2chart/";
export default {
  components: { f2Chart },
  data() {
    return {
      baseData: [
        { item: "银行卡", amount: 20000, type: "收入" },
        { item: "银行卡", amount: 500, type: "支出" },
        { item: "发货结算单", amount: 3115, type: "收入" },
        { item: "发货结算单", amount: 0, type: "支出" },
        { item: "微信", amount: 120, type: "收入" },
        { item: "微信", amount: 120, type: "支出" },
        { item: "支付宝", amount: 2300, type: "收入" },
        { item: "支付宝", amount: 350, type: "支出" },
        { item: "工价结算", amount: 280, type: "收入" },
        { item: "工价结算", amount: 150, type: "支出" },
      ],
    };
  },
  mounted() {
    this.$refs.chart.init((config) => {
      const chart = new F2.Chart(config);
      chart.source(this.baseData);
      chart.interval().position("item*amount").color("type").adjust({
        type: "dodge",
        marginRatio: 0.15, // 设置分组间柱子的间距
      });
      chart.render();
      // 需要把 chart 返回
      return chart;
    });
  },
  methods: {},
};
</script>

<style scoped lang="scss">
.content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 30rpx;
}

.box {
  background-color: #fff;
  border-radius: 20rpx;
  padding: 10rpx;
}
</style>
