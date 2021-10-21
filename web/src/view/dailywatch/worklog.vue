<template>
  <div>
    <div class="gva-table-box">
      <div class="gva-btn-list">
        <el-button
          size="mini"
          type="primary"
          icon="el-icon-plus"
          @click="openDialog"
          >新增</el-button
        >
      </div>
      <el-row :gutter="15"> </el-row>
      <el-table
        ref="multipleTable"
        :data="tableData1"
        style="width: 100%"
        :header-cell-style="{'text-align':'center'}"
        :cell-style="{'text-align':'center'}"
        tooltip-effect="dark"
        row-key="ID">
      >
        <el-table-column type="selection" width="55" />
        <el-table-column
          align="left"
          label="值班日期"
          prop="workDate"
          width="120"
        />
        <el-table-column align="left" label="姓名" prop="fullName" width="120" />
        <el-table-column
          align="left"
          label="交接事项"
          prop="event"
          width="350"
        />
        <el-table-column
          align="left"
          label="工作日志"
          prop="eventlog"
          width="350"
        />
        <el-table-column align="left" label="操作" min-width="160">
          <template #default="scope">
            <el-button size="small"  type="text" icon="el-icon-edit" @click="openEditDialog(scope.row, scope.$index)">编辑</el-button>
            <el-popover :visible="scope.row.visible" placement="top" width="160">
              <p>确定要删除吗？</p>
              <div style="text-align: right; margin-top: 8px;">
                <el-button size="mini" type="text" @click="scope.row.visible = false">取消</el-button>
                <el-button type="text" size="mini" @click="handleDelete(scope.$index)">确定</el-button>
              </div>
              <template #reference>
                <el-button type="text" icon="el-icon-delete" size="mini">删除</el-button>
              </template>
            </el-popover>
          </template>
        </el-table-column>
      </el-table>
      <!-- <div class="gva-pagination">
        <el-pagination
          :current-page="page"
          :page-size="pageSize"
          :page-sizes="[10, 30, 50, 100]"
          :total="total"
          layout="total, sizes, prev, pager, next, jumper"
        />
      </div> -->
    </div>
    <el-dialog
      v-model="dialogFormVisible"
      :before-close="closeDialog"
      title="工作日志明细"
      width="40%"
    >
      <el-form 
          :inline="false" 
          :model="formData" 
          label-width="80px"
          ref="ruleForm"
          :rule="rules">
        <el-form-item label="时间" style="width:45%">
          <el-date-picker
          v-model="formData.workDate"
          value-format="yyyy-MM-DD" />
        </el-form-item>
        <el-form-item label="姓名" style="width:33%">
          <el-input v-model="formData.fullName" autocomplete="off" />
        </el-form-item>
        <el-form-item label="交接事项" style="width:50%">
          <el-input v-model="formData.event" autocomplete="off" />
        </el-form-item>
        <el-form-item label="工作日志" style="width:50%">
          <el-input
            type="textarea"
            v-model="formData.eventlog"
            autocomplete="off"
          />
        </el-form-item>
      </el-form>
      <template #footer>
        <div class="dialog-footer">
          <el-button size="small" type="primary" @click="submitForm"
            >确 定</el-button>
          <el-button size="small" @click="closeDialog">取 消</el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>
<script>
import { formatTimeToStr } from '@/utils/date.js'

export default {
  props: [],
  data() {
    return {
      formData: {
        workDate: undefined,
        fullName: undefined,
        event: undefined,
        eventlog: undefined
      },
      tableData1: [
        {
          workDate: "2021-10-10",
          fullName: "王小虎",
          event: "生产企业后续工作处理",
          eventlog: "本日工作无异常",
        },
        {
          workDate: "2021-10-10",
          fullName: "王小虎",
          event: "近期多雨，需注意市区积水情况",
          eventlog: "本日值班无异常",
        },
      ],
      rules: {
        workDate: [
          {required: true, message: "日期不能为空", trigger: 'blur'},
        ],
        fullName: [
          {required: true, message: "姓名不能为空", trigger: 'blur'},
        ],
        event: [
          {required: true, message: "事件不能为空", trigger: 'blur'},
        ],
        eventlog: [
          {required: true, message: "事件日志不能为空", trigger: 'blur'},
        ]
      },
      dialogFormVisible: false,
      type: "",
      listIndex: undefined
    };
  },
  computed: {},
  watch: {},
  created() {
  },
  mounted() {},
  methods: {
    submitForm() {
      this.$refs["ruleForm"].validate((valid) => {
        if (valid) {
          if(this.listIndex) {
            const _index = this.listIndex
            
            this.tableData1[_index] = this.formData
            this.dialogFormVisible = false
          } else {
            if(this.formData)
              this.formData.workDate = formatTimeToStr(this.formData.workDate,"yyyy-MM-dd")
              this.tableData1.push(this.formData)
          }
        }
      else  {
        this.$message({
          message:"请输入相应内容",
          type:"error"
        })
      }
        return;
        // TODO 提交表单
      });


    },
    resetForm() {
      this.$refs["ruleForm"].resetFields();
    },
    openEditDialog(row, index) {
      this.type = "update";
      this.dialogFormVisible = true;
      this.$nextTick(() => {
        this.listIndex = index
        this.formData.workDate = row.workDate
        this.formData.fullName = row.fullName
        this.formData.event = row.event
        this.formData.eventlog = row.eventlog
      })


    },
    closeDialog() {
      this.dialogFormVisible = false;
      setTimeout(() => {
        this.formData = []
        this.listIndex = ''
        this.$refs["ruleForm"].resetFields()
      }, 200)

    },
    handleDelete(index) {
      this.tableData1.splice(index, 1)
    },
    openDialog() {
      this.type = "create";
      this.dialogFormVisible = true;
    },
  },
};
</script>
<style></style>
