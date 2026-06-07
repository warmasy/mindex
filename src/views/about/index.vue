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
        <el-divider />
        <h4>内容说明</h4>
        <ul>
          <li> <strong>AI管理</strong>，不可用，还没删</li>
          <li> <strong>日志管理</strong>，不可用，还没删</li>
          <li> <strong>其它</strong>，不可用，还没删</li>
          <li> <strong>所有数据请求改为本地 Mock 数据，无需后端服务</strong> </li>
        </ul>
        <el-divider />

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
