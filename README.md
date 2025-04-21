一个基于Elment Plus和vue的表单复用组件。使用组件el-table,使用环境vue

用法：

父组件导入

```
import Table from "./components/Table/index.vue";
```

父组件页面使用

```


<template>

 *<!-- 用户表格 -->*

    <div class="w-full overflow-x-auto">

   <Table

​    :columns="userColumn"

​    :data="userList"

​    :total="total"

​    :initialPageNo="pageNo"

​    :operationW="250"

​    @selectionChange="selectionChange"

​    @currentChange="onCurrentChange"

​    @sizeChange="onSizeChange"

   \>

​    *<!-- 状态列自定义内容 -->*

​    <template #blocked="{ row }">

​     <el-tag :type="row.blocked ? 'success' : 'info'">

​      {{ row.blocked ? "启用" : "停用" }}

​     </el-tag>

​    </template>

​    <template #operation="{ row, index }">

​     <el-button type="primary" text @click="openAddUserDrawer(row)"\>编辑</el-button\>

​    </template>

   </Table>

  </div>

</template>
```

表单组件props

```
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

```

