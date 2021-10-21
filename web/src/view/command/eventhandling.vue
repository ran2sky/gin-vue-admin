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
          label="记录日期"
          prop="recordDate"
          width="100"
        />
        <el-table-column
          align="left"
          label="事件地点"
          prop="eventLocation"
          width="350"
        />
        <el-table-column
          align="left"
          label="事件人员"
          prop="eventPeople"
          width="200"
        />
        <el-table-column
          align="left"
          label="事件单位"
          prop="eventGroup"
          width="200"
        />
                <el-table-column
          align="left"
          label="事件内容"
          prop="eventContent"
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
          v-model="formData.recordDate"
          value-format="yyyy-MM-DD" />
        </el-form-item>
        <el-form-item label="姓名" style="width:33%">
          <el-input v-model="formData.eventLocation" autocomplete="off" />
        </el-form-item>
        <el-form-item label="交接事项" style="width:50%">
          <el-input v-model="formData.eventPeople" autocomplete="off" />
        </el-form-item>
        <el-form-item label="工作日志" style="width:50%">
          <el-input
            type="textarea"
            v-model="formData.eventGroup"
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
        recordDate: undefined,
        eventLocation: undefined,
        eventPeople: undefined,
        eventGroup: undefined,
        eventDevice: undefined,
        eventContent: undefined
      },
      tableData1: [
        {
          recordDate: "2021-08-12",
          eventLocation: "资五路苏仙段项目经理部生活区",
          eventPeople: "应急小组全体人员",
          eventGroup: "生产力促进中心",
          eventDevice: '无',
          eventContent:"部分地区大楼突然停电"
        },
        {
          recordDate: "2021-04-28",
          eventLocation: "斜井二中段运输巷道内",
          eventPeople: "救援小组",
          eventGroup: "市应急管理局",
          eventDevice: '无',
          eventContent:"斜井二中段运输巷道内电缆绝缘损坏，发生火灾。"
        },
                {
          recordDate: "2021-06-25",
          eventLocation: "广东白云学院北校区",
          eventPeople: "冼土培、冼土贵",
          eventGroup: "消防局应急中心",
          eventDevice: '无',
          eventContent:"发现施工现场内一处起火，立即查看并报告总指挥冼土培，总指挥下达启动应急救援指令，让冼土贵通知所有参加义务消防队员在火警附近集合进行应急处理。"
        },
      ],
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
              this.formData.recordDate = formatTimeToStr(this.formData.recordDate,"yyyy-MM-dd")
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
        this.formData.recordDate = row.recordDate
        this.formData.eventLocation = row.eventLocation
        this.formData.eventPeople = row.eventPeople
        this.formData.eventGroup = row.eventGroup
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
