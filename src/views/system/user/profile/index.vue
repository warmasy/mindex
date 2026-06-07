<template>
   <div class="app-container profile-page">
      <el-row :gutter="20" class="profile-row">
         <el-col :span="6" :xs="24">
            <el-card class="box-card info-card">
               <template v-slot:header>
                 <div class="clearfix">
                   <span>个人信息</span>
                 </div>
               </template>
               <div class="info-content">
                  <div class="text-center">
                     <userAvatar />
                  </div>
                  <ul class="list-group list-group-striped">
                     <li class="list-group-item">
                        <svg-icon icon-class="documentation" />网站名称
                        <div class="pull-right">地球online</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="phone" />手记号码
                        <div class="pull-right">88888888</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="email" />邮箱
                        <div class="pull-right">123@3139822.xyz</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="peoples" />角色
                        <div class="pull-right">NPC</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="date" />创建日期
                        <div class="pull-right">{{ state.user.createTime }}</div>
                     </li>
                  </ul>
               </div>
            </el-card>
         </el-col>
         <el-col :span="18" :xs="24">
            <el-card class="tab-card">
               <template v-slot:header>
                 <div class="clearfix">
                   <span>资料中心</span>
                 </div>
               </template>
               <el-tabs v-model="selectedTab">
                  <el-tab-pane label="平凡之路" name="userinfo">
                     <userInfo />
                  </el-tab-pane>
                  <el-tab-pane label="起风了" name="resetPwd">
                     <resetPwd />
                  </el-tab-pane>
                  <el-tab-pane label="晴天" name="tabC">
                     <div class="lyric-box">
                        <h3>《晴天》</h3>
                        <p>故事的小黄花 从出生那年就飘着</p>
                        <p>童年的荡秋千 随记忆一直晃到现在</p>
                        <p>Re So So Si Do Si La So La Si Si Si Si La Si La So</p>
                        <p>吹着前奏望着天空 我想起花瓣试着掉落</p>
                        <p>为你翘课的那一天 花落的那一天</p>
                        <p>教室的那一间 我怎么看不见</p>
                        <p>消失的下雨天 我好想再淋一遍</p>
                        <p>没想到失去的勇气我还留着</p>
                        <p>好想再问一遍 你会等待还是离开</p>
                        <p>刮风这天 我试过握着你手</p>
                        <p>但偏偏 雨渐渐 大到我看你不见</p>
                        <p>还要多久 我才能在你身边</p>
                        <p>等到放晴的那天 也许我会比较好一点</p>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="稻香" name="tabD">
                     <div class="lyric-box">
                        <h3>《稻香》</h3>
                        <p>对这个世界如果你有太多的抱怨</p>
                        <p>跌倒了就不敢继续往前走</p>
                        <p>为什么人要这么的脆弱 堕落</p>
                        <p>请你打开电视看看 多少人为生命在努力勇敢的走下去</p>
                        <p>我们是不是该知足 珍惜一切 就算没有拥有</p>
                        <p>还记得你说家是唯一的城堡</p>
                        <p>随着稻香河流继续奔跑</p>
                        <p>微微笑 小时候的梦我知道</p>
                        <p>不要哭让萤火虫带着你逃跑</p>
                        <p>乡间的歌谣永远的依靠</p>
                        <p>回家吧 回到最初的美好</p>
                     </div>
                  </el-tab-pane>
                  <el-tab-pane label="夜空中最亮的星" name="tabE">
                     <div class="lyric-box">
                        <h3>《夜空中最亮的星》</h3>
                        <p>夜空中最亮的星 能否听清</p>
                        <p>那仰望的人 心底的孤独和叹息</p>
                        <p>夜空中最亮的星 能否记起</p>
                        <p>曾与我同行 消失在风里的身影</p>
                        <p>我祈祷拥有一颗透明的心灵</p>
                        <p>和会流泪的眼睛</p>
                        <p>给我再去相信的勇气</p>
                        <p>越过谎言去拥抱你</p>
                        <p>每当我找不到存在的意义</p>
                        <p>每当我迷失在黑夜里</p>
                        <p>夜空中最亮的星</p>
                        <p>请指引我靠近你</p>
                     </div>
                  </el-tab-pane>
               </el-tabs>
            </el-card>
         </el-col>
      </el-row>
   </div>
</template>

<script setup name="Profile">
import userAvatar from "./userAvatar";
import userInfo from "./userInfo";
import resetPwd from "./resetPwd";
import { getUserProfile } from "@/api/system/user";

const route = useRoute()
const selectedTab = ref("userinfo")
const state = reactive({
  user: {},
  roleGroup: {},
  postGroup: {}
});

function getUser() {
  getUserProfile().then(response => {
    state.user = response.data;
    state.roleGroup = response.roleGroup;
    state.postGroup = response.postGroup;
  });
};

onMounted(() => {
  const activeTab = route.params && route.params.activeTab
  if (activeTab) {
    selectedTab.value = activeTab
  }
  getUser()
})
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
.list-group {
  flex: 1;
  padding-left: 0;
  list-style: none;
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
.lyric-box {
  padding: 20px;
  line-height: 2;
}
.lyric-box h3 {
  margin-bottom: 16px;
  color: var(--el-color-primary);
  font-size: 18px;
}
.lyric-box p {
  margin: 8px 0;
  color: var(--el-text-color-regular);
  font-size: 14px;
}
</style>
