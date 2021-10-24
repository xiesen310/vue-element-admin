<template>
  <div class="custom-theme" style="width: 90%;margin-left: 5%">
    <h2>创建数据库</h2>
    <el-form
      ref="dynamicValidateForm"
      :model="database"
      label-width="100px"
      class="demo-dynamic"
    >

      <el-form-item
        prop="databaseName"
        label="数据库名"
        :rules="[
          { required: true, message: '请输入数据库名称', trigger: 'blur' }
        ]"
        style="margin-bottom: 20px"
      >
        <el-input v-model="database.databaseName" />
      </el-form-item>

      <el-form-item label="数据库引擎">
        <el-select v-model="database.engineValue" filterable placeholder="请选择数据库引擎" style="margin-bottom: 20px">
          <el-option
            v-for="item in database.engines"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm('database')">确定</el-button>
        <el-button @click="resetForm('database')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      database: {
        databaseName: '',
        engines: [{
          value: 'Atomic',
          label: 'Atomic'
        }, {
          value: 'MySQL',
          label: 'MySQL'
        }, {
          value: 'PostgreSQL',
          label: 'PostgreSQL'
        }],
        engineValue: 'Atomic'
      }
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    },
    validateDatabaseName() {
      console.log(this.database.databaseName)
      var reg = /^[A-Za-z0-9]{1,30}$/
      if (!reg.test(this.database.databaseName)) {
        this.$message.error('数据库名称只能是字符或者数字')
        this.database.databaseName = ''
      }
    }
  }
}
</script>
