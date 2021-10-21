<template>
  <div>
	  <div>
    <div class="gva-table-box">
      <div class="gva-btn-list">
        <el-button size="mini" type="primary" icon="el-icon-plus" @click="openDialog">新增</el-button>
        <el-button size="mini" type="warning" icon="el-icon-edit" @click="openDialog">编辑</el-button>
      </div>
      <el-row :gutter="15">
      <el-form ref="elForm" :model="formData" :rules="rules" size="medium" label-width="80px" label-position="left">
        <el-col :span="12">
          <el-form-item label="行政区域" prop="field101" >
            <el-select v-model="formData.field101" placeholder="请选择行政区域" clearable :style="{width: '100%'}">
              <el-option v-for="(item, index) in regionOptions" :key="index" :label="item.label"
                :value="item.value" :disabled="item.disabled"></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="部门" prop="field108" label-width="50px">
            <el-select v-model="formData.field108" placeholder="请选择部门" clearable :style="{width: '100%'}">
              <el-option v-for="(item, index) in depOptions" :key="index" :label="item.label"
                :value="item.value" :disabled="item.disabled"></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="3">
          <el-form-item label-width="0" prop="field109">
            <el-button type="warning" icon="el-icon-search" size="small">查询</el-button>
          </el-form-item>
        </el-col>
      </el-form>
    </el-row>
      <el-table
        ref="multipleTable"
        :data="tableData1"
        style="width: 100%"
        tooltip-effect="dark"
        @selection-change="handelSelectionChang"
        row-key="ID"
      >
        <el-table-column type="selection" width="55" />
        <el-table-column align="left" label="姓名" prop="name" width="120" />
        <el-table-column align="left" label="电话" prop="phonenumber" width="120" />
        <el-table-column align="left" label="行政区域" prop="region" width="120" />
        <el-table-column align="left" label="部门" prop="dept" width="120" />
        <el-table-column align="left" label="操作" min-width="160">
          <template #default="scope">
            <el-button size="small"  type="success" icon="el-icon-phone" @click="openCallDialog">语音呼叫</el-button>
            <!-- <el-popover :visible="scope.row.visible" placement="top" width="160">
              <p>确定要删除吗？</p>
              <div style="text-align: right; margin-top: 8px;">
                <el-button size="mini" type="text" @click="scope.row.visible = false">取消</el-button>
                <el-button type="primary" size="mini" @click="deleteCustomer(scope.row)">确定</el-button>
              </div>
              <template #reference>
                <el-button type="text" icon="el-icon-delete" size="mini">删除</el-button>
              </template>
            </el-popover> -->
          </template>
        </el-table-column>
      </el-table>
      <div class="gva-pagination">
        <el-pagination
          :current-page="page"
          :page-size="pageSize"
          :page-sizes="[10, 30, 50, 100]"
          :total="total"
          layout="total, sizes, prev, pager, next, jumper"
          @current-change="handleCurrentChange"
          @size-change="handleSizeChange"
        />
      </div>
    </div>
    <el-dialog v-model="dialogFormVisible" :before-close="closeDialog" title="客户">
      <el-form :inline="true" :model="form" label-width="80px">
        <el-form-item label="客户名">
          <el-input v-model="form.customerName" autocomplete="off" />
        </el-form-item>
        <el-form-item label="客户电话">
          <el-input v-model="form.customerPhoneData" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
        <div class="dialog-footer">
          <el-button size="small" @click="closeDialog">取 消</el-button>
          <el-button size="small" type="primary" @click="enterDialog">确 定</el-button>
        </div>
      </template>
    </el-dialog>
    <el-dialog v-model="dialogCallVisible" :before-close="closeCallDialog" title="语音呼叫" width="500px">
      <el-form :inline="false">
        <el-form-item style="margin-left:120px">
          <video ref="myVideo" src="/src/assets/video/calldemo.mp4" width="240" height="480" loop/>
        </el-form-item>
        <el-form-item style="margin-left:200px">
          <el-button size="small" type="danger" icon="el-icon-phone" @click="closeCallDialog">挂断</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
  </div>
