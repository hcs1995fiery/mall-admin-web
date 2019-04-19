<template>
  <div>
    <el-button
      @click="transform">进行文件转化</el-button>
    <el-table
      :data="tableData">
      <el-table-column
        v-for="(item, index) in tableConfig"
        :key="index"
        :prop="Object.keys(item)[0]"
        :label="item[index]">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
    import Papaparse from 'papaparse'
    export default {
      data () {
        return {
          tableData: [],
          tableConfig: []
        }
      },
      methods: {
        transform () {
          Papaparse.parse('./20180803_rty_bus.csv', {
            download: true,
            complete: (result) => {
              console.log('result', result)
              let data = JSON.parse(JSON.stringify(result.data))
              this.tableConfig = data[0].map((item, index) => {
                return {[index]: item}
              })
              data.shift()
              this.tableData = data.map(row => {
                let tableRow = {}
                row.map((item, index) => {
                  tableRow[index] = item
                })
                return tableRow
              })
              console.log('this.tableData', this.tableData)
            }
          })
        }
      },
      mounted() {
        // this.getConfig()
      }
    }
</script>

<style scoped>

</style>
