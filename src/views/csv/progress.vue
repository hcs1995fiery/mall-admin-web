<template>
    <div>
      <el-button @click="visible = true">打开弹窗</el-button>
      <el-dialog
        title="获取FMEA"
        center
        :visible.sync="visible">
        <div class="dialog-wrap">
          <el-form ref="form" v-if="!showProgress" :model="form" :rules="rules" label-width="100px">
            <el-form-item label="链接地址" prop="url">
              <el-input v-model="form.url"></el-input>
            </el-form-item>
            <el-form-item style="text-align: center">
              <el-button @click="download" type="primary" size="medium">获取</el-button>
            </el-form-item>
          </el-form>
          <div v-if="showProgress">
            最新文件Excel
            <el-progress :percentage="progress" :status="progressStatus"></el-progress>
          </div>
        </div>
      </el-dialog>
    </div>
</template>

<script>
    export default {
      data () {
        return {
          visible: false,
          showProgress: false,
          progress: 0,
          completeDownload: false,
          progressStatus: '',
          form: {
            url: ''
          },
          rules: {
            url: [
              {required: true, message: '请输入链接地址', trigger: 'blur'}
            ]
          }
        }
      },
      methods: {
        download () {
          this.$refs['form'].validate((valid) => {
            if (valid) {
              this.showProgress = true
              setTimeout(() => {
                this.completeDownload = true
              }, 12000)
              let progress = setInterval(() => {
                if (this.progress < 99) {
                  this.progress++
                } else if (this.completeDownload) {
                  this.progress++
                  this.completeDownload = false
                  this.progressStatus = 'success'
                  clearInterval(progress)
                }
              }, 100)
            }
          })
        }
      }
    }
</script>

<style scoped>
  .dialog-wrap{
    min-height: 300px;
  }
</style>
