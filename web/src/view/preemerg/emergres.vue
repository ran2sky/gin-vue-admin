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
          label="物资编码"
          prop="productId"
          width="120"
        />
        <el-table-column align="left" label="物资分类" prop="className" width="120" />
        <el-table-column
          align="left"
          label="物资名称"
          prop="productName"
          width="350"
        />
        <el-table-column
          align="left"
          label="总数量"
          prop="totalVolume"
          width="120"
        />
                <el-table-column
          align="left"
          label="发出数量"
          prop="currentVolume"
          width="120"
        />
                <el-table-column
          align="left"
          label="剩余数量"
          prop="remainingVolume"
          width="120"
        />
                <el-table-column
          align="left"
          label="存放地点"
          prop="storeLocation"
          width="200"
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
          v-model="formData.productId"
          value-format="yyyy-MM-DD" />
        </el-form-item>
        <el-form-item label="姓名" style="width:33%">
          <el-input v-model="formData.className" autocomplete="off" />
        </el-form-item>
        <el-form-item label="交接事项" style="width:50%">
          <el-input v-model="formData.productName" autocomplete="off" />
        </el-form-item>
        <el-form-item label="工作日志" style="width:50%">
          <el-input
            type="textarea"
            v-model="formData.totalVolume"
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
        productId: undefined,
        className: undefined,
        productName: undefined,
        totalVolume: undefined,
        currentVolume: undefined,
        remainingVolume: undefined,
        storeLocation: undefined
      },
      tableData1: [
        {
          productId: "A0001",
          className: "个人防护物资",
          productName: "防尘口罩 ",
          totalVolume: "100000",
          currentVolume: '30000',
          remainingVolume: "70000",
          storeLocation: "Shanghai",
        },
        {
          productId: "A0002",
          className: "个人防护物资",
          productName: "过滤式防毒面具 ",
          totalVolume: "60000",
          currentVolume: '30000',
          remainingVolume: "30000",
          storeLocation: "Shanghai",
        },
        {
          productId: "A0003",
          className: "个人防护物资",
          productName: "氧气呼吸器",
          totalVolume: "30000",
          currentVolume: '10000',
          remainingVolume: "20000",
          storeLocation: "Shanghai",
        },
        {
          productId: "A0004",
          className: "个人防护物资",
          productName: "气密型化学防护服",
          totalVolume: "20000",
          currentVolume: '10000',
          remainingVolume: "10000",
          storeLocation: "Shanghai",
        },
        {
          productId: "A0005",
          className: "围堵物资",
          productName: "橡胶围油栏",
          totalVolume: "100000",
          currentVolume: '5000',
          remainingVolume: "5000",
          storeLocation: "Shanghai",
        },
        
        {
          productId: "A0007",
          className: "处理处置物资",
          productName: "浮动油馕",
          totalVolume: "1000",
          currentVolume: '200',
          remainingVolume: "800",
          storeLocation: "Shanghai",
        },
        {
          productId: "A0008",
          className: "处理处置物资",
          productName: "颗粒状活性炭",
          totalVolume: "30000",
          currentVolume: '11000',
          remainingVolume: "19000",
          storeLocation: "Shanghai",
        },

        {
          productId: "A0009",
          className: "处理处置物资",
          productName: "水泥",
          totalVolume: "1000000",
          currentVolume: '400000',
          remainingVolume: "600000",
          storeLocation: "Shanghai",
        },




        {
          productId: "A0010",
          className: "其他类物资",
          productName: "小型密封容器",
          totalVolume: "1000000",
          currentVolume: '330000',
          remainingVolume: "670000",
          storeLocation: "Shanghai",
        },
      ],
      rules: {
        productId: [
          {required: true, message: "日期不能为空", trigger: 'blur'},
        ],
        className: [
          {required: true, message: "姓名不能为空", trigger: 'blur'},
        ],
        productName: [
          {required: true, message: "事件不能为空", trigger: 'blur'},
        ],
        totalVolume: [
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
              this.formData.productId = formatTimeToStr(this.formData.productId,"yyyy-MM-dd")
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
        this.formData.productId = row.productId
        this.formData.className = row.className
        this.formData.productName = row.productName
        this.formData.totalVolume = row.totalVolume
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
