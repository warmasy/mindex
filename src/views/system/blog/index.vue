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
                  <el-tab-pane label="文章列表" name="articles">
                     <div class="article-list">
                        <div v-for="article in articleList" :key="article.id" class="article-item">
                           <h4>{{ article.title }}</h4>
                           <p>{{ article.summary }}</p>
                           <div class="article-meta">
                             <span>{{ article.date }}</span>
                             <span>阅读 {{ article.views }}</span>
                           </div>
                        </div>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="标签云" name="tags">
                     <div class="tag-cloud">
                        <el-tag v-for="tag in tags" :key="tag" class="tag-item">{{ tag }}</el-tag>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="关于" name="about">
                     <div class="about-box">
                        <h3>关于博客</h3>
                        <p>{{ siteInfo.description }}</p>
                     </div>
                  </el-tab-pane>
               </el-tabs>
            </el-card>
         </el-col>
      </el-row>
   </div>
</template>

<script setup name="Blog">
import { ElMessage } from 'element-plus'

const defaultAvatar = 'https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png'

const siteInfo = reactive({
  logo: 'https://www.baidu.com/img/flexible/logo/pc/result.png',
  name: '我的博客',
  url: 'https://www.baidu.com',
  description: '这是一个简洁的个人博客，记录学习与生活。'
})

const selectedTab = ref("articles")

const tags = ref(['Vue', 'JavaScript', 'Element Plus', '前端', '后端', '生活', '技术', '笔记'])

const articleList = ref([
  { id: 1, title: '博客文章标题 1', summary: '这是博客文章的摘要内容，展示文章的前几行文字...', date: '2024-06-01', views: 123 },
  { id: 2, title: '博客文章标题 2', summary: '这是博客文章的摘要内容，展示文章的前几行文字...', date: '2024-06-02', views: 234 },
  { id: 3, title: '博客文章标题 3', summary: '这是博客文章的摘要内容，展示文章的前几行文字...', date: '2024-06-03', views: 345 },
  { id: 4, title: '博客文章标题 4', summary: '这是博客文章的摘要内容，展示文章的前几行文字...', date: '2024-06-04', views: 456 },
  { id: 5, title: '博客文章标题 5', summary: '这是博客文章的摘要内容，展示文章的前几行文字...', date: '2024-06-05', views: 567 }
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
.article-list {
  padding: 10px 0;
}
.article-item {
  padding: 16px 0;
  border-bottom: 1px solid var(--el-border-color-lighter);
}
.article-item:last-child {
  border-bottom: none;
}
.article-item h4 {
  margin-bottom: 8px;
  color: var(--el-color-primary);
  font-size: 16px;
}
.article-item p {
  color: var(--el-text-color-regular);
  font-size: 14px;
  margin-bottom: 8px;
}
.article-meta {
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
.about-box {
  padding: 20px;
  line-height: 2;
}
.about-box h3 {
  margin-bottom: 16px;
  color: var(--el-color-primary);
}
</style>