</template>
<script>
import {
  createExaCustomer,
  updateExaCustomer,
  deleteExaCustomer,
  getExaCustomer,
  getExaCustomerList
} from '@/api/customer'
import infoList from '@/mixins/infoList'
import warningBar from '@/components/warningBar/warningBar.vue'
export default {
  name: 'Customer',
  components: { warningBar },
  mixins: [infoList],
  props: [],
  data() {
    return {
      formData: {
        field101: 1,
        field108: 1,
        field109: undefined,
        field102: undefined,
      },
      rules: {
        field101: [{
          required: true,
          message: '请选择行政区域',
          trigger: 'change'
        }],
        field108: [{
          required: true,
          message: '请选择部门',
          trigger: 'change'
        }],
      },
      regionOptions: [{
        "label": "静安区",
        "value": 1
      }, {
        "label": "长宁区",
        "value": 2
      }, {
        "label": "宝山区",
        "value": 3
      }, {
        "label": "浦东新区",
        "value": 4
      }],
      depOptions: [{
        "label": "交通管理部",
        "value": 1
      }, {
        "label": "生产安全部",
        "value": 2
      }],
      tableData1: [{
            userId: '01',
            name: '王小虎',
            region: "静安区",
            dept: "生产安全部",
            phonenumber: "13029201910"
          }, {
            userId: '02',
            name: '赵小飞',
            region: "宝山区",
            dept: "交通管理部",
            phonenumber: "15019029934"
          }, {
            userId: "03",
            name: '刘小东',
            region: "长宁区",
            dept: "交通管理部",
            phonenumber: "18993927171"
          }, {
            userId: "04",
            name: '张小牛',
            region: "长宁区",
            dept: "生产安全部",
            phonenumber: "13802931087"
          }],
      listApi: getExaCustomerList,
      dialogFormVisible: false,
      dialogCallVisible: false,
      type: '',
      form: {
        customerName: '',
        customerPhoneData: ''
      }
    }
  },
  computed: {},
  watch: {},
  created() {
     this.getTableData()
  },
  mounted() {},
  methods: {
    submitForm() {
      this.$refs['elForm'].validate(valid => {
        if (!valid) return
        // TODO 提交表单
      })
    },
    resetForm() {
      this.$refs['elForm'].resetFields()
    },
    async updateCustomer(row) {
      const res = await getExaCustomer({ ID: row.ID })
      this.type = 'update'
      if (res.code === 0) {
        this.form = res.data.customer
        this.dialogFormVisible = true
      }
    },
    closeDialog() {
      this.dialogFormVisible = false
      this.form = {
        customerName: '',
        customerPhoneData: ''
      }
    },
    closeCallDialog() {
      this.dialogCallVisible = false
      this.$nextTick(() => {
        this.$refs.myVideo.currentTime = 0
        this.$refs.myVideo.pause();
      })
       
    },
    async deleteCustomer(row) {
      row.visible = false
      const res = await deleteExaCustomer({ ID: row.ID })
      if (res.code === 0) {
        this.$message({
          type: 'success',
          message: '删除成功'
        })
        if (this.tableData.length === 1 && this.page > 1) {
          this.page--
        }
        this.getTableData()
      }
    },
    async enterDialog() {
      let res
      switch (this.type) {
        case 'create':
          res = await createExaCustomer(this.form)
          break
        case 'update':
          res = await updateExaCustomer(this.form)
          break
        default:
          res = await createExaCustomer(this.form)
          break
      }

      if (res.code === 0) {
        this.closeDialog()
        this.getTableData()
      }
    },
    openDialog() {
      this.type = 'create'
      this.dialogFormVisible = true
    },
    openCallDialog() {
      this.dialogCallVisible = true
      this.$nextTick(function(){
            this.$refs.myVideo.play();
      })
        
  }
}
}

</script>
<style>
</style>
