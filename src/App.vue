<template>
  <div>
    <el-form :model="dynamicValidateForm" ref="dynamicValidateForm" label-width="100px" class="demo-dynamic">
      <el-form-item
        prop="email"
        label="网站"
      >
        <el-input v-model="dynamicValidateForm.email"></el-input>
      </el-form-item>
      <el-form-item
        v-for="(domain, index) in dynamicValidateForm.domains"
        :label="'产品' + index"
        :key="domain.key"
        :prop="'domains.' + index + '.value'"
        :rules="{
      required: true, message: '不能为空', trigger: 'blur'
    }"
      >
        <el-input v-model="domain.value"></el-input><el-button @click.prevent="removeDomain(domain)">删除</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('dynamicValidateForm')">提交</el-button>
        <el-button @click="addDomain">新增产品</el-button>
        <el-button @click="resetForm('dynamicValidateForm')">重置</el-button>
      </el-form-item>
    </el-form>
    <el-input v-model="url"></el-input>
  </div>
</template>
<script>
export default {
  data () {
    return {
      url: '',
      dynamicValidateForm: {
        domains: [{
          value: ''
        }],
        email: 'https://www.babeside.com/collections/best-seller?sort_by='
      }
    }
  },
  methods: {
    submitForm (formName) {
      const that = this
      this.$refs[formName].validate((valid) => {
        if (valid) {
          that.url = that.dynamicValidateForm.email
          that.dynamicValidateForm.domains.map(function (value, index) {
            console.log(index)
            if (index !== that.dynamicValidateForm.domains.length - 1) {
              that.url += value.value + ','
            } else {
              that.url += value.value
            }
            return ''
          })
          that.url = that.url + '&q=' + that.turnTimestamp()
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
    removeDomain (item) {
      const index = this.dynamicValidateForm.domains.indexOf(item)
      if (index !== -1) {
        this.dynamicValidateForm.domains.splice(index, 1)
      }
    },
    addDomain () {
      this.dynamicValidateForm.domains.push({
        value: '',
        key: Date.now()
      })
    },
    turnTimestamp () {
      return Math.floor(Date.now() / 1000)
    }
  }
}
</script>
