<template>
  <div class="container_box">
    <el-container>
      <el-header>
        <div class="header_box">
          <div class="header_left">
            <div class="logo_box">
              <img src="../assets/logo.png" alt="">
            </div>
            <p>电商后台管理系统</p>
          </div>
          <div class="header_right">
            <el-button type="info" @click="login_out">退出</el-button>
          </div>
        </div>
      </el-header>
      <el-container>
        <!-- 侧边栏 -->
        <el-aside :width="is_collapse ? 'auto' : '200px'">
          <div class="toggle-button" @click="toggle_collapse">|||</div>
          <el-menu
            default-active="1"
            text-color="#fff"
            active-text-color="#409EFF"
            background-color="#333744"
            :unique-opened="true"
            :collapse="is_collapse"
            :collapse-transition="false"
            :router="true"
          >
            <el-submenu :index=" '/' + item.path" v-for="(item, index) in menulist" :key="index" :default-active="act_path">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{item.authName}}</span>
              </template>
              <el-menu-item :index="'/' + item.path" v-for="(child, index) in item.children" :key="index" @click="save_nav_state('/' + item.path)">
                <template>
                  <i class="el-icon-menu"></i>
                  <span>{{child.authName}}</span>
                </template>
              </el-menu-item>
            </el-submenu>
          </el-menu>
        </el-aside>
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      menulist: [], // 导航菜单
      is_collapse: false, // 是否收起导航侧边栏，默认否,ture 时展开
      act_path: '' // 激活的链接
    }
  },
  created() {
    this.getMenuList()
    this.act_path = window.sessionStorage.getItem('act_path')
  },
  methods: {
    login_out () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有菜单
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status === 200) {
        this.menulist = res.data
      } else {
        return this.$message.error(res.meta.msg)
      }
    },
    // 导航的收起与展开
    toggle_collapse() {
      this.is_collapse = !this.is_collapse
    },
    // 点击导航时把链接存进去
    save_nav_state(path) {
      window.sessionStorage.setItem('act_path', path)
      this.act_path = path
    }
  }
}
</script>

<style lang="scss">
.el-container {
  height: 100%;
}
.el-header {
  background-color: #373d41;
  height: 50%;
}
.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background-color: #eeedf1;
}
  .container_box {
    height: 100%;

    .header_box {
      height: 100%;
      margin: 0 auto;
      display: flex;
      justify-content: space-between;
      align-items: center;

      .header_left {
        display: flex;
        align-items: center;
        .logo_box {
          width: 50px;
          height: 50px;
          background-color: azure;
          border-radius: 50%;
          margin-right: 10px;

          img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
          }
        }
        p {
          color: #ffffff;
          font-size: 20px;
        }
      }
    }
  }
  .toggle-button {
    cursor: pointer;
    color: #ffffff;
    letter-spacing: 3px;
    height: 56px;
    line-height: 56px;
    text-align: center;
  }
</style>
