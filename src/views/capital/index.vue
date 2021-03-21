<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="id">
            <el-input v-model="form.id" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="idCard">
            <el-input v-model="form.idCard" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="contract">
            <el-input v-model="form.contract" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="name">
            <el-input v-model="form.name" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="date">
            <el-input v-model="form.date" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="money">
            <el-input v-model="form.money" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="term">
            <el-input v-model="form.term" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="company">
            <el-input v-model="form.company" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="car">
            <el-input v-model="form.car" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="oper">
            <el-input v-model="form.oper" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="operDate">
            <el-input v-model="form.operDate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="notes">
            <el-input v-model="form.notes" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="address">
            <el-input v-model="form.address" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="code">
            <el-input v-model="form.code" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="dueDate">
            <el-input v-model="form.dueDate" style="width: 370px;" />
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />
        <el-table-column prop="id" label="id" />
        <el-table-column prop="idCard" label="idCard" />
        <el-table-column prop="contract" label="contract" />
        <el-table-column prop="name" label="name" />
        <el-table-column prop="date" label="date" />
        <el-table-column prop="money" label="money" />
        <el-table-column prop="term" label="term" />
        <el-table-column prop="company" label="company" />
        <el-table-column prop="car" label="car" />
        <el-table-column prop="oper" label="oper" />
        <el-table-column prop="operDate" label="operDate">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.operDate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="notes" label="notes" />
        <el-table-column prop="address" label="address" />
        <el-table-column prop="code" label="code" />
        <el-table-column prop="dueDate" label="dueDate" />
        <el-table-column v-permission="['admin','capital:edit','capital:del']" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudCapital from '@/api/capital'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, idCard: null, contract: null, name: null, date: null, money: null, term: null, company: null, car: null, oper: null, operDate: null, notes: null, address: null, code: null, dueDate: null }
export default {
  name: 'Capital',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: '中登生成', url: 'api/capital', idField: 'id', sort: 'id,desc', crudMethod: { ...crudCapital }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'capital:add'],
        edit: ['admin', 'capital:edit'],
        del: ['admin', 'capital:del']
      },
      rules: {
      }    }
  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }
  }
}
</script>

<style scoped>

</style>
