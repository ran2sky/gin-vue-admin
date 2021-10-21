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
        tooltip-effect="dark"
        row-key="ID"
      >
        >
        <el-table-column type="selection" width="55" />
        <el-table-column
          align="left"
          label="调度日期"
          prop="allocatDate"
          width="120"
        />
        <el-table-column
          align="left"
          label="调度车辆"
          prop="allocatCar"
          width="300"
        />
        <el-table-column
          align="left"
          label="供给目标"
          prop="allocatTarget"
          width="300"
        />
        <el-table-column
          align="left"
          label="调度事项"
          prop="allocatEvent"
          width="300"
        />
  
        <el-table-column
          align="left"
          label="调度状态"
          prop="allocatStatus"
          width="100"
        />

        <el-table-column align="left" label="操作" min-width="160">
          <template #default="scope">
            <el-button
              size="small"
              type="text"
              icon="el-icon-edit"
              @click="openEditDialog(scope.row, scope.$index)"
              >编辑</el-button
            >
            <el-popover
              :visible="scope.row.visible"
              placement="top"
              width="160"
            >
              <p>确定要删除吗？</p>
              <div style="text-align: right; margin-top: 8px;">
                <el-button
                  size="mini"
                  type="text"
                  @click="scope.row.visible = false"
                  >取消</el-button
                >
                <el-button
                  type="text"
                  size="mini"
                  @click="handleDelete(scope.$index)"
                  >确定</el-button
                >
              </div>
              <template #reference>
                <el-button type="text" icon="el-icon-delete" size="mini"
                  >删除</el-button
                >
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
    <div style="display:flex;margin-top:20px;align-items:center;justify-content:center">
      <img src="@/assets/resource/allocatmap.png" />
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
        :rule="rules"
      >
        <el-form-item label="时间" style="width:45%">
          <el-date-picker
            v-model="formData.workDate"
            value-format="yyyy-MM-DD"
          />
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
            >确 定</el-button
          >
          <el-button size="small" @click="closeDialog">取 消</el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>
<script>
import Image from '../../components/upload/image.vue';
export default {
  components: {Image},
  props: [],
  data() {
    return {
      formData: {
        workDate: undefined,
        fullName: undefined,
        event: undefined,
        eventlog: undefined,
      },
      tableData1: [
        {
          allocatId: '20211002001',
          allocatDate: "2021-10-02",
          allocatCar:'测试车辆：沪A899999',
          allocatEvent: "王小虎",
          allocatTarget: "上海市金山区",
          allocatStatus: "调度中",
        },
        {
          allocatId: '20211002001',
          allocatDate: "2021-10-02",
          allocatCar:'测试车辆：沪A899999',
          allocatEvent: "王小虎",
          allocatTarget: "上海市金山区",
          allocatStatus: "调度中",
        },
        {
          allocatId: '20211002001',
          allocatDate: "2021-10-02",
          allocatCar:'测试车辆：沪A899999',
          allocatEvent: "王小虎",
          allocatTarget: "上海市金山区",
          allocatStatus: "调度结束",
        },
        {
          allocatId: '20211002001',
          allocatDate: "2021-08-03  ",
          allocatCar:'测试车辆：沪A899999',
          allocatEvent: "王小虎",
          allocatTarget: "上海市金山区",
          allocatStatus: "调度结束",
        },
      ],
      rules: {
        workDate: [
          { required: true, message: "日期不能为空", trigger: "blur" },
        ],
        fullName: [
          { required: true, message: "姓名不能为空", trigger: "blur" },
        ],
        event: [{ required: true, message: "事件不能为空", trigger: "blur" }],
        eventlog: [
          { required: true, message: "事件日志不能为空", trigger: "blur" },
        ],
      },
      dialogFormVisible: false,
      type: "",
      listIndex: undefined,
    };
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {},
};
</script>
<style></style>
