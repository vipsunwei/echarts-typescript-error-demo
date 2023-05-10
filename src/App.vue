<script setup lang="ts">
import type {
  EChartsOption,
  SeriesOption,
  XAXisComponentOption,
} from "echarts";
import { BarChart, LineChart } from "echarts/charts";
import { GridComponent, LegendComponent } from "echarts/components";
import * as echarts from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";

import { onMounted, onUnmounted, ref } from "vue";

echarts.use([
  BarChart,
  LineChart,
  GridComponent,
  LegendComponent,
  CanvasRenderer,
]);

const chart = ref<echarts.ECharts>();
const chartRef = ref<HTMLDivElement>();
const option: EChartsOption = {
  backgroundColor: "transparent",
  grid: { bottom: 30 },
  legend: {
    data: ["事件", "温度", "水量"],
    textStyle: {
      fontSize: 16,
      color: "#ffffff",
    },
  },
  tooltip: {
    show: true,
    // formatter: function (params) {
    //   const unit = params.seriesName === '温度' ? '°' : params.seriesName === '水量' ? 'ml' : ''
    //   return params.seriesName + '<br/>' + params.name + '  ' + params.value + ' ' + unit
    // },
  },
  xAxis: [
    {
      type: "category",
      axisLabel: { color: "#fff" },
      axisLine: { lineStyle: { color: "#C6CCCF" } },
      axisTick: { show: false },
      data: [
        //   "1月",
        //   "2月",
        //   "3月",
        //   "4月",
        //   "5月",
        //   "6月",
        //   "7月",
        //   "8月",
        //   "9月",
        //   "10月",
        //   "11月",
        //   "12月",
      ],
    },
  ],

  yAxis: [
    {
      type: "value",
      name: "水量 (ml)",
      position: "left",
      nameTextStyle: { color: "#fff" },
      axisLabel: { color: "#fff" },
      splitLine: {
        show: true,
        lineStyle: { color: "rgba(77,91,107,0.9)", type: "dashed" },
      },
      axisLine: { show: true, lineStyle: { color: "#fff" } },
    },
    {
      type: "value",
      name: "温度 (°)",
      position: "right",
      nameTextStyle: { color: "#fff" },
      axisLabel: { color: "#fff" },
      splitLine: { show: false },
      axisLine: { show: true, lineStyle: { color: "#fff" } },
    },
  ],
  series: [
    {
      name: "事件",
      type: "bar",
      barWidth: 9,
      itemStyle: {
        color: "#27B6D8",
      },
      data: [
        /* 事件数据 */
      ],
    },
    {
      name: "温度",
      type: "line",
      symbolSize: 7,
      itemStyle: {
        color: "#AA4425",
      },
      yAxisIndex: 1,
      data: [
        /* 温度数据 */
      ],
    },
    {
      name: "水量",
      type: "line",
      symbolSize: 7,
      itemStyle: {
        color: "#0680F1",
      },
      data: [
        /* 水量数据 */
      ],
    },
  ],
};

const onResize = () => {
  if (chart.value) {
    chart.value.resize();
  }
};

interface UpdateChartParam {
  incident: number[];
  temp: number[];
  water: number[];
  monthArr?: string[];
}

const updateChart = ({ incident, temp, water, monthArr }: UpdateChartParam) => {
  if (chart.value) {
    (option.xAxis as XAXisComponentOption[])[0].data = monthArr;
    (option.series as SeriesOption[])[0].data = incident;
    (option.series as SeriesOption[])[1].data = temp;
    (option.series as SeriesOption[])[2].data = water;
    chart.value.setOption(option);
  }
};

onMounted(() => {
  if (chartRef.value) {
    chart.value = echarts.init(chartRef.value, "dark");

    chart.value.setOption(option);

    window.addEventListener("resize", onResize);
  }

  // 模拟数据更新
  setTimeout(() => {
    const monthArr = [
      "1月",
      "2月",
      "3月",
      "4月",
      "5月",
      "6月",
      "7月",
      "8月",
      "9月",
      "10月",
      "11月",
      "12月",
    ];
    const incident = [10, 110, 10, 13, 14, 16, 17, 180, 10, 180, 70, 150];
    const temp = [10, 11, 12, 13, 14, 26, 27, 28, 19, 18, 17, 15];
    const water = [100, 110, 120, 130, 140, 160, 170, 180, 190, 180, 170, 150];
    updateChart({ incident, temp, water, monthArr });
  }, 1000);
});

onUnmounted(() => {
  window.removeEventListener("resize", onResize);

  if (chart.value) {
    chart.value.dispose();
  }
});
</script>

<template>
  <div ref="chartRef" style="width: 100%; height: 100%"></div>
</template>

<style scoped></style>
