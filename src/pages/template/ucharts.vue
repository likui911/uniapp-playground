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
      <view class="charts-box">
        <qiun-data-charts
          type="pie"
          :chartData="chartsDataPie"
          :echartsH5="true"
          :echartsApp="true"
        />
      </view>
    </view>
  </view>
</template>

<script>
import qiunDataCharts from "@/components/qiun-data-charts/qiun-data-charts";
export default {
  components: { qiunDataCharts },
  data() {
    return {
      chartsDataPie: {
        series: [
          {
            data: [
              {
                name: "一班",
                value: 50,
              },
              {
                name: "二班",
                value: 30,
              },
              {
                name: "三班",
                value: 20,
              },
              {
                name: "四班",
                value: 18,
              },
              {
                name: "五班",
                value: 8,
              },
            ],
          },
        ],
      },
    };
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

.charts-box {
  width: 100%;
  height: 300px;
}
</style>
