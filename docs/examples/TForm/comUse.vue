<template>
  <t-layout-page>
    <t-layout-page-item>
      <t-form ref="TFormDemo" v-model="formOpts.ref" :formOpts="formOpts" :widthSize="1" />
    </t-layout-page-item>
  </t-layout-page>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted } from "vue"
import cascaderData from "./cascaderData.json"
const hobbyList = ref([
  { label: "吉他", value: "0" },
  { label: "看书", value: "1" },
  { label: "美剧", value: "2" },
  { label: "旅游", value: "3" },
  { label: "音乐", value: "4" }
])
const sexList = ref([
  { label: "女", value: 1 },
  { label: "男", value: 0 }
])
const statusList = ref([
  { label: "启用", value: 1 },
  { label: "停用", value: 0 }
])
const initials = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]
const stepList = Array.from({ length: 1000 }).map((_, idx) => ({
  value: `Option ${idx + 1}`,
  label: `${initials[idx % 10]}${idx}`
}))
const tableData = ref([
  { id: 1, code: 1, name: "物料名称1", spec: "物料规格1", unitName: "吨" },
  { id: 2, code: 2, name: "物料名称2", spec: "物料规格2", unitName: "吨" },
  { id: 3, code: 3, name: "物料名称3", spec: "物料规格3", unitName: "吨" },
  { id: 4, code: 4, name: "物料名称4", spec: "物料规格4", unitName: "吨" },
  { id: 5, code: 5, name: "物料名称5", spec: "物料规格5", unitName: "吨" },
  { id: 6, code: 6, name: "物料名称6", spec: "物料规格6", unitName: "吨" },
  { id: 7, code: 7, name: "物料名称7", spec: "物料规格7", unitName: "吨" },
  { id: 8, code: 8, name: "物料名称8", spec: "物料规格8", unitName: "吨" },
  { id: 9, code: 9, name: "物料名称9", spec: "物料规格9", unitName: "吨" }
])
// 获取ref
const TFormDemo: any = ref<HTMLElement | null>(null)
// 提交formOpts.ref 方式form表单
const submitForm = () => {
  formOpts.ref.validate(valid => {
    console.log(88, valid)
    console.log(77, formOpts.formData)
    if (!valid) return
    console.log("最终数据", formOpts.formData)
  })
}
// 提交form表单
// const submitForm = async () => {
//   const { valid, formData } = await TFormDemo.value.validate()
//   console.log('formOpts.ref', formOpts.ref)
//   console.log('formOpts.formData', formData)
//   if (!valid) return
//   console.log('最终数据', formData)
// }
// 重置form表单
const resetForm = () => {
  TFormDemo.value.selfResetFields()
}

