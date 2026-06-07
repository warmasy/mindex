<template>
  <div class="app-container">
    <el-card class="box-card">
      <template #header>
        <div class="card-header">
          <span>关于本项目</span>
        </div>
      </template>
      <div class="about-content">
        <h3>地球online</h3>
        <p>本项目是 <strong>地球online</strong>。</p>
        <el-divider />
        <h4>主要改造内容</h4>
        <ul>
          <li>移除登录验证，打开页面自动以管理员身份登录</li>
          <li>移除右上角头像下拉菜单中的退出登录按钮</li>
          <li>将后端动态路由改为前端静态路由，所有菜单对所有用户可见</li>
          <li>所有数据请求改为本地 Mock 数据，无需后端服务</li>
          <li>新增关于页面（本页面）</li>
          <li>底部增加版权信息和项目说明</li>
        </ul>
        <el-divider />
        <h4>技术栈</h4>
        <ul>
          <li><strong>前端框架：</strong>Vue 3 + Vite</li>
          <li><strong>UI 组件库：</strong>Element Plus</li>
          <li><strong>状态管理：</strong>Pinia</li>
          <li><strong>路由管理：</strong>Vue Router 4</li>
          <li><strong>CSS 预处理器：</strong>SCSS</li>
        </ul>
        <el-divider />
        <h4>原项目信息</h4>
        <p>项目：地球online</p>
        <p>基于 Vue3 + Vite + Element Plus 构建的管理系统。</p>
        <p>所有数据均为本地模拟数据。</p>
      </div>
    </el-card>
  </div>
</template>

<script setup>
</script>

<style scoped>
.about-content {
  line-height: 1.8;
  color: #333;
}
.about-content h3 {
  margin-top: 0;
  color: #409EFF;
}
.about-content h4 {
  margin-top: 16px;
  color: #606266;
}
.about-content ul {
  padding-left: 20px;
}
.about-content li {
  margin-bottom: 8px;
}
.card-header {
  font-weight: bold;
  font-size: 16px;
}
</style>
