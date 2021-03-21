<template>
  <div class="app-container">
    <upload-excel-component :on-success="handleSuccess" :before-upload="beforeUpload" />
    <el-table :data="tableData" border highlight-current-row style="width: 100%;margin-top:20px;">
      <el-table-column v-for="item of tableHeader" :key="item" :prop="item" :label="item" />
    </el-table>
  </div>
</template>

<script>
import UploadExcelComponent from '@/components/UploadExcel/index.vue'
import { getToken } from '@/utils/auth'
export default {
  name: 'UploadExcel',
  components: { UploadExcelComponent },
  data() {
    return {
      tableData: [],
      tableHeader: [],
      headers: {
        'Authorization': getToken()
      }
    }
  },
  methods: {
    beforeUpload(file) {
      const isLt1M = file.size / 1024 / 1024 < 1
      if (isLt1M) {
        return true
      }

      this.$message({
        message: '请不要上传大于1m的文件.',
        type: 'warning'
      })
      return false
    },
    handleSuccess({ results, header }) {
      this.tableData = results
      this.tableHeader = header
      this.url = '/api/capital/import'
      const config = {
        headers: { 'Authorization': getToken() }
      }
      const params = new URLSearchParams()
      params.append('file', JSON.stringify(results))
      console.log(JSON.stringify(results))
      this.$axios.post(this.url, params, config).then(res => {
        const rest = res.data.data
        console.log(rest)
      }).catch((e) => {
        this.$message.error('excel上传失败，请重新上传')
      }).finally(() => {

      })
    }
  }
}
</script>
