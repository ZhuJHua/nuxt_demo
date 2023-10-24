<template>
  <div class="table-box">
    <div class="title">
      <h2>demo1</h2>
    </div>

    <!--  query -->
    <div class="query-box">
      <el-input class="query-input" v-model="queryInput" placeholder="请输入搜索"/>
      <div class="btn-list">
        <el-button type="primary" @click="handleAdd">增加</el-button>
        <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length>0">删除多选</el-button>
      </div>
    </div>
    <el-table ref="multipleTableRef"
              :data="tableData"
              style="width: 100%"
              @selection-change="handleSelectionChange"
              border="border">
      <el-table-column type="selection" width="55"/>
      <el-table-column prop="name" label="姓名" width="120"/>
      <el-table-column prop="email" label="邮箱" width="120"/>
      <el-table-column prop="phone" label="电话" width="120"/>
      <el-table-column prop="state" label="状态" width="120"/>
      <el-table-column prop="address" label="地址" width="300"/>
      <el-table-column fixed="right" label="操作" width="120">
        <template #default="scope">
          <el-button link type="primary" size="small" @click="handleRowDel(scope.row)"
                     style="color: #F56C6C">删除
          </el-button>
          <el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!--dialog-->
    <el-dialog v-model="dialogFormVisible" :title="dialogType==='add'?'新增':'编辑'">
      <el-form :model="tableForm">
        <el-form-item label="姓名" :label-width="80">
          <el-input v-model="tableForm.name" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="邮箱" :label-width="80">
          <el-input v-model="tableForm.email" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="电话" :label-width="80">
          <el-input v-model="tableForm.phone" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="状态" :label-width="80">
          <el-input v-model="tableForm.state" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="地址" :label-width="80">
          <el-input v-model="tableForm.address" autocomplete="off"/>
        </el-form-item>
      </el-form>
      <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogConfirm">
          确认
        </el-button>
      </span>
      </template>
    </el-dialog>
  </div>
</template>
<script setup>

import {ref, watch} from "vue";
//数据
let dialogFormVisible = ref(false)
let multipleSelection = ref([])
let tableForm = ref({
  id: undefined,
  name: '张三',
  email: '1234@qq.com',
  phone: '1234567',
  state: "在职",
  address: "广东"
})
let dialogType = ref("add")
let queryInput = ref("")
let tableData = ref([{
  id: 1,
  name: 'Tom',
  email: '123@qq,com',
  phone: '123456',
  state: 'California',
  address: 'No. 189, Grove St, Los Angeles',
}, {
  id: 2,
  name: 'Bob',
  email: '123@qq,com',
  phone: '123456',
  state: 'California',
  address: 'No. 189, Grove St, Los Angeles',
}, {
  id: 3,
  name: 'Jack',
  email: '123@qq,com',
  phone: '123456',
  state: 'California',
  address: 'No. 189, Grove St, Los Angeles',
}, {
  id: 4,
  name: 'Jace',
  email: '123@qq,com',
  phone: '123456',
  state: 'California',
  address: 'No. 189, Grove St, Los Angeles',
},])
let tableDataCopy = ref(Object.assign(tableData.value))


//搜索
watch(queryInput, () => {
  if (queryInput.value.length > 0) {
    tableData.value = tableData.value.filter(e => e.name.toLowerCase().includes(queryInput.value.toLowerCase()))
  } else {
    tableData.value = tableDataCopy.value
  }

})

//编辑
const handleEdit = (row) => {
  dialogFormVisible.value = true
  dialogType.value = 'edit'
  tableForm.value = {
    ...row
  }
}
//删除一条
const handleRowDel = ({id}) => {
  //通过id获取
  //通过
  tableData.value = tableData.value.filter(e => e.id !== id)
}
//增加
const handleAdd = () => {
  dialogFormVisible.value = true
  tableForm.value = {address: "", email: "", name: "", phone: "", state: ""}
  dialogType.value = 'add'
}
//删除多选
const handleDelList = () => {
  multipleSelection.value.forEach(id => {
    handleRowDel({id})
  })
  multipleSelection.value = []
}
const dialogConfirm = () => {
  dialogFormVisible.value = false
  //判断是新增还是编辑
  if (dialogType.value === 'add') {
    //拿到数据
    //添加到table
    tableData.value.push({
      id: tableData.value.length + 1,
      ...tableForm.value
    })
  } else {
    //获取当前的索引
    let index = tableData.value.findIndex(e => e.id === tableForm.value.id)
    //替换数据
    tableData.value[index] = tableForm.value
  }

}

//选中
const handleSelectionChange = (val) => {
  //先清空
  multipleSelection.value = []
  val.forEach(e => {
    multipleSelection.value.push(e.id)
  })
}


</script>

<style scoped>
.table-box {
  width: 800px;
  margin: 200px auto;
}

.title {
  text-align: center;
}

.query-box {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.query-input {
  width: 200px;
}
</style>
