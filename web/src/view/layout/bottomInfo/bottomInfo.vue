<template>
  <div class="bottom-info">
    <div>
      <span>Powered by</span>
      <span>
        <a href="https://github.com/flipped-aurora/gin-vue-admin">{{ $GIN_VUE_ADMIN.appName }}</a>
      </span>
      <el-divider direction="vertical" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'BottomInfo'
}
</script>

<style lang="scss">
.bottom-info {
  color: #888;
  height: 30px;
  line-height: 20px;
  a {
    color: #888;
  }
  div {
    display: flex;
    justify-content: center;
    span{
      margin: 0 3px;
    }
  }
}
</style>
