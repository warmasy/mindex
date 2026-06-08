<template>
   <div class="app-container profile-page">
      <el-row :gutter="20" class="profile-row">
         <el-col :span="6" :xs="24">
            <el-card class="box-card info-card">
               <template v-slot:header>
                 <div class="clearfix">
                   <span>网站信息</span>
                 </div>
               </template>
                                             <div class="info-content">
                  <div class="text-center">
                     <div class="avatar-box">
                        <img v-if="siteInfo.logo" :src="siteInfo.logo" class="site-logo-img" />
                        <el-avatar v-else :size="80" :src="defaultAvatar" />
                     </div>
                  </div>
                  <ul class="list-group list-group-striped">
                     <li class="list-group-item">
                        <svg-icon icon-class="documentation" />网站名称
                        <div class="pull-right">{{ siteInfo.name }}</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="link" />网址
                        <div class="pull-right">{{ siteInfo.url }}</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="message" />简介
                        <div class="pull-right">{{ siteInfo.description }}</div>
                     </li>
                  </ul>
                  <div class="info-actions">
                    <el-button type="primary" size="small" icon="CopyDocument" @click="copyUrl(siteInfo.url, siteInfo.name)">复制网址</el-button>
                    <el-button type="success" size="small" icon="TopRight" @click="openWebsite(siteInfo.url)">跳转</el-button>
                  </div>
               </div>
            </el-card>
         </el-col>
         <el-col :span="18" :xs="24">
            <el-card class="tab-card">
               <template v-slot:header>
                 <div class="clearfix">
                   <span>简介</span>
                 </div>
               </template>
               <el-tabs v-model="selectedTab">
                  <el-tab-pane label="常用计算" name="common">
                     <div class="calc-list">
                        <div v-for="item in calcItems" :key="item.id" class="calc-item">
                           <h4>{{ item.name }}</h4>
                           <p>{{ item.desc }}</p>
                           <div class="calc-meta">
                             <span>类型: {{ item.type }}</span>
                             <span>更新: {{ item.updateTime }}</span>
                           </div>
                        </div>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="公式库" name="formula">
                     <div class="formula-list">
                        <div v-for="formula in formulas" :key="formula.id" class="formula-item">
                           <el-tag type="primary" class="formula-tag">{{ formula.name }}</el-tag>
                           <span class="formula-expr">{{ formula.expr }}</span>
                        </div>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="历史记录" name="history">
                     <div class="history-list">
                        <div v-for="history in historyList" :key="history.id" class="history-item">
                           <span class="history-name">{{ history.name }}</span>
                           <span class="history-result">结果: {{ history.result }}</span>
                           <span class="history-time">{{ history.time }}</span>
                        </div>
                     </div>
                  </el-tab-pane>
               </el-tabs>
            </el-card>
         </el-col>
      </el-row>
   </div>
</template>

<script setup name="Mechanical">
import { ElMessage } from 'element-plus'

const defaultAvatar = 'https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png'

const siteInfo = reactive({
  logo: 'https://mj002.zh-cn.edgeone.cool/favicon.svg',
  name: '机械计算',
  url: 'https://mech.example.com',
  description: '机械工程常用计算工具平台。'
})

const selectedTab = ref("common")

const calcItems = ref([
  { id: 1, name: '齿轮传动计算', desc: '计算齿轮传动比、模数、齿数等参数', type: '传动', updateTime: '2024-06-01' }
])

const formulas = ref([
  { id: 1, name: '扭矩', expr: 'T = 9550 × P / n (N·m)' }
])

const historyList = ref([
  { id: 1, name: '计算任务 1', result: '86.42', time: '2024-06-01 10:00' }
])

function openWebsite(url) {
  if (url) {
    window.open(url, '_blank')
  }
}

function copyUrl(url, name) {
  if (!url) {
    ElMessage.warning('网址为空，无法复制')
    return
  }
  navigator.clipboard.writeText(url).then(() => {
    ElMessage.success(`已复制 ${name} 的网址`)
  }).catch(() => {
    const input = document.createElement('input')
    input.value = url
    document.body.appendChild(input)
    input.select()
    document.execCommand('copy')
    document.body.removeChild(input)
    ElMessage.success(`已复制 ${name} 的网址`)
  })
}
</script>

<style scoped>
.profile-page {
  height: 100%;
}
.profile-row {
  height: 100%;
}
.profile-row > .el-col {
  height: 100%;
}
.info-card {
  height: 100%;
  display: flex;
  flex-direction: column;
}
.info-card :deep(.el-card__body) {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.info-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.avatar-box {
  margin-bottom: 20px;
  text-align: center;
}
.site-logo-img {
  width: 80px;
  height: 80px;
  object-fit: contain;
  border-radius: 8px;
}
.list-group {
  flex: 1;
  padding-left: 0;
  list-style: none;
  margin: 0;
}
.list-group-item {
  display: flex;
  align-items: center;
  padding: 11px 0;
  border-bottom: 1px solid var(--el-border-color-lighter);
  font-size: 13px;
}
.list-group-item:last-child {
  border-bottom: none;
}
.list-group-item .svg-icon {
  margin-right: 8px;
  font-size: 16px;
}
.pull-right {
  margin-left: auto;
  color: var(--el-text-color-secondary);
}
.info-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 16px;
  padding-top: 12px;
  border-top: 1px solid var(--el-border-color-lighter);
}
.tab-card {
  height: 100%;
}
.tab-card :deep(.el-card__body) {
  height: calc(100% - 55px);
}
.tab-card :deep(.el-tabs) {
  height: 100%;
}
.tab-card :deep(.el-tabs__content) {
  height: calc(100% - 50px);
  overflow-y: auto;
}
.calc-list {
  padding: 10px 0;
}
.calc-item {
  padding: 16px 0;
  border-bottom: 1px solid var(--el-border-color-lighter);
}
.calc-item:last-child {
  border-bottom: none;
}
.calc-item h4 {
  margin-bottom: 8px;
  color: var(--el-color-primary);
  font-size: 16px;
}
.calc-item p {
  color: var(--el-text-color-regular);
  font-size: 14px;
  margin-bottom: 8px;
}
.calc-meta {
  display: flex;
  gap: 20px;
  font-size: 12px;
  color: var(--el-text-color-secondary);
}
.formula-list {
  padding: 20px;
}
.formula-item {
  padding: 12px 0;
  border-bottom: 1px solid var(--el-border-color-lighter);
  display: flex;
  align-items: center;
  gap: 16px;
}
.formula-item:last-child {
  border-bottom: none;
}
.formula-tag {
  flex-shrink: 0;
}
.formula-expr {
  font-family: 'Courier New', monospace;
  color: var(--el-text-color-regular);
  font-size: 14px;
}
.history-list {
  padding: 10px 0;
}
.history-item {
  padding: 12px 16px;
  margin-bottom: 8px;
  border-radius: 6px;
  background: var(--el-fill-color-lighter);
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 14px;
}
.history-item:last-child {
  margin-bottom: 0;
}
.history-name {
  color: var(--el-text-color-primary);
  font-weight: 500;
}
.history-result {
  color: var(--el-color-success);
}
.history-time {
  color: var(--el-text-color-secondary);
  font-size: 12px;
}
</style>
