<template>
  <t-layout-page>
    <t-layout-page-item>
      <t-table
        title="单选框"
        ref="selectionTable"
        :table="table"
        :columns="table.columns"
        @radioChange="radioChange"
        :rowClickRadio="false"
        :isShowPagination="false"
      >
       <template #toolbar>
          <el-button size="default" type="primary" @click="cancelSelect"
            >取消选中</el-button
          >
        </template>
      </t-table>
    </t-layout-page-item>
  </t-layout-page>
</template>

<script setup lang="ts">
import { ref } from "vue"
let table = ref<TableTypes.Table>({
  firstColumn: { type: "radio", fixed: true },
  // 接口返回数据
  data: [
    {
      id: "1",
      date: "2019-09-25",
      date1: "2019-09-26",
      name: "张三",
      status: "2",
      address: "广东省广州市天河区"
    },
    {
      id: "2",
      date: "2019-09-26",
      date1: "2019-09-27",
      name: "张三1",
      status: "1",
      address: "广东省广州市天广东省广州市天河区2广东省广州市天河区2河区2"
    },
    {
      id: "3",
      date: "2019-09-26",
      date1: "2019-09-28",
      name: "张三1",
      status: "1",
      address: "广东省广州市天广东省广州市天河区2广东省广州市天河区2河区2"
    },
    {
      id: "4",
      date: "2019-09-26",
      date1: "2019-09-29",
      name: "张三1",
      status: "1",
      address: "广东省广州市天广东省广州市天河区2广东省广州市天河区2河区2"
    }
  ],
  // 表头数据
  columns: [
    { prop: "name", label: "姓名", minWidth: "100" },
    { prop: "date", label: "日期", minWidth: "180" },
    { prop: "status", label: "状态", minWidth: "80" },
    { prop: "address", label: "地址", minWidth: "220" }
  ]
})
// 获取ref
const selectionTable: any = ref<HTMLElement | null>(null)
// 选择单选框
const radioChange = (val: any) => {
  console.log("选择单选框", val)
}
// 取消选择
const cancelSelect = () => {
  selectionTable.value.clearRadioHandle()
}
</script>
