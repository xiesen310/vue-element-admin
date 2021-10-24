<template>
  <div class="custom-theme" style="width: 90%;margin-left: 5%">
    <h2>创建表</h2>
    <el-form
      ref="dynamicValidateForm"
      :model="dynamicValidateForm"
      label-width="100px"
      class="demo-dynamic"
    >
      <div class="el-form-item">
        <el-form-item
          prop="tableName"
          label="表名"
          :rules="[
            { required: true, message: '请输入数据库表名', trigger: 'blur' }
          ]"
          style="margin-bottom: 20px"
        >
          <el-input v-model="dynamicValidateForm.clusterName" />
        </el-form-item>
        <el-form-item
          prop="clusterName"
          label="集群名称"
          :rules="[
            { required: true, message: '请输入集群名称', trigger: 'blur' }
          ]"
        >
          <el-input v-model="dynamicValidateForm.tableName" />
        </el-form-item>
      </div>

      <el-form-item
        v-for="(domain, index) in dynamicValidateForm.domains"
        :key="domain.key"
        :label="'字段' + index"
        :prop="'domains.' + index + '.value'"
        :rules="{
          required: true, message: '字段不能为空', trigger: 'blur'
        }"
      >
        <div style="display: inline">
          <el-input v-model="domain.value" placeholder="请输入字段名称" style="width: 30%;margin-right: 10px;" />
          <el-input v-model="domain.value" placeholder="请输入字段描述" style="width: 30%;margin-right: 10px;" />
          <el-select v-model="fieldValue" filterable placeholder="请选择数据类型" style="width: 15%;margin-right: 10px">
            <el-option
              v-for="item in dynamicValidateForm.fieldTypes"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
          <el-button style="width: 10%" @click="addFields">新增字段</el-button>
          <el-button type="danger" icon="el-icon-delete" circle @click.passive="removeDomain(domain)" />
        </div>
      </el-form-item>

      <el-form-item label="表引擎">
        <el-select v-model="engineValue" filterable placeholder="请选择表引擎" style="margin-bottom: 20px">
          <el-option
            v-for="item in dynamicValidateForm.engines"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="表描述" style="width: 50%">
        <el-input v-model="tableComment" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('dynamicValidateForm')">创建</el-button>
        <el-button @click="resetForm('dynamicValidateForm')">重置</el-button>
      </el-form-item>

    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dynamicValidateForm: {
        domains: [{
          value: ''
        }],
        tableName: '',
        clusterName: '',
        fieldTypes: [{
          value: 'String',
          label: 'String'
        }, {
          value: 'Int32',
          label: 'Int32'
        }, {
          value: 'Int64',
          label: 'Int64'
        }, {
          value: 'Boolean',
          label: 'Boolean'
        }, {
          value: 'Date',
          label: 'Date'
        }, {
          value: 'DateTime',
          label: 'DateTime'
        }],
        engines: [{
          value: 'MergeTree',
          label: 'MergeTree'
        }, {
          value: 'ReplacingMergeTree',
          label: 'ReplacingMergeTree'
        }]
      },
      fieldValue: '',
      engineValue: '',
      tableComment: ''
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
    removeDomain(item) {
      var index = this.dynamicValidateForm.domains.indexOf(item)
      if (index !== -1) {
        this.dynamicValidateForm.domains.splice(index, 1)
      }
    },
    addFields() {
      this.dynamicValidateForm.domains.push({
        value: '',
        key: Date.now()
      })
    }
  }
}
</script>
