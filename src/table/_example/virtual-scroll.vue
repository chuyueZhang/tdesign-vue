<template>
  <t-space direction="vertical">
    <t-space align="center">
      <t-button @click="scrollToElement">滚动到指定元素</t-button>
      <t-checkbox v-model="bordered">是否显示边框</t-checkbox>
      <!-- <t-button @click="setLowerHeight">lower height</t-button>
      <t-button @click="setHigherHeight">higher height</t-button> -->
    </t-space>
    <!--
      1. rowHeight 接近平均高度即可
      2. bufferSize 别太大，5 ～ 30 之间合适
      3. 如果是固定行高请设置 isFixedRowHeight: true。rowHeight 设置精确值
      4. 当数据量小于 `scroll.threshold` 时，无论虚拟滚动的配置是否存在，组件内部都不会开启虚拟滚动，默认值 100
    -->
    <t-table
      ref="tableRef"
      row-key="id"
      :columns="columns"
      :data="data"
      :scroll="{ type: 'virtual', rowHeight: 69, bufferSize: 10 }"
      :height="height"
      :bordered="bordered"
    >
    </t-table>
  </t-space>
</template>

<script lang="jsx">
import { ErrorCircleFilledIcon, CheckCircleFilledIcon, CloseCircleFilledIcon } from 'tdesign-icons-vue';

const columns = [
  { colKey: 'serial-number', width: 80, title: '序号' },
  { colKey: 'applicant', title: '申请人', width: '100' },
  {
    colKey: 'status',
    title: '申请状态',
    width: '150',
    cell: (h, { rowIndex }) => {
      const status = rowIndex % 3;
      const statusNameListMap = {
        0: { label: '审批通过', theme: 'success', icon: <CheckCircleFilledIcon /> },
        1: { label: '审批失败', theme: 'danger', icon: <CloseCircleFilledIcon /> },
        2: { label: '审批过期', theme: 'warning', icon: <ErrorCircleFilledIcon /> },
      };
      return (
        <t-tag shape="round" theme={statusNameListMap[status].theme} variant="light-outline">
          {statusNameListMap[status].icon}
          {statusNameListMap[status].label}
        </t-tag>
      );
    },
  },
  { colKey: 'matters', title: '申请事项', width: '140' },
  { colKey: 'detail.email', title: '邮箱地址' },
  { colKey: 'createTime', title: '申请时间' },
];

const initialData = [];
for (let i = 0; i < 10; i++) {
  initialData.push({
    id: i + 1,
    applicant: ['贾明', '张三', '王芳'][i % 3],
    channel: ['电子签署', '纸质签署', '纸质签署'][i % 3],
    detail: {
      email: ['w.cezkdudy@lhll.au', 'r.nmgw@peurezgn.sl', 'p.cumx@rampblpa.ru'][i % 3],
    },
    matters: ['部分宣传物料制作费用', 'algolia 服务报销', '相关周边制作费', '激励奖品快递费'][i % 4],
    time: [2, 3, 1, 4][i % 4],
    createTime: ['2022-01-01', '2022-02-01', '2022-03-01', '2022-04-01', '2022-05-01'][i % 4],
  });
}
// 为了使得表格滚动更加平稳，建议指定row-height参数值为接近表格的平均行高
const times = Array.from(new Array(1000), () => '');
const testData = [];
times.forEach((item, i) => {
  const k = i % 10;
  testData[i] = { ...initialData[k], id: i + 1 };
});

export default {
  name: 'VirtualScroll',
  data() {
    return {
      tableRef: null,
      data: [...testData],
      columns,
      bordered: true,
      height: 300,
    };
  },

  methods: {
    scrollToElement() {
      this.$refs.tableRef.scrollToElement({
        // 跳转元素下标（第 256 个元素位置）
        index: 255,
        // 滚动元素距离顶部的距离（如表头高度）
        top: 47,
        // 行高度动态变化场景场景下，即 isFixedRowHeight = false。延迟设置元素位置，一般用于依赖不同高度异步渲染等场景，单位：毫秒。（固定高度不需要这个）
        time: 60,
      });
    },

    // setLowerHeight() {
    //   this.height = 150;
    // },

    // setHigherHeight() {
    //   this.height = 600;
    // },
  },
};
</script>
