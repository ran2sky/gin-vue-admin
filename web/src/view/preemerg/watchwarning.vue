<template>
  <div>
	<el-row :gutter="20">
		 <el-col :span="12">
       <div style="text-align:center">台风预警监测</div>
<iframe width="800" height="450" src="https://embed.windy.com/embed2.html?lat=34.235&lon=121.553&detailLat=37.456&detailLon=126.729&width=800&height=450&zoom=5&level=surface&overlay=wind&product=ecmwf&menu=&message=true&marker=true&calendar=12&pressure=true&type=map&location=coordinates&detail=&metricWind=km%2Fh&metricTemp=%C2%B0C&radarRange=-1" frameborder="0"></iframe>
        </el-col>

        <el-col :span="12">
			<a class="weatherwidget-io" href="https://forecast7.com/zh/31d23121d47/shanghai/" data-label_1="SHANGHAI" data-label_2="当前天气" data-font="微软雅黑 (Microsoft Yahei)" data-theme="pure" >SHANGHAI 当前天气</a>
        </el-col>
        <el-col :span="12">
        </el-col>
        <el-col :span="12">
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
