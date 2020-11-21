<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="">
            </div>
            <div class="input_box">
                <!-- <el-input v-model="username" placeholder="请输入用户名"></el-input>
                <el-input v-model="password" placeholder="请输入密码" show-password></el-input> -->
                <el-form label-position="right" label-width="80px">
                    <el-form-item label="用户名">
                        <el-input v-model="login_info.username" prefix-icon="el-icon-user"></el-input>
                    </el-form-item>
                    <el-form-item label="密码">
                        <el-input v-model="login_info.password" show-password prefix-icon="el-icon-lock"></el-input>
                    </el-form-item>
                     <el-form-item>
                         <div class="btns">
                            <el-button type="primary" @click="login" v-if="login_info.username&&login_info.password">登录</el-button>
                             <el-button type="primary" disabled v-else>登录</el-button>
                            <el-button type="info" @click="reset_login">重置</el-button>
                         </div>
                    </el-form-item>
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      login_info: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    login () {
      if (this.login_info.username && this.login_info.password) {
        this.$http.post('login', this.login_info).then(res => {
          if (res.data.meta.status === 200) {
            this.$message({
              message: res.data.meta.msg,
              type: 'success'
            })
            window.sessionStorage.setItem('token', res.data.data.token)
            this.$router.push('/home')
          } else {
            this.$message({
              message: res.data.meta.msg,
              type: 'warning'
            })
          }
        })
      }
    },
    reset_login () {
      this.login_info.username = ''
      this.login_info.password = ''
    }
  }
}
</script>

<style lang="scss">
    .login_container {
        background-color: #2b4b6b;
        height: 100%;

        .login_box {
            width: 450px;
            height: 300px;
            background: #ffffff;
            position: absolute;
            top: 50%;
            left: 50%;
            margin: -150px 0 0 -225px;
            border-radius: 5px;

            .avatar_box {
                width: 130px;
                height: 130px;
                border-radius: 50%;
                border: 1px solid #eeeeee;
                margin: -65px auto 0 auto;
                padding: 10px;
                box-shadow: 0 0 10px #dddddd;
                background: #ffffff;

                img {
                    width: 100%;
                    height: 100%;
                    border-radius: 50%;
                    background-color: #eeeeee;
                }
            }

            .input_box {
                width: 80%;
                margin: 0 auto;
                margin-top: 25px;

                .btns {
                    display: flex;
                    justify-content: flex-end;
                }
            }
        }
    }
</style>
