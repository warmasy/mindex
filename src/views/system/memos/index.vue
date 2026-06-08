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
                  <el-tab-pane label="备忘录" name="memos">
                     <div class="memo-list">
                        <div v-for="memo in memoList" :key="memo.id" class="memo-item">
                           <div class="memo-content">
                              <p>{{ memo.content }}</p>
                           </div>
                           <div class="memo-meta">
                             <span>{{ memo.date }}</span>
                             <span>标签: {{ memo.tag }}</span>
                           </div>
                        </div>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="标签" name="tags">
                     <div class="tag-cloud">
                        <el-tag v-for="tag in tags" :key="tag" class="tag-item">{{ tag }}</el-tag>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="归档" name="archive">
                     <div class="archive-list">
                        <div v-for="archive in archiveList" :key="archive.month" class="archive-item">
                           <el-link type="primary">{{ archive.month }}</el-link>
                           <span class="archive-count">({{ archive.count }} 条)</span>
                        </div>
                     </div>
                  </el-tab-pane>
               </el-tabs>
            </el-card>
         </el-col>
      </el-row>
   </div>
</template>

<script setup name="Memos">
import { ElMessage } from 'element-plus'

const defaultAvatar = 'https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png'

const siteInfo = reactive({
  logo: 'http://memos.3139822.xyz/full-logo.webp',
  name: 'Memos',
  url: 'http://memos.3139822.xyz',
  description: '这是一个简洁的备忘录应用。'
})

const selectedTab = ref("memos")

const tags = ref(['工作', '学习', '生活', '灵感', '待办', '笔记', '想法', '计划'])

const memoList = ref([
  { id: 1, content: '这是第 1 条备忘录内容，记录一些重要的想法和待办事项...', date: '2024-06-01', tag: '工作' },
  { id: 2, content: '这是第 2 条备忘录内容，记录一些重要的想法和待办事项...', date: '2024-06-02', tag: '学习' },
  { id: 3, content: '这是第 3 条备忘录内容，记录一些重要的想法和待办事项...', date: '2024-06-03', tag: '生活' },
  { id: 4, content: '这是第 4 条备忘录内容，记录一些重要的想法和待办事项...', date: '2024-06-04', tag: '灵感' },
  { id: 5, content: '这是第 5 条备忘录内容，记录一些重要的想法和待办事项...', date: '2024-06-05', tag: '待办' }
])

const archiveList = ref([
  { month: '2024年6月', count: 15 },
  { month: '2024年5月', count: 23 },
  { month: '2024年4月', count: 18 },
  { month: '2024年3月', count: 12 },
  { month: '2024年2月', count: 9 },
  { month: '2024年1月', count: 6 }
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
.memo-list {
  padding: 10px 0;
}
.memo-item {
  padding: 16px;
  margin-bottom: 12px;
  border-radius: 8px;
  background: var(--el-fill-color-lighter);
  border: 1px solid var(--el-border-color-lighter);
}
.memo-item:last-child {
  margin-bottom: 0;
}
.memo-content p {
  color: var(--el-text-color-regular);
  font-size: 14px;
  line-height: 1.6;
  margin-bottom: 8px;
}
.memo-meta {
  display: flex;
  gap: 20px;
  font-size: 12px;
  color: var(--el-text-color-secondary);
}
.tag-cloud {
  padding: 20px;
}
.tag-item {
  margin: 6px;
  cursor: pointer;
}
.archive-list {
  padding: 20px;
}
.archive-item {
  padding: 10px 0;
  border-bottom: 1px solid var(--el-border-color-lighter);
  display: flex;
  align-items: center;
  gap: 10px;
}
.archive-item:last-child {
  border-bottom: none;
}
.archive-count {
  color: var(--el-text-color-secondary);
  font-size: 13px;
}
</style>
