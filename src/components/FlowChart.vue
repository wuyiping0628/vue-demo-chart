<template>
  <div class="tiny-chart-demo" ref="container">
    <tiny-flowchart
      ref="chart"
      :data="chartDataRaw"
      :config="chartConfigRaw"
      @click-node="onClickNode"
      @click-link="onClickLink"
      @click-blank="onClickBlank"
    >
      <template #label="params">
          <div class="label-item label-title">
            {{ params.afterNode.raw.info.label }}
          </div>
          <div class="label-item label-date">
            {{ params.afterNode.raw.info.date }}
          </div>
          <div class="label-item label-other">
            {{ params.afterNode.raw.info.other }}
          </div>
      </template>
    </tiny-flowchart>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { TinyModal, TinyFlowchart } from "@opentiny/vue";
import { hooks, props } from "@opentiny/vue-common";

const { createConfig } = TinyFlowchart;

// template refs
const chart = ref(null);
const container = ref(null);
const nodeWrapperSize = 32;

let ro = null;

onMounted(() => {
  const parent =
    (container.value && container.value.parentNode) || container.value;
  if (!parent) return;

  const applySize = () => {
    const w = parent.offsetWidth * 0.95 || 0;
    console.log(111, w);

    // ensure chart cols accommodate max col in data
    const maxCol = Math.max(
      ...chartData.nodes.map((n) => Number(n.info.col) || 0)
    );
    chartConfig.cols = Math.max(chartConfig.cols || 0, Math.ceil(maxCol) + 1);
    chartConfig.width = w;

    // call refresh on component instance if available
    if (chart.value && typeof chart.value.refresh === "function") {
      chart.value.refresh({ graphWidth: w, adjustX: -nodeWrapperSize / 2 });
    }
  };

  // initial
  applySize();

  ro = new ResizeObserver(() => {
    applySize();
  });

  ro.observe(parent);
});

onBeforeUnmount(() => {
  if (ro) {
    ro.disconnect();
    ro = null;
  }
});

// accept chartData from parent
const propsData = defineProps({
  chartData: {
    type: Object,
    default: () => ({ nodes: [], links: [] }),
  },
});

const chartData = propsData.chartData || {};

const chartConfig = createConfig();

// content 插槽需更大展示空间，默认 listWidth 62px 过小会导致文字挤压重叠
chartConfig.listWidth = 160;
// label 显示优化：更宽的 label 避免换行重叠，
chartConfig.labelWidth = 140;
chartConfig.anchor = "center";
chartConfig.labelHeight = 80;

const chartDataRaw = hooks.markRaw(chartData);
const chartConfigRaw = hooks.markRaw(chartConfig);

function onClickNode(_afterNode, _e) {
  TinyModal.message("click-node");
}

function onClickLink(_afterLink, _e) {
  TinyModal.message("click-link");
}

function onClickBlank(_param, _e) {
  TinyModal.message("click-blank");
}
</script>

<style scoped>
.tiny-chart-demo {
  height: 172px;
  overflow: hidden;
  margin-left: 20px;
}
:deep(
    .tiny-flow-chart__node-icon-wrapper .tiny-flow-chart__node-icon.complete
  ) {
  background: #5cb300 !important;
}
:deep(
    .tiny-flow-chart__node-icon-wrapper .tiny-flow-chart__node-icon.complete svg
  ) {
  fill: #fff !important;
}

:deep(.tiny-flow-chart__node-icon-wrapper .tiny-flow-chart__node-icon.fail) {
  background: #fd7d75 !important;
  border-color: #fd7d75 !important;
}
:deep(
    .tiny-flow-chart__node-icon-wrapper .tiny-flow-chart__node-icon.fail svg
  ) {
  fill: #fd7d75 !important;
}
:deep(.tiny-flow-chart__node-label) {
  /* allow label container to expand vertically so long labels won't be clipped */
  height: auto !important;
  max-width: 80px !important; /* matches chartConfig.labelWidth */
  min-height: 80px !important; /* matches chartConfig.labelHeight */
}

:deep(.label-item.label-title) {
  display: block;
  white-space: normal !important;
  word-break: break-word !important;
  overflow-wrap: break-word !important;
  text-overflow: clip !important;
}
:deep(
    .tiny-flow-chart
      .tiny-flow-chart__node-icon-wrapper
      .tiny-flow-chart__node-label
  ) {
  left: 50% !important;
  transform: translateX(-50%) !important;
}
.label-title{
  height: 36px;
}
.label-item{
  margin-top: 4px;
  cursor: pointer;
  text-align: center;
}
.label-date{
  color: rgb(153, 153, 153);
}
.label-item.label-other{
  margin-top: 4px !important;
  color: rgb(153, 153, 153);
}
</style>
