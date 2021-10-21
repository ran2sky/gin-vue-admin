<template>
  <div>
    <el-row :gutter="5" style="align-item:center">
      <span style="text-align:center">视频信号调度系统</span>
    </el-row>
	<el-row :gutter="20">
		 <el-col :span="12">
			 <iframe src="http://demo.easynvr.com:10800/play.html?channel=9&iframe=yes&aspect=640x360&showConsole=no&autoplay=true" width="800" height="450" allowfullscreen allow="autoplay"></iframe>
        </el-col>
        <el-col :span="12">
			<iframe src="http://demo.easynvr.com:10800/play.html?channel=54&iframe=yes&aspect=640x360&showConsole=no&autoplay=true" width="800" height="450" allowfullscreen allow="autoplay"></iframe>
        </el-col>
        <el-col :span="12">
			<iframe src="http://demo.easynvr.com:10800/play.html?channel=65&iframe=yes&aspect=640x360&showConsole=no&autoplay=true" width="800" height="450" allowfullscreen allow="autoplay"></iframe>
        </el-col>
        <el-col :span="12">
			<iframe src="http://demo.easynvr.com:10800/play.html?channel=128&iframe=yes&aspect=640x360&showConsole=no&autoplay=true" width="800" height="450" allowfullscreen allow="autoplay"></iframe>
        </el-col>
	</el-row>
  </div>
</template>
<script>
export default {
  components: {},
  props: [],
  data() {
    return {
      formData: {
        field101: undefined,
        field104: undefined,
        field103: undefined,
      },
      rules: {},
    }
  },
  computed: {},
  watch: {},
  created() {},
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
  }
}

</script>
<style>
</style>
