<script setup lang="ts">
import FlowChart from './components/FlowChart.vue';
import { ref, onMounted, compile, computed } from 'vue';
import { TinyGrid, TinyGridColumn, TinyButton,TinyPager , TinyInput ,TinyFlowchart } from '@opentiny/vue';
import { iconSearch } from '@opentiny/vue-icon';
const TinyIconSearch = iconSearch();

const expandConfigData = {
  expandAll: false, // 默认展开所有行
  trigger: 'row', // 触发方式 default（点击按钮触发）,cell（点击单元格触发）,row（点击行触发）
  expandRowKeys: [], // 默认展开指定行（需要有 row-id）
  accordion: false, // 对于同一级的节点，每次只能展开一个
  activeMethod(row) {
    // 控制是否渲染展开行
    return row.id !== '4';
  },
  showIcon: true, // 配置是否显示展开图标
};

const tableData = ref([
  {
    id: '1',
    type: '进行中',
    taskName: '上海南京东路旗舰店改造项目全专业设计',
    name: 'VIP-2026-294 旗舰店改造项目',
    taskType: '跨专业',
    initiator: '郑娇',
    taskCode: 'SJLX-2026-294',
    major: '精装专业',
    completionTime: '2026-12-31',
    taskStartTime: '2026-01-01',
    chartData: {
      nodes: [
        ['1', 1, '设计立项', '2026-12-31', [], 1, 1,'2026-12-31'],
        ['2', 4, '设计任务书', '', [], 1, 2],
        ['3', 4, '设计策划', '2026-12-31', [], 1, 3],
        ['4', 4, '设计交底', '', [], 1, 4],
        ['5', 0, '定制评审要素', '2026-12-31', [], 1, 5,'2026-12-31'],
        ['6', 0, '概念成果上传', '', [], 1, 6],
        ['7', 0, '概念成果确认', '', [], 1, 7],
        ['8', 0, '方案成果上传', '', [], 1, 8],
        ['9', 0, '会签要点', '', [], 0, 8.5],
        ['10', 0, '方案成果确认', '', [], 1, 9],
        ['11', 0, '设计成果上传', '', [], 1, 10],
        ['12', 0, '会签要点', '', [], 0, 10.5],
        ['13', 0, '设计成果确认', '', [], 1, 11],
        ['14', 1, '施工图成果上传', '', [], 1, 12],
        ['15', 1, '会签要点', '', [], 0, 12.5],
        ['16', 1, '施工图成果确认', '', [], 1, 13],
        ['17', 0, '业务策略', '', [], 1, 14],
        ['18', 0, '招标图纸提供', '', [], 1, 15],
        ['19', 0, '施工图纸下发', '', [], 1, 16],
        ['20', 0, '深化图纸下发', '', [], 0, 16.5],
        ['21', 0, '专业线巡检', '', [], 1, 17],
        ['22', 0, '复盘总结', '', [], 1, 18],
        ['23', 0, '竣工图归档', '', [], 1, 19],
      ],
      links: [
        ['1', '2', '', 1],
        ['2', '3', '', 1],
        ['3', '4', '', 1],
        ['4', '5', '', 1],
        ['5', '6', '', 1],
        ['6', '7', '', 1],
        ['7', '8', '', 1],
        ['8', '9', '0 t1 c r0.5', 1, 'dash'],
        ['8', '10', '', 1],
        ['9', '10', '0.5 r0.5 c b1', 1, 'dash'],
        ['10', '11', '', 1],
        ['11', '12', '0 t1 c r0.5', 1, 'dash'],
        ['11', '13', '', 1],
        ['12', '13', '0.5 r0.5 c b1', 1, 'dash'],
        ['13', '14', '', 1],
        ['14', '15', '0 t1 c r0.5', 1, 'dash'],
        ['14', '16', '', 1],
        ['15', '16', '0.5 r0.5 c b1', 1, 'dash'],
        ['16', '17', '', 1],
        ['17', '18', '', 1],
        ['18', '19', '', 1],
        ['19', '20', '0 t1 c r0.5', 1, 'dash'],
        ['19', '21', '', 1],
        ['20', '21', '0.5 r0.5 c b1', 1, 'dash'],
        ['21', '22', '', 1],
        ['22', '23', '', 1],
      ],
    },
  },
  {
    id: '2',
    type: '进行中',
    taskName: '极目科技园改造项目全专业设计',
    name: '极目科技园二期项目',
    taskType: '单专业（分阶段）',
    initiator: '罗婵',
    taskCode: 'SJLX-2026-187',
    major: '软装专业',
    completionTime: '2022-1-31',
    taskStartTime: '2024-01-01',
    chartData: {
      nodes: [
        ['1', 1, '设计立项', '2026-12-31', [], 1, 1],
        ['2', 4, '设计任务书', '', [], 1, 2],
        ['3', 4, '设计策划', '2026-12-31', [], 1, 3],
        ['4', 4, '设计交底', '', [], 1, 4],
        ['5', 1, '定制评审要素', '2026-12-31', [], 1, 5],
        ['8', 1, '方案成果上传', '2026-12-31', [], 1, 6,'2026-12-31'],
        ['9', 1, '会签要点', '', [], 0, 6.5],
        ['10', 0, '方案成果确认', '', [], 1, 7],
        ['11', 0, '设计成果上传', '', [], 1, 8],
        ['12', 0, '会签要点', '', [], 0, 8.5],
        ['13', 0, '设计成果确认', '', [], 1, 9],
        ['14', 1, '施工图成果上传', '', [], 1, 10],
        ['15', 1, '会签要点', '', [], 0, 10.5],
        ['16', 1, '施工图成果确认', '', [], 1, 11],
        ['17', 0, '业务策略', '', [], 1, 12],
        ['18', 0, '招标图纸提供', '', [], 1, 13],
        ['19', 0, '施工图纸下发', '', [], 1, 14],
        ['20', 0, '深化图纸下发', '', [], 0, 14.5],
        ['21', 0, '专业线巡检', '', [], 1, 15],
        ['22', 0, '复盘总结', '', [], 1, 16],
        ['23', 0, '竣工图归档', '', [], 1, 17],
      ],
      links: [
        ['1', '2', '', 1],
        ['2', '3', '', 1],
        ['3', '4', '', 1],
        ['4', '5', '', 1],
        ['5', '8', '', 1],
        ['8', '9', '0 t1 c r0.5', 1, 'dash'],
        ['8', '10', '', 1],
        ['9', '10', '0.5 r0.5 c b1', 1, 'dash'],
        ['10', '11', '', 1],
        ['11', '12', '0 t1 c r0.5', 1, 'dash'],
        ['11', '13', '', 1],
        ['12', '13', '0.5 r0.5 c b1', 1, 'dash'],
        ['13', '14', '', 1],
        ['14', '15', '0 t1 c r0.5', 1, 'dash'],
        ['14', '16', '', 1],
        ['15', '16', '0.5 r0.5 c b1', 1, 'dash'],
        ['16', '17', '', 1],
        ['17', '18', '', 1],
        ['18', '19', '', 1],
        ['19', '20', '0 t1 c r0.5', 1, 'dash'],
        ['19', '21', '', 1],
        ['20', '21', '0.5 r0.5 c b1', 1, 'dash'],
        ['21', '22', '', 1],
        ['22', '23', '', 1],
      ],
    },
  },
  {
    id: '3',
    type: '进行中',
    taskName: '华为上海青浦研发项目（G组团）幕墙设计',
    name: '华为上海青浦研发项目',
    taskType: '单专业（不分阶段）',
    initiator: '程杰',
    taskCode: 'SJLX-2026-124',
    major: '建筑专业',
    completionTime: '2025-12-31',
    taskStartTime: '2025-01-01',
    chartData: {
      nodes: [
        ['1', 1, '设计立项', '2026-12-31', [], 1, 1],
        ['2', 4, '设计任务书', '', [], 1, 2],
        ['3', 4, '设计策划', '2026-12-31', [], 1, 3],
        ['4', 4, '设计交底', '', [], 1, 4],
        ['5', 0, '定制评审要素', '2026-12-31', [], 1, 5],
        ['6', 0, '概念成果上传', '', [], 1, 6],
        ['7', 0, '概念成果确认', '', [], 1, 7],
        ['8', 0, '方案成果上传', '', [], 1, 8],
        ['9', 0, '会签要点', '', [], 0, 8.5],
        ['10', 0, '方案成果确认', '', [], 1, 9],
        ['11', 0, '设计成果上传', '', [], 1, 10],
        ['12', 0, '会签要点', '', [], 0, 10.5],
        ['13', 0, '设计成果确认', '', [], 1, 11],
        ['14', 1, '施工图成果上传','2026-12-31', [], 1, 12 ,'2026-12-31'],
        ['15', 1, '会签要点', '', [], 0, 12.5],
        ['16', 1, '施工图成果确认', '', [], 1, 13],
        ['17', 0, '业务策略', '', [], 1, 14],
        ['18', 0, '招标图纸提供', '', [], 1, 15],
        ['19', 0, '施工图纸下发', '', [], 1, 16],
        ['20', 0, '深化图纸下发', '', [], 0, 16.5],
        ['21', 0, '专业线巡检', '', [], 1, 17],
        ['22', 0, '复盘总结', '', [], 1, 18],
        ['23', 0, '竣工图归档', '', [], 1, 19],
      ],
      links: [
        ['1', '2', '', 1],
        ['2', '3', '', 1],
        ['3', '4', '', 1],
        ['4', '5', '', 1],
        ['5', '6', '', 1],
        ['6', '7', '', 1],
        ['7', '8', '', 1],
        ['8', '9', '0 t1 c r0.5', 1, 'dash'],
        ['8', '10', '', 1],
        ['9', '10', '0.5 r0.5 c b1', 1, 'dash'],
        ['10', '11', '', 1],
        ['11', '12', '0 t1 c r0.5', 1, 'dash'],
        ['11', '13', '', 1],
        ['12', '13', '0.5 r0.5 c b1', 1, 'dash'],
        ['13', '14', '', 1],
        ['14', '15', '0 t1 c r0.5', 1, 'dash'],
        ['14', '16', '', 1],
        ['15', '16', '0.5 r0.5 c b1', 1, 'dash'],
        ['16', '17', '', 1],
        ['17', '18', '', 1],
        ['18', '19', '', 1],
        ['19', '20', '0 t1 c r0.5', 1, 'dash'],
        ['19', '21', '', 1],
        ['20', '21', '0.5 r0.5 c b1', 1, 'dash'],
        ['21', '22', '', 1],
        ['22', '23', '', 1],
      ],
    },
  },
]);

