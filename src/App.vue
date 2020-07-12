<template>
  <div>
    <table width="100%" class="table">
      <thead>
        <tr @click="sortTable($event)">
          <th colspan="1" rowspan="2"><input :checked="checkAll" type="checkbox"  @change="changeAll($event)"></th>
          <th colspan="1" rowspan="2" width="20%">ID</th>
          <th colspan="1" rowspan="2" width="20%">Name</th>
          <th colspan="1" rowspan="2" width="20%">Location</th>
          <th colspan="2" rowspan="1" width="30%">
            Phone
          </th>
        </tr>
        <tr @click="sortTable($event)"><th>Office</th><th>Cell</th></tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in tableData" :key="item.id">
          <td><input type="checkbox" :checked="item.checked"  @change="changeRow($event, item, index)"></td>
          <td>{{ item.id }}</td>
          <td>
            <input v-if="item.addFlag" v-model="item.name" type="text">
            <div v-else>{{ item.name }}</div>
          </td>
          <td>
            <input v-if="item.addFlag" v-model="item.location" type="text">
            <div v-else>{{ item.location }}</div>
          </td>
          <td>
            <input v-if="item.addFlag" v-model="item.office" type="text">
            <div v-else>{{ item.office }}</div>
          </td>
          <td>
            <input v-if="item.editFlag || item.addFlag" v-model="item.cell" type="text" @blur="updateCell($event, item, index)">
            <div v-else @dblclick="dblclickCell($event, item, index)">
              {{ item.cell }}
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <button type="button" class="btn" :disabled="!tableData.some(item => item.checked)" color="error" @click="deleteData">Delete</button>
    <button type="button" class="btn" :disabled="!tableData.some(item => item.checked)" color="warning" @click="update">Update</button>
    <button type="button" class="btn" color="warning" @click="addRow">Add</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      checkAll: false,
      tableData: [{
          checked: false,
          id: 501,
          name: 'Khali Zhang',
          location: 'shanghai',
          office: 'x55778',
          cell: '650-353-1239'
        },{
          checked: false,
          id: 502,
          name: 'Mei Li',
          location: 'beijing',
          office: 'z55778',
          cell: '350-353-1239'
        }],
      editFlag: false
    }
  },
  methods: {
    changeAll(e) {
      this.tableData.forEach(item => {
        item.checked = e.target.checked
      })
      e.stopPropagation();
    },
    changeRow(e, row, index) {
      row.checked = e.target.checked
      this.tableData.splice(index, 1, row)
      if (e.target.checked)
        this.checkAll = this.tableData.every(item => item.checked)
      e.stopPropagation();
    },
    sortTable(e) {
      if (!e.target.textContent)
        return
      let key = (e.target.textContent).toLowerCase()
      this.tableData = this.tableData.sort((row1, row2) => row1[key] > row2[key] ? 1 : -1)
    },
    dblclickCell(e, item, index) {
      this.$set(item, 'editFlag', true)
      this.tableData.splice(index, 1, item)
    },
    updateCell(e, item, index) {
      item.cell = e.target.value
      item.editFlag = false
      this.tableData.splice(index, 1, item)
    },
    addRow() {
      this.tableData.push({
        name: '',
        location: '',
        office: '',
        cell: '',
        addFlag: true,
      })
    },
    deleteData() {
      this.tableData = this.tableData.filter(item => !item.checked)
    },
    update() {
      let str = '更新ID为'
      let indexs = '新增第'
      this.tableData.forEach((item, index) => {
        if (item.checked) {
          if (item.id)
            str += `${item.id} `
          else
            indexs += `${index+1}`
        }
      })
      window.confirm(`${str} ${indexs}条`)
    }
  }
}
</script>

<style>
.table{
  table-layout: fixed;
  border-collapse: separate;
}
.table th, .table td{
  padding: 12px 0;
    min-width: 0;
    box-sizing: border-box;
    text-overflow: ellipsis;
    vertical-align: middle;
    position: relative;
    text-align: left;
    border-bottom: 1px solid #ebeef5;
}
.table thead{
  background: #f5f7fa;
  color: #909399;
  font-weight: 500;
}
.btn{
    display: inline-block;
    margin: 20px;
    line-height: 1;
    white-space: nowrap;
    cursor: pointer;
    background: #fff;
    border: 1px solid #dcdfe6;
    color: #606266;
    -webkit-appearance: none;
    text-align: center;
    box-sizing: border-box;
    outline: none;
    padding: 12px 20px;
    font-size: 14px;
    border-radius: 4px;
}
.btn[color="error"]{
  color:#f07474;
  border-color: #f07474;
}
.btn[color="warning"]{
  color:#f4bf71;
  border-color:#f4bf71;
}
.btn[disabled]{
  cursor: not-allowed;
}
</style>
