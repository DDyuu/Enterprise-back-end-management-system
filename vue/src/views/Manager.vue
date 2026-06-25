<template>
  <div>
    <!-- 头部开始 -->
    <div class="header">
      <div class="header-left">
        <!-- 移动端菜单按钮 -->
        <button class="menu-toggle" @click="toggleSidebar" aria-label="切换菜单">
          <svg viewBox="0 0 24 24" width="24" height="24" fill="white">
            <path d="M3 6h18v2H3V6zm0 5h18v2H3v-2zm0 5h18v2H3v-2z"/>
          </svg>
        </button>
        <img src="../assets/java.svg" alt="" style="width: 1.875rem; height: 1.875rem;" />
        <span class="header-title">后台管理系统</span>
      </div>

      <div style="flex: 1"></div>

      <div class="header-right">
        <img :src="data.user.avatar" alt="https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png" class="avatar-img" />
        <span class="user-name">{{ data.user.name }}</span>
      </div>
    </div>
    <!-- 头部结束 -->

    <!-- 下面部分开始 -->
    <div class="main-container">

      <!-- 侧边栏遮罩（移动端） -->
      <div class="sidebar-overlay" :class="{ 'sidebar-overlay--visible': sidebarVisible }" @click="closeSidebar"></div>

      <!-- 左侧导航开始 -->
      <div class="sidebar" :class="{ 'sidebar--open': sidebarVisible }">
        <el-menu :default-active="router.currentRoute.value.path" router :default-openeds="['1']"
                 class="sidebar-menu" @select="closeSidebar">
          <el-menu-item index="/manager/home">
            <el-icon><House/></el-icon>
            系统首页
          </el-menu-item>

          <el-menu-item v-if="data.user.role === 'admin'" index="/manager/data">
            <el-icon><DataLine/></el-icon>
            数据统计
          </el-menu-item>

          <el-menu-item v-if="data.user.role === 'admin'" index="/manager/article">
            <el-icon><Document/></el-icon>
            文章管理
          </el-menu-item>

          <el-menu-item v-if="data.user.role === 'admin'" index="/manager/department">
            <el-icon><OfficeBuilding/></el-icon>
            部门管理
          </el-menu-item>

          <el-sub-menu v-if="data.user.role === 'admin'" index="1">
            <template #title>
              <el-icon><User/></el-icon>
              <span>用户管理</span>
            </template>
            <el-menu-item index="/manager/Admin">管理员信息</el-menu-item>
            <el-menu-item index="/manager/employee">员工信息</el-menu-item>
          </el-sub-menu>

          <el-menu-item index="/manager/person">
            <el-icon><UserFilled/></el-icon>
            个人信息
          </el-menu-item>
          <el-menu-item index="/manager/modifyPassword">
            <el-icon><Lock/></el-icon>
            修改密码
          </el-menu-item>
          <el-menu-item @click="logout">
            <el-icon><SwitchButton/></el-icon>
            退出登录
          </el-menu-item>
        </el-menu>
      </div>
      <!-- 左侧导航结束 -->

      <!-- 右侧主体开始 -->
      <div class="main-content" :class="{ 'main-content--shifted': sidebarVisible }">
        <RouterView @updateUser="updateUser"/>
      </div>
      <!-- 右侧主体结束 -->
    </div>
    <!-- 下面部分结束 -->
  </div>
</template>

<script setup>
import { reactive, ref, watch } from "vue";
import { useRoute } from "vue-router";
import router from "../router/index.js";
import {House, DataLine, Document, OfficeBuilding, User, UserFilled, Lock, SwitchButton} from "@element-plus/icons-vue"

const route = useRoute()
const sidebarVisible = ref(false)

const toggleSidebar = () => {
  sidebarVisible.value = !sidebarVisible.value
}

const closeSidebar = () => {
  sidebarVisible.value = false
}

// 路由切换时自动关闭移动端侧边栏
watch(() => route.path, () => {
  closeSidebar()
})

const data = reactive({
  user: JSON.parse(localStorage.getItem("user"))
})

const logout =() => {
  localStorage.removeItem("user");
  location.href = "/login";
}

const updateUser = () =>{
  data.user = JSON.parse(localStorage.getItem("user"));
}
</script>

<style>
.el-menu .is-active {
  background-color: #e6ecf7 !important;
}

.el-menu .is-active > .el-sub-menu__title {
  background-color: white !important;
}

/* ========== 布局样式 ========== */
.header {
  height: 3.75rem;
  background-color: #1677ff;
  width: 100%;
  display: flex;
  align-items: center;
  position: relative;
  z-index: 1001;
}

.header-left {
  width: 12.5rem;
  display: flex;
  margin-left: 1.25rem;
  align-items: center;
  gap: 0.5rem;
}

.header-title {
  font-size: 1.5rem;
  color: white;
  white-space: nowrap;
}

.header-right {
  width: fit-content;
  display: flex;
  align-items: center;
  padding-right: 0.625rem;
  gap: 0.3125rem;
}

.avatar-img {
  width: 2.5rem;
  height: 2.5rem;
  border-radius: 50%;
}

.user-name {
  color: white;
  font-size: 1.125rem;
  white-space: nowrap;
}

/* ---- 汉堡菜单按钮 ---- */
.menu-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.25rem;
  line-height: 0;
}

.main-container {
  display: flex;
  position: relative;
}

/* ---- 侧边栏 ---- */
.sidebar {
  width: 12.5rem;
  background: #fff;
  position: relative;
  z-index: 1000;
  flex-shrink: 0;
  transition: transform 0.3s ease;
}

.sidebar-menu {
  border-right: 1px solid gray;
  min-height: calc(100vh - 3.75rem);
}

/* ---- 主内容区 ---- */
.main-content {
  flex: 1;
  width: 0;
  background-color: transparent;
  padding: 0.625rem;
  min-width: 0;
}

/* ========== 移动端抽屉式侧边栏 ========== */
@media (max-width: 768px) {
  .menu-toggle {
    display: block;
  }

  .header-left {
    width: auto;
    margin-left: 0.625rem;
  }

  .header-title {
    font-size: 1rem;
  }

  .user-name {
    display: none;
  }

  .avatar-img {
    width: 2rem;
    height: 2rem;
  }

  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    z-index: 2000;
    transform: translateX(-100%);
    box-shadow: 0 0 1rem rgba(0, 0, 0, 0.2);
  }

  .sidebar--open {
    transform: translateX(0);
  }

  .sidebar-menu {
    min-height: 100vh;
    padding-top: 0.5rem;
    border-right: none;
  }

  /* 遮罩 */
  .sidebar-overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.4);
    z-index: 1999;
    transition: opacity 0.3s ease;
  }

  .sidebar-overlay--visible {
    display: block;
  }

  .main-content {
    padding: 0.375rem;
  }
}
</style>
