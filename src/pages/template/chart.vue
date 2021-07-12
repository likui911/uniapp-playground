<template>
  <view class="content">
    <view class="box">
      <view class="title">收支报表</view>
      <view class="row">
        <view class="label">日期范围</view>
        <view class="tag active">近1周</view>
        <view class="tag">近1年</view>
        <view class="tag">自定义</view>
      </view>
      <view class="row">
        <view class="label">选择指标</view>
        <view class="tag active">按类型</view>
        <view class="tag">按收付款人</view>
      </view>
      <view class="row">
        <view class="label">收支类型</view>
        <view class="tag active">收入</view>
        <view class="tag">支出</view>
      </view>
      <f2-chart ref="chart"></f2-chart>
    </view>
  </view>
</template>

<script>
import F2 from "@/libs/f2/f2-all.min.js";
import f2Chart from "@/components/f2chart/";
export default {
  components: { f2Chart },
  data() {
    return {
      baseData: [
        {
          name: "银行卡",
          y: 6371664,
          const: "const",
        },
        {
          name: "微信",
          y: 7216301,
          const: "const",
        },
        {
          name: "支付宝",
          y: 1500621,
          const: "const",
        },
        {
          name: "发货结算",
          y: 586622,
          const: "const",
        },
        {
          name: "工价结算",
          y: 900000,
          const: "const",
        },
      ],
    };
  },
  mounted() {
    this.$refs.chart.init((config) => {
      const chart = new F2.Chart(config);
      chart.source(this.baseData);
      chart.coord("polar", {
        transposed: true,
        radius: 0.75,
      });
      chart.legend(false);
      chart.axis(false);
      chart.tooltip(false);
      chart.pieLabel({
        sidePadding: 10,
        label1: function label1(data, color) {
          return {
            text: data.name,
            fill: color,
          };
        },
        label2: function label2(data) {
          return {
            text:
              "￥" +
              String(Math.floor(data.y * 100) / 100).replace(
                /\B(?=(\d{3})+(?!\d))/g,
                ","
              ),
            fill: "#808080",
            fontWeight: "bold",
          };
        },
      });
      chart
        .interval()
        .position("const*y")
        .color("name"/*, ["#1890FF", "#13C2C2", "#2FC25B", "#FACC14", "#F04864"]*/)
        .adjust("stack");
      chart.render();
   
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
  padding: 30rpx 15rpx 30rpx;
  display: flex;
  flex-direction: column;
  font-size: 24rpx;

  .title {
    font-size: 36rpx;
    margin-bottom: 30rpx;
  }

  .row {
    display: flex;
    align-items: center;
    margin-bottom: 30rpx;
  }

  .label {
    color: #b4b4b4;
  }

  .tag {
    background-color: #f6f6f6;
    border-radius: 17rpx;
    padding: 10rpx 20rpx;
    margin-left: 30rpx;
  }

  .active {
    background-color: #faf9ff;
    color: #2979ff;
  }
}
</style>
