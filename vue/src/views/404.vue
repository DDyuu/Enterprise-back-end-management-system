<template>
  <div style="text-align:center;">
    <img src="../assets/pic_404.jpg" height="420" width="650" alt=""/>
    <div>
      <el-button type="primary" style="width:9.375rem; height:3.75rem; font-size:1.25rem" @click="goHome">返回主页</el-button>
    </div>
  </div>
</template>
  
<script setup>
  const goHome = () => {
    location.href = '/manager/home'
  }
</script>

<style scoped>
img {
  max-width: 90%;
  height: auto;
}

@media (max-width: 768px) {
  div > div > .el-button {
    width: 7.5rem !important;
    height: 2.8125rem !important;
    font-size: 1rem !important;
  }
}
</style>
