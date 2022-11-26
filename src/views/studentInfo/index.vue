<template>
  <div class="app-container">
    <div class="filter-container">
      <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="EditPen" @click="openCreate">
        新增
      </el-button>
    </div>

    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="stuId" label="编号" />
      <el-table-column prop="stuNo" label="学号" />
      <el-table-column prop="stuName" label="姓名" />
      <el-table-column prop="stuAge" label="年龄" />
      <el-table-column prop="stuSex" label="性别" />
      <el-table-column prop="stuBirthday" label="生日" />
      <el-table-column prop="stuAddress" label="家庭住址" />
      <el-table-column align="right" label="操作">
        <template #default="scope">
          <el-button size="small" @click="updateEdit(scope.$index, scope.row)">修改</el-button>
          <el-button size="small" type="danger" @click="deleteRow(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog v-model="dialogVisible" :title="titleMap[titleStatus]" width="30%" :before-close="handleClose">
      <el-form ref="ruleFormRef" :model="ruleForm" :rules="rules" label-width="120px" class="demo-ruleForm"
        :size="formSize" status-icon>
        <el-form-item label="Activity name" prop="name">
          <el-input v-model="ruleForm.name" />
        </el-form-item>
        <el-form-item label="Activity zone" prop="region">
          <el-select v-model="ruleForm.region" placeholder="Activity zone">
            <el-option label="Zone one" value="shanghai" />
            <el-option label="Zone two" value="beijing" />
          </el-select>
        </el-form-item>
        <el-form-item label="Activity count" prop="count">
          <el-select-v2 v-model="ruleForm.count" placeholder="Activity count" :options="options" />
        </el-form-item>
        <el-form-item label="Activity time" required>
          <el-col :span="11">
            <el-form-item prop="date1">
              <el-date-picker v-model="ruleForm.date1" type="date" label="Pick a date" placeholder="Pick a date"
                style="width: 100%" />
            </el-form-item>
          </el-col>
          <el-col class="text-center" :span="2">
            <span class="text-gray-500">-</span>
          </el-col>
          <el-col :span="11">
            <el-form-item prop="date2">
              <el-time-picker v-model="ruleForm.date2" label="Pick a time" placeholder="Pick a time"
                style="width: 100%" />
            </el-form-item>
          </el-col>
        </el-form-item>
        <el-form-item label="Instant delivery" prop="delivery">
          <el-switch v-model="ruleForm.delivery" />
        </el-form-item>
        <el-form-item label="Activity type" prop="type">
          <el-checkbox-group v-model="ruleForm.type">
            <el-checkbox label="Online activities" name="type" />
            <el-checkbox label="Promotion activities" name="type" />
            <el-checkbox label="Offline activities" name="type" />
            <el-checkbox label="Simple brand exposure" name="type" />
          </el-checkbox-group>
        </el-form-item>
        <el-form-item label="Resources" prop="resource">
          <el-radio-group v-model="ruleForm.resource">
            <el-radio label="Sponsorship" />
            <el-radio label="Venue" />
          </el-radio-group>
        </el-form-item>
        <el-form-item label="Activity form" prop="desc">
          <el-input v-model="ruleForm.desc" type="textarea" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm(ruleFormRef)">Create</el-button>
          <el-button @click="resetForm(ruleFormRef)">Reset</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script lang="ts">
import { reactive, ref } from 'vue'
import { ElMessageBox, FormRules, FormInstance } from 'element-plus'
export default {
  data() {
    return {
      tableData: [
        {
          stuId: '1',
          stuNo: '001',
          stuName: '张三',
          stuAge: '18',
          stuSex: '0',
          stuBirthday: '2014-02-06',
          stuAddress: 'No. 189, Grove St, Los Angeles'
        },
        {
          stuId: '2',
          stuNo: '002',
          stuName: '李四',
          stuAge: '17',
          stuSex: '0',
          stuBirthday: '2013-02-06',
          stuAddress: 'No. 189, Grove St, Los Angeles'
        },
        {
          stuId: '3',
          stuNo: '003',
          stuName: '李雷',
          stuAge: '15',
          stuSex: '0',
          stuBirthday: '2012-02-06',
          stuAddress: 'No. 189, Grove St, Los Angeles'
        },
        {
          stuId: '4',
          stuNo: '004',
          stuName: '韩梅梅',
          stuAge: '15',
          stuSex: '1',
          stuBirthday: '2012-01-06',
          stuAddress: 'No. 189, Grove St, Los Angeles'
        }
      ],
      ruleForm: reactive({
        name: 'Hello',
        region: '',
        count: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }),
      ruleFormRef: ref<FormInstance>(),
      rules: reactive<FormRules>({
        name: [
          { required: true, message: 'Please input Activity name', trigger: 'blur' },
          { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' },
        ],
        region: [
          {
            required: true,
            message: 'Please select Activity zone',
            trigger: 'change',
          },
        ],
        count: [
          {
            required: true,
            message: 'Please select Activity count',
            trigger: 'change',
          },
        ],
        date1: [
          {
            type: 'date',
            required: true,
            message: 'Please pick a date',
            trigger: 'change',
          },
        ],
        date2: [
          {
            type: 'date',
            required: true,
            message: 'Please pick a time',
            trigger: 'change',
          },
        ],
        type: [
          {
            type: 'array',
            required: true,
            message: 'Please select at least one activity type',
            trigger: 'change',
          },
        ],
        resource: [
          {
            required: true,
            message: 'Please select activity resource',
            trigger: 'change',
          },
        ],
        desc: [
          { required: true, message: 'Please input activity form', trigger: 'blur' },
        ],
      }),
      formSize: ref('default'),
      options: Array.from({ length: 10000 }).map((_, idx) => ({
        value: `${idx + 1}`,
        label: `${idx + 1}`,
      })),
      dialogVisible: false,
      titleStatus: '',
      titleMap: {
        update: '编辑',
        create: '新增'
      }
    }
  },
  methods: {
    handleClose: (done: () => void) => {
      ElMessageBox.confirm('Are you sure to close this dialog?')
        .then(() => {
          done()
        })
        .catch(() => {
          // catch error
        })
    },
    submitForm: async (formEl: FormInstance | undefined) => {
      console.log("11111111111");
      if (!formEl) return
      await formEl.validate((valid, fields) => {
        if (valid) {
          console.log('submit!')
        } else {
          console.log('error submit!', fields)
        }
      })
    },
    resetForm: (formEl: FormInstance | undefined) => {
      console.log("22222222222");
      if (!formEl) return
      formEl.resetFields()
    },
    openCreate() {
      this.titleStatus = 'create'
      this.dialogVisible = true
    },
    updateEdit() {

    }
  }
}
</script>
<style scoped>
.dialog-footer button:first-child {
  margin-right: 10px;
}
</style>