const radioChange = row => {
  console.log("下拉选择表格-单选", row)
  formOpts.formData.deptCode = row?.id
}
const selectionChangeHandler = (row, ids) => {
  console.log("下拉选择表格--复选框", row, ids)
  formOpts.formData.createDeptCode = ids
}
const formOpts = reactive<FormTypes.FormOpts>({
  ref: null,
  labelWidth: "140px",
  formData: {
    sex: null,
    hobby: null,
    hobby1: null,
    hobby2: null,
    status: null,
    wechat: null,
    deptCode: null,
    createDeptCode: null,
    createDate: null,
    valDate1: null,
    valDate2: null,
    valDate3: null,
    date: null,
    icon: ""
  },
  fieldList: [
    {
      label: "性别",
      value: "sex",
      placeholder: "TSelect单选",
      type: "select-arr",
      comp: "t-select",
      ref: "adioSelectRef",
      isSelfCom: true,
      bind: { optionSource: sexList, valueCustom: "value" }
    },
    {
      label: "状态",
      value: "status",
      placeholder: "TSelect单选",
      type: "select-arr",
      comp: "t-select",
      isSelfCom: true,
      bind: { optionSource: statusList, valueCustom: "value" }
    },
    {
      label: "爱好",
      value: "hobby",
      placeholder: "TSelect多选",
      comp: "t-select",
      isSelfCom: true,
      bind: { multiple: true, optionSource: hobbyList, valueCustom: "value" }
    },
    {
      label: "爱好1",
      value: "hobby1",
      placeholder: "TSelect多选",
      comp: "t-select",
      isSelfCom: true,
      bind: { multiple: true, optionSource: hobbyList, valueCustom: "value" }
    },
    {
      label: "图标选择器",
      value: "icon",
      placeholder: "TSelectIcon图标选择",
      comp: "t-select-icon",
      isSelfCom: true
      // bind: { multiple: true }
    },
    {
      label: "年份",
      value: "createDate",
      placeholder: "TDatePicker选择年份",
      bind: { type: "year" },
      comp: "t-date-picker",
      eventHandle: {
        change: val => createDateChange(val)
      }
    },
    {
      label: "日期",
      value: "date",
      placeholder: "TDatePicker选择日期",
      comp: "t-date-picker"
    },
    {
      label: "月份范围",
      value: "valDate1",
      comp: "t-date-picker",
      bind: { type: "monthrange", isPickerOptions: true }
    },
    {
      label: "日期范围",
      value: "valDate2",
      comp: "t-date-picker",
      bind: { type: "daterange", isPickerOptions: true }
    },
    {
      label: "时间范围",
      value: "valDate3",
      comp: "t-date-picker",
      bind: { type: "datetimerange", isPickerOptions: true }
    },
    {
      label: "虚拟列表",
      value: "hobby2",
      placeholder: "TSelect虚拟列表",
      comp: "t-select",
      isSelfCom: true,
      bind: { useVirtual: true, optionSource: stepList }
    },
    {
      label: "部门",
      value: "deptCode1",
      placeholder: "el-cascader使用",
      comp: "el-cascader",
      isSelfCom: true,
      bind: {
        props: {
          children: "children",
          label: "deptName",
          value: "deptNum"
        },
        options: cascaderData.data
      }
    },
    {
      label: "下拉选择表格-单选",
      value: "deptCode",
      placeholder: "t-select-table单选使用",
      ref: "selectTableRef",
      comp: "t-select-table",
      isSelfCom: true,
      bind: {
        isKeyup: true,
        maxHeight: 400,
        keywords: { label: "name", value: "id" },
        table: { data: tableData },
        columns: [
          { label: "物料编号", width: "100px", prop: "code", align: "left" },
          { label: "物料名称", width: "149px", prop: "name" },
          { label: "规格", width: "149px", prop: "spec" },
          { label: "单位", width: "110px", prop: "unitName" },
          { label: "物料编号1", width: "149px", prop: "code" },
          { label: "物料名称1", width: "149px", prop: "name" }
        ]
      },
      eventHandle: {
        radioChange: val => radioChange(val)
      }
    },
    {
      label: "下拉选择表格-多选",
      value: "createDeptCode",
      placeholder: "t-select-table多选使用",
      comp: "t-select-table",
      isSelfCom: true,
      bind: {
        multiple: true,
        maxHeight: 400,
        keywords: { label: "name", value: "id" },
        table: { data: tableData },
        columns: [
          { label: "物料编号", width: "100px", prop: "code", align: "left" },
          { label: "物料名称", width: "149px", prop: "name" },
          { label: "规格", width: "149px", prop: "spec" },
          { label: "单位", width: "110px", prop: "unitName" },
          { label: "物料编号1", width: "149px", prop: "code" },
          { label: "物料名称1", width: "149px", prop: "name" }
        ]
      },
      eventHandle: {
        selectionChange: (val, ids) => selectionChangeHandler(val, ids)
      }
    }
  ],
  operatorList: [
    { label: "提交", bind: { type: "danger" }, fun: submitForm },
    { label: "重置", bind: { type: "primary" }, fun: resetForm }
  ]
})

const createDateChange = val => {
  console.log("年份选择", val)
}
</script>
