<template>
    <div>
        <el-button type="info" @click="login_out">退出</el-button>
    </div>
</template>

<script>
export default {
  data () {
    return {}
  },
  methods: {
    login_out () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style lang="scss">

</style>
