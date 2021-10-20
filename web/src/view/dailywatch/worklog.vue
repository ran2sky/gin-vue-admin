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
        <el-button
          size="mini"
          type="warning"
          icon="el-icon-edit"
          @click="openDialog"
          >编辑</el-button
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
        <el-table-column type="selection" width="55" />
        <el-table-column
          align="left"
          label="值班日期"
          prop="workDate"
          width="120"
        />
        <el-table-column align="left" label="姓名" prop="name" width="120" />
        <el-table-column
          align="left"
          label="交接事项"
          prop="event"
          width="240"
        />
        <el-table-column
          align="left"
          label="工作日志"
          prop="eventlog"
          width="300"
        />
        <el-table-column label="操作" align="center">
          <template slot-scope="{ row, $index }">
            <el-button
              v-if="!showBtn[$index]"
              type="primary"
              size="mini"
              @click.native="handleEdit($index, row)"
              >编辑</el-button
            >
            <el-button
              v-if="showBtn[$index]"
              type="primary"
              size="mini"
              @click.native="handleCancel($index, row)"
              >取消</el-button
            >
          </template>
        </el-table-column>
        <!-- <el-table-column align="left" label="操作" min-width="160">
          <template #default="scope">
            <el-button size="small"  type="text" icon="el-icon-edit" @click="updateCustomer(scope.row)">编辑</el-button>
            <el-popover :visible="scope.row.visible" placement="top" width="160">
              <p>确定要删除吗？</p>
              <div style="text-align: right; margin-top: 8px;">
                <el-button size="mini" type="text" @click="scope.row.visible = false">取消</el-button>
                <el-button type="text" size="mini" @click="deleteCustomer(scope.row)">确定</el-button>
              </div>
              <template #reference>
                <el-button type="text" icon="el-icon-delete" size="mini">删除</el-button>
              </template>
            </el-popover>
          </template>
        </el-table-column> -->
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
    <el-dialog
      v-model="dialogFormVisible"
      :before-close="closeDialog"
      title="客户"
    >
      <el-form :inline="false" :model="form" label-width="80px">
        <el-form-item label="时间" style="width:45%">
          <el-date-picker v-model="form.customerName" />
        </el-form-item>
        <el-form-item label="姓名" style="width:33%">
          <el-input v-model="form.customerPhoneData" autocomplete="off" />
        </el-form-item>
        <el-form-item label="交接事项" style="width:50%">
          <el-input v-model="form.customerPhoneData" autocomplete="off" />
        </el-form-item>
        <el-form-item label="工作日志" style="width:50%">
          <el-input
            type="textarea"
            v-model="form.customerPhoneData"
            autocomplete="off"
          />
        </el-form-item>
      </el-form>
      <template #footer>
        <div class="dialog-footer">
          <el-button size="small" @click="closeDialog">取 消</el-button>
          <el-button size="small" type="primary" @click="enterDialog"
            >确 定</el-button
          >
        </div>
      </template>
    </el-dialog>
  </div>
</template>
<script>
import {
  createExaCustomer,
  updateExaCustomer,
  deleteExaCustomer,
  getExaCustomer,
  getExaCustomerList,
} from "@/api/customer";
import infoList from "@/mixins/infoList";
import warningBar from "@/components/warningBar/warningBar.vue";
export default {
  name: "Customer",
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
      tableData1: [
        {
          workDate: "2021-10-10",
          name: "王小虎",
          event: "生产企业后续工作处理",
          eventlog: "本日工作无异常",
        },
        {
          workDate: "2021-10-10",
          name: "王小虎",
          event: "近期多雨，需注意市区积水情况",
          eventlog: "本日值班无异常",
        },
      ],
      listApi: getExaCustomerList,
      dialogFormVisible: false,
      type: "",
      form: {
        customerName: "",
        customerPhoneData: "",
      },
      showEdit: [],
      showBtn: [],
    };
  },
  computed: {},
  watch: {},
  created() {
    this.getTableData();
  },
  mounted() {},
  methods: {
    submitForm() {
      this.$refs["elForm"].validate((valid) => {
        if (!valid) return;
        // TODO 提交表单
      });
    },
    resetForm() {
      this.$refs["elForm"].resetFields();
    },
    async updateCustomer(row) {
      const res = await getExaCustomer({ ID: row.ID });
      this.type = "update";
      if (res.code === 0) {
        this.form = res.data.customer;
        this.dialogFormVisible = true;
      }
    },
    closeDialog() {
      this.dialogFormVisible = false;
      this.form = {
        customerName: "",
        customerPhoneData: "",
      };
    },
    async deleteCustomer(row) {
      row.visible = false;
      const res = await deleteExaCustomer({ ID: row.ID });
      if (res.code === 0) {
        this.$message({
          type: "success",
          message: "删除成功",
        });
        if (this.tableData.length === 1 && this.page > 1) {
          this.page--;
        }
        this.getTableData();
      }
    },
    async enterDialog() {
      let res;
      switch (this.type) {
        case "create":
          res = await createExaCustomer(this.form);
          break;
        case "update":
          res = await updateExaCustomer(this.form);
          break;
        default:
          res = await createExaCustomer(this.form);
          break;
      }

      if (res.code === 0) {
        this.closeDialog();
        this.getTableData();
      }
    },
    openDialog() {
      this.type = "create";
      this.dialogFormVisible = true;
    },
  },
};
</script>
<style></style>