const selfTableRef = ref();
const total = computed(() => tableData.value.length);
onMounted(() => {
});
</script>

<template>
  <div class="chart-demo">
    <div class="title">Hi:,欢迎使用设计业务地铁图查询！</div>
    <div class="seek-box">
      <div class="left-box">
        <div class="box-title">活动设计看板</div>
        <div class="box-buttons">
          <tiny-button type="info"> 新增 </tiny-button>
          <tiny-button type="info"> 导出 </tiny-button>
          <tiny-button type="info"> 刷新 </tiny-button>
          <tiny-button type="info"> 复制审批链接 </tiny-button>
          <tiny-button type="info"> 催办 </tiny-button>
        </div>
      </div>
      <div class="right-box">
        <tiny-input
          v-model="input"
          placeholder="搜索单据编号/项目名称"
          input-box-type="underline"
          :prefix-icon="TinyIconSearch"
        >
        </tiny-input>
      </div>
    </div>
    <div class="chart-table">
      <tiny-grid
        ref="selfTableRef"
        :expand-config="expandConfigData"
        :data="tableData"
      >
        <tiny-grid-column
          type="index"
          title="#"
          width="100"
          align="center"
        ></tiny-grid-column>
        <tiny-grid-column
          field="type"
          title="状态"
          width="100"
        ></tiny-grid-column>
        <tiny-grid-column
          field="taskName"
          title="设计任务名称"
        ></tiny-grid-column>
        <tiny-grid-column field="name" title="项目名称"></tiny-grid-column>
        <tiny-grid-column
          field="taskType"
          title="设计任务类型"
          width="160"
        ></tiny-grid-column>
        <tiny-grid-column
          field="initiator"
          title="发起人"
          width="100"
        ></tiny-grid-column>
        <tiny-grid-column field="taskCode" title="任务编码" width="180">
          <template #default="data">
            <a>{{data.row.taskCode}}</a>
          </template>
        </tiny-grid-column>
        <tiny-grid-column
          field="major"
          title="专业"
          width="100"
        ></tiny-grid-column>
        <tiny-grid-column
          field="completionTime"
          title="施工图计划完成时间"
          width="180"
        ></tiny-grid-column>
        <tiny-grid-column
          field="taskStartTime"
          title="任务启动时间"
          width="120"
        ></tiny-grid-column>
        <tiny-grid-column
          type="expand"
          title="流程进度图"
          width="160"
          align="center"
        >
          <template #default="data">
            <FlowChart :chartData="data.row.chartData" />
            <span class="plan-type">计划完成：</span>
            <span class="actual-type">实际完成：</span>
          </template>
        </tiny-grid-column>
      </tiny-grid>
      <tiny-pager :total="total"></tiny-pager>
    </div>
  </div>
</template>

<style scoped>
.chart-demo {
  text-align: left;
}
.title {
  font-size: 20px;
  color: #fff;
  background: #1476ff;
  padding: 0 20px;
  height: 40px;
  line-height: 40px;
}

.seek-box {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background: #fff;
}

.left-box {
  display: flex;
  flex-direction: column;
}

.box-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 8px;
}

.box-buttons {
  display: flex;
  gap: 8px;
}

.btn {
  padding: 4px 12px;
  font-size: 14px;
  cursor: pointer;
}

.right-box {
  display: flex;
  align-items: center;
}

.search-input {
  padding: 4px 8px;
  font-size: 14px;
  width: 200px;
}
.chart-table {
  margin: 10px;
}
:deep(.tiny-grid-body__expanded-cell) {
  padding: 0;
}
.plan-type {
  position: relative;
  left: 40px;
  top: -38px;
  z-index: 9999;
  color: #999999;
}
.actual-type {
  position: relative;
  left: -32px;
  top: -16px;
  z-index: 9999;
  color: #999999;
}
</style>
