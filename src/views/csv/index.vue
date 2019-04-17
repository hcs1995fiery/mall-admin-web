<template>
  <div>
    <el-button
      @click="transform">进行文件转化</el-button>
    <el-table
      :data="tableData">
      <el-table-column
        v-for="(item, index) in tableConfig"
        :key="index"
        :prop="item"
        :label="item">
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
        getConfig () {
          this.tableConfig = Object.keys(this.tableData[0])
        },
        transform () {
          Papaparse.parse('./20180803_rty_bus.csv', {
            download: true,
            complete: (result) => {
              console.log('r=>', result)
              let data = JSON.parse(JSON.stringify(result.data))
              // let data = result.data
              // let r = {data: [1,2,3,4,5]}
              // let data = JSON.parse(JSON.stringify(r.data))
              // console.log('rd')
              this.tableConfig = data[0]
              console.log('tC=>', typeof this.tableConfig)
              // data.shift()
              console.log('data=>', data)
              data.shift()
              // data = 1
              console.log('d==>', data)
              console.log('TC=>', this.tableConfig)
            }
          })
        }
      },
      mounted() {
        this.getConfig()
      }
    }
</script>

<style scoped>

</style>
