<template>
  <div class="charts-wrapper">
    <div id="bar" class="chart-box card"></div>
    <div id="main1" class="chart-box card"></div>
    <div id="pie" class="chart-box card"></div>
  </div>
</template>

<script setup>
import {reactive, onMounted} from "vue";
import * as echarts from "echarts";
import request from "@/utils/request.js";

const barOption = {
  title: { text: '各部门员工数量' },
  tooltip: {},
  legend: { data: ['员工数量'] },
  xAxis: { data: [] },
  yAxis: {},
  series: [{ name: '员工数量', type: 'bar', data: [] }]
}

const option1 = {
  title: { text: '每日文章数量' },
  tooltip: {},
  legend: { data: ['文章数量'] },
  xAxis: { data: [] },
  yAxis: {},
  series: [{ name: '文章数量', type: 'line', data: [], smooth: true }]
}

const pieOption = {
  title: { text: '员工部门分布', left: 'center' },
  tooltip: { trigger: 'item', formatter: '{a} <br/>{b}: {c}人 ({d}%)' },
  legend: { orient: 'vertical', left: 'left' },
  series: [{
    name: '部门分布',
    type: 'pie',
    radius: '50%',
    data: [],
    emphasis: { itemStyle: { shadowBlur: 10, shadowOffsetX: 0, shadowColor: 'rgba(0, 0, 0, 0.5)' } }
  }]
}

onMounted(() => {
  request.get('/barData').then(res => {
    barOption.xAxis.data = res.data.department;
    barOption.series[0].data = res.data.count;
    echarts.init(document.getElementById('bar')).setOption(barOption);
  })
  request.get('/lineData').then(res => {
    option1.xAxis.data = res.data.date;
    option1.series[0].data = res.data.count;
    echarts.init(document.getElementById('main1')).setOption(option1);
  })
  request.get('/pieData').then(res => {
    pieOption.series[0].data = res.data;
    echarts.init(document.getElementById('pie')).setOption(pieOption);
  })
})
</script>

<style scoped>
.charts-wrapper {
  display: flex;
  justify-content: start;
  gap: 1.25rem;
  flex-wrap: wrap;
}

.chart-box {
  width: 37.5rem;
  height: 25rem;
}

@media (max-width: 1024px) {
  .chart-box {
    width: 31.25rem;
    height: 21.875rem;
  }
}

@media (max-width: 768px) {
  .charts-wrapper {
    justify-content: center;
  }
  .chart-box {
    width: 100%;
    height: 20rem;
  }
}

@media (max-width: 576px) {
  .chart-box {
    height: 17.5rem;
  }
}
</style>
