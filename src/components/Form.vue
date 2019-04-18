<template>
  <div style="background:#f0f2f5;padding:16px 16px 16px;height:890px" >
    <el-row style="background:#fff;">
      <el-form ref="postForm" :model="postForm" :rules="rules">

        <div v-for="item in config" :key="item.name">
          <el-row>
            <el-col :span="7">
              {{ item.name }}
            </el-col>
          </el-row>

          <div v-for="field in item.fields" :key="field.columnName">
            <el-row>
              <el-col :span="7">
                <el-form-item :label="field.showName+':'" :prop="field.columnName" label-width="130px">
                  <el-input v-model="field.value" style="width:200px" :maxlength="field.maxLength" />
                </el-form-item>
              </el-col>
            </el-row>
          </div>

        </div>

        <el-row>
          <el-col :span="7">
            <el-form-item label="" prop="" label-width="130px">
              <el-button type="primary" @click="onSubmit">保存</el-button>
              <el-button >取消</el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </el-row>
  </div>
</template>

<script>

export default {
  name: 'CreateForm',
  components: { },
  props: {
    type: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      config: [{
        name: '软件配置',
        type: 1,
        fields: [{
          showName: '软件名称',
          columnName: 'appName',
          columnType: 1,
          maxLength: 8,
          ifShow: 1,
          defaultValue: ''
        },
        {
          showName: '软件状态',
          columnName: 'status',
          columnType: 4,
          defaultValue: 1,
          ifShow: 1,
          options: [{
            key: '启用',
            value: 1
          }, {
            key: '停用',
            value: 2
          }, {
            key: '维护',
            value: 3
          }, {
            key: '仅查看',
            value: 4
          }]
        }
        ]
      }],
      rules: {},
      postForm: {}
    }
  },
  created () {
    this.initPostForm()
  },
  methods: {
    initPostForm () {
      for (let i = 0; i < this.config.length; i++) {
        for (let j = 0; j < this.config[i].fields.length; j++) {
          this.$set(this.postForm, this.config[i].fields[j].columnName, this.config[i].fields[j].defaultValue)
          this.$set(this.rules, this.config[i].fields[j].columnName, [{ required: true, message: '请输入' + this.config[i].fields[j].showName, trigger: 'blur' }])
        }
      }
    },
    onSubmit () {
      for (let i = 0; i < this.config.length; i++) {
        for (let j = 0; j < this.config[i].fields.length; j++) {
          this.$set(this.postForm, this.config[i].fields[j].columnName, this.config[i].fields[j].value)
        }
      }
      this.$refs.postForm.validate(valid => {
        if (valid) {
          console.log(1)
        } else {
          return false
        }
      })
    }

  }

}
</script>
