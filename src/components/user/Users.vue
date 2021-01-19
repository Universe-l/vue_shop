<template>
  <div class="page_content">
    <!-- 面包屑 -->
    <el-breadcrumb separator="/" class="bread_card">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>

    <el-card class="box_card">
      <!-- 搜索框 -->
      <el-row :gutter="20">
        <el-col :span="10">
          <el-input placeholder="请输入内容" v-model="push_data.query" class="input-with-select">
            <el-button slot="append" icon="el-icon-search"></el-button>
          </el-input>
        </el-col>
        <el-col :span="6">
          <el-button type="primary">添加用户</el-button>
        </el-col>
      </el-row>

      <!-- 用户列表表格 -->
      <el-table :data="user_list" border stripe class="user_table">
        <el-table-column type="index"></el-table-column>
        <el-table-column prop="username" label="姓名" min-width="180"></el-table-column>
        <el-table-column prop="email" label="邮箱" min-width="180"></el-table-column>
        <el-table-column prop="mobile" label="电话" min-width="180"></el-table-column>
        <el-table-column prop="role_name" label="角色" min-width="180"></el-table-column>
        <el-table-column prop="mg_state" label="状态" min-width="180">
          <template slot-scope="scope">
            <el-switch v-model="scope.row.mg_state"></el-switch>
          </template>
        </el-table-column>
        <el-table-column label="操作" min-width="180">
          <template slot-scope="scope">
            <el-button type="primary" @click="handle(1, scope.row.id)">编辑</el-button>
            <el-button type="danger">删除</el-button>
            <el-tooltip class="item" effect="dark" content="分配角色" placement="top">
              <el-button type="warning">设置</el-button>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>

      <el-pagination
        :background="true"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage4"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="push_data.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
        class="page"
      >
      </el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      push_data: {
        query: '', // 查询参数
        pagenum: 1,
        pagesize: 5
      },
      user_list: [], // 用户列表
      pagenum: '', // 页数
      total: '' // 总条数
    }
  },
  created() {
    this.get_user_list()
  },
  methods: {
    async get_user_list() {
      const { data: res } = await this.$http.get('users', { params: this.push_data })
      if (res.meta.status !== 200) {
        return this.$message.error('获取用户列表失败！')
      } else {
        this.user_list = res.data.users
        this.pagenum = res.data.pagenum
        this.total = res.data.total
      }
      console.log(res)
    },
    // 编辑
    edit_user(id) {

    },
    handleSizeChange() {
    },
    handleCurrentChange() {

    }
  }
}
</script>

<style lang="scss" scoped>
  .page_content {
    width: 100%;
    height: 100%;

    .bread_card {
      height: 22px;
      line-height: 22px;
    }

    .box_card {
      padding: 15px;
      margin-top: 30px;

      .user_table {
        margin-top: 50px;
        font-size: 14px;
      }

      .page {
        margin-top: 50px;
      }
    }
  }
</style>
