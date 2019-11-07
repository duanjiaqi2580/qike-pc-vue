<template>
  <el-container class="home-container">
    <el-aside :width="isOpen?'200px':'64px'">
      <!-- logo -->
      <div :class="{smallLogo:!isOpen}" class="logo"></div>
      <!-- 导航菜单 -->
      <el-menu
        :collapse="!isOpen"
        :collapse-transition="false"
        :default-active="$route.path"
        active-text-color="#ffd04b"
        background-color="#002033"
        router
        style="border-right:none"
        text-color="#fff"
      >
        <el-menu-item index="/">
          <i class="el-icon-s-home"></i>
          <span slot="title">首页</span>
        </el-menu-item>
        <el-menu-item index="/article">
          <i class="el-icon-document"></i>
          <span slot="title">内容管理</span>
        </el-menu-item>
        <el-menu-item index="/image">
          <i class="el-icon-picture"></i>
          <span slot="title">素材管理</span>
        </el-menu-item>
        <el-menu-item index="/publish">
          <i class="el-icon-s-promotion"></i>
          <span slot="title">发布文章</span>
        </el-menu-item>
        <el-menu-item index="/comment">
          <i class="el-icon-chat-dot-round"></i>
          <span slot="title">评论管理</span>
        </el-menu-item>
        <el-menu-item index="/fans">
          <i class="el-icon-present"></i>
          <span slot="title">粉丝管理</span>
        </el-menu-item>
        <el-menu-item index="/setting">
          <i class="el-icon-setting"></i>
          <span slot="title">个人设置</span>
        </el-menu-item>
      </el-menu>
    </el-aside>
    <el-container>
      <el-header>
        <!-- 图标 -->
        <span @click="toggleMenu" class="el-icon-s-fold icon"></span>
        <!-- 文字 -->
        <span class="text">江苏传智播客科技教育有限公司</span>
        <!-- 下拉菜单组件 -->
        <el-dropdown @command="handleClick" class="dropdown">
          <span class="el-dropdown-link">
            <img :src="photo" alt class="headIcon" />
            <span class="userName">{{name}}</span>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="setting" icon="el-icon-setting">个人设置</el-dropdown-item>
            <el-dropdown-item command="logout" icon="el-icon-unlock">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-header>
      <el-main>
        <!-- 二级路由容器 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import local from '@/utils/local'
import eventBus from '@/eventBus'
export default {
  data () {
    return {
      // 是不是展开的
      isOpen: true,
      // 头像
      photo: '',
      // 名称
      name: ''
    }
  },
  created () {
    const user = local.getUser() || {}
    this.photo = user.photo
    this.name = user.name
    // 绑定事件 updateName
    eventBus.$on('updateName', name => {
      this.name = name
    })
    // 绑定事件 updatePhoto
    eventBus.$on('updatePhoto', photo => {
      this.photo = photo
    })
  },
  methods: {
    toggleMenu () {
      // 切换侧边栏  展开与收起
      this.isOpen = !this.isOpen
    },
    // 绑定的click事件无效
    // 给的是element-ui提供的组件绑定的click事件，如果组件不支持click事件，无法触发。
    // 组件不支持，给组件解析后的DOM绑定事件
    // vue提供了事件修饰符功能，prevent once stop --- native 把事件绑定在原生DOM上
    setting () {
      this.$router.push('/setting')
    },
    logout () {
      local.delUser()
      this.$router.push('/login')
    },
    handleClick (command) {
      // command  值  setting | logout
      // 根据 command 值去执行不同的业务
      this[command]()
      // this.setting() === command setting
      // this.logout() === command logout
    }
  }
}
</script>

<style scoped lang='less'>
.home-container {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  .el-aside {
    background: #002033;
    .logo {
      width: 100%;
      height: 60px;
      background: #002244 url(../../assets/logo_admin.png) no-repeat center /
        140px auto;
    }
    // 覆盖大图成小图
    .smallLogo {
      background-image: url(../../assets/logo_admin_01.png);
      background-size: 36px auto;
    }
  }
  .el-header {
    border-bottom: 1px solid #ddd;
    line-height: 60px;
    .icon {
      font-size: 30px;
      vertical-align: middle;
    }
    .text {
      margin-left: 10px;
      vertical-align: middle;
    }
    .dropdown {
      float: right;
      .headIcon {
        width: 30px;
        height: 30px;
        vertical-align: middle;
      }
      .userName {
        margin-left: 5px;
        font-weight: bold;
        vertical-align: middle;
      }
    }
  }
}
</style>
