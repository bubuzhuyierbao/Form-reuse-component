<!---@modules: 表单组件Table------>
<template>
  <el-table
    style="width: 100%"
    :data="data"
    :disabled="disabled"
    :border="border"
    @selection-change="handleSelectionChange"
  >
    <!-- 表格列保持不变 -->
    <template v-for="column in columns" :key="column.prop">
      <el-table-column
        :type="column.type"
        :label="column.label"
        :prop="column.prop"
        :align="column.align"
        :width="column.width"
      >
        <!-- 操作列 -->
        <template v-if="column.slotName" #default="{ row }">
          <slot :name="column.slotName" :row="row" />
        </template>
        <!-- 无插槽的列 -->
        <template v-else #default="{ row }">
          <!-- 空值显示 - -->
          {{ row[column.prop] ? row[column.prop] : "-" }}
        </template>
      </el-table-column>
    </template>

    <el-table-column
      v-if="hasOperation"
      label="操作"
      :width="operationW"
      align="center"
      :fixed="fixed"
    >
      <template #default="{ row, $index }">
        <slot name="operation" :row="row" :index="$index" />
      </template>
    </el-table-column>
  </el-table>

  <!-- 分页 -->
  <div class="mt-4 flex justify-end">
    <el-pagination
      background
      :total="total"
      :current-page="currentPage"
      layout="prev, pager, next"
      @current-change="handleCurrentChange"
      @size-change="handleSizeChange"
    />
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref, watch } from "vue";

const props = defineProps({
  data: Array, //表单数据
  columns: {
    //表头数据
    type: Array,
    required: true,
  },
  border: {
    //是否带有纵向边框
    type: Boolean,
    default: false,
  },
  disabled: {
    //禁用表格行的操作
    type: Boolean,
    default: false,
  },
  total: {
    //数据总数
    type: Number,
    required: true,
  },
  initialPageNo: {
    //默认页码
    type: Number,
    default: 1,
  },
  operationW: {
    //操作列宽度
    type: Number,
    default: 200,
  },
  hasOperation: {
    //是否开启操作列
    type: Boolean,
    default: true,
  },
  fixed: {
    //操作列固定位置
    type: String,
    default: "right",
  },
});

const emits = defineEmits(["selectionChange", "currentChange", "sizeChange"]);

const currentPage = ref(props.initialPageNo);

watch(
  () => props.initialPageNo,
  (newPageNo) => {
    currentPage.value = newPageNo;
  },
);

// 分页变化处理
const handleCurrentChange = (val) => {
  currentPage.value = val;
  emits("currentChange", val);
};

const handleSizeChange = (val) => {
  emits("sizeChange", val);
};

const handleSelectionChange = (selection) => {
  emits("selectionChange", selection);
};
</script>
<style lang="scss" scoped></style>
