<template>


  <div>

	  <div>
    <div class="gva-table-box" style="border-bottom:10px solid;border-bottom-color:#f0f2f5" >
      <div style="height:30px;line-height:15px">联系人管理</div>
      <el-form ref="elForm" :inline="false" >
        <el-form-item label="发送目标">
          <el-select
            v-model="tableData1.userId"
            multiple
            style="width:432px"
            placeholder="请选择人员">
            <el-option
              v-for="item in tableData1"
              :key="item.userId"
              :label="item.name"
              :value="item.userId">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="发送内容" style="width:500px">
          <el-input  
          type="textarea" 
          :rows = "10"
          @change="change($event)"
          v-model="messageData" />
        </el-form-item>
      </el-form>
        <div class="dialog-footer" style="margin-left:285px">
          <el-button size="small" icon="el-icon-message" type="success" @click="submitForm">确认发送</el-button>
          <el-button size="small" icon="el-icon-message" type="primary" @click="clearForm">清空内容</el-button>
        </div>
    </div>
  </div>
  </div>
</template>
<script>
export default {
  name: 'Customer',
  props: [],
  data() {
    return {
      formData: {
        field101: 1,
        field108: 1,
        field109: undefined,
        field102: undefined,
      },
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
      messageData: ''
    }
  },
  computed: {},
  watch: {},
  created() {
  },
  mounted() {},
  methods: {
    submitForm() {
        this.$message({
          type: "success",
          message:"发送成功！"
        })
    },
    clearForm() {
      this.$refs['elForm'].resetFields()
    },
    openDialog() {
      this.type = 'create'
      this.dialogFormVisible = true
    },
    change(){
      this.$forceUpdate()
    }
}
}

</script>
<style>
</style>
