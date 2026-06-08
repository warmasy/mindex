<template>
  <div class="dashboard-wrapper">
    <div class="pageHeaderContent">
      <div class="avatar">
        <el-avatar size="large" :src="currentUser.avatar" />
      </div>
      <div class="content">
        <div class="contentTitle">
          早安，{{ currentUser.name }}，祝你开心每一天！
        </div>
		    <div>{{ currentUser.title }} | {{ currentUser.group }}</div>
      </div>
      <div class="extraContent">
        <div class="statItem">
          <el-statistic title="项目数" :value="0" />
        </div>
        <div class="statItem">
          <el-statistic title="团队内排名" :value="0" suffix="/ 0" />
        </div>
        <div class="statItem">
          <el-statistic title="项目访问" :value="0" />
        </div>
      </div>
    </div>

    <div class="main-content">
      <el-row :gutter="16">
        <el-col :span="16">
          <el-card class="projectList" :body-style="{ padding: '0px' }">
            <template #header>
              <div class="card-header">
                <span>进行中的项目</span>
                <a href="" target="_blank" rel="noopener noreferrer">
                  <span style="color: var(--el-color-primary)">全部项目</span>
                </a>
              </div>
            </template>
            <div class="project-grid-wrapper">
              <div
                v-for="item in projectNotice"
                :key="item.id"
                class="projectGrid"
              >
                <div class="grid-inner">
                  <div class="card-meta">
                    <div class="cardTitle">
                      <el-avatar size="small" :src="item.logo" />
                      <a :href="item.href" target="_blank" rel="noopener noreferrer">{{ item.title }}</a>
                    </div>
                    <div class="meta-description">{{ item.description }}</div>
                  </div>
                  <div class="projectItemContent">
                    <a :href="item.memberLink" target="_blank" rel="noopener noreferrer">{{ item.member || "" }}</a>
                    <span class="datetime" :title="item.updatedAt">
                      {{ item.updatedAt }}
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </el-card>

          <el-card class="activeCard" :body-style="{ padding: '0px' }" style="margin-top: 16px;">
            <template #header>
              <div class="card-header"><span>动态</span></div>
            </template>
            <div class="activitiesList">
              <div
                v-for="item in activities"
                :key="item.id"
                class="activity-item"
              >
                <div class="activity-avatar">
                  <el-avatar :src="item.user.avatar" />
                </div>
                <div class="activity-content">
                  <div class="activity-title">
                    <a class="username">{{ item.user.name }}</a>&nbsp;
                    <span class="event">
                      <span>{{ item.template1 }}</span>&nbsp;
                      <a href="" target="_blank" rel="noopener noreferrer" style="color: var(--el-color-primary)">
                        {{ item?.group?.name }}
                      </a>&nbsp;
                      <span>{{ item.template2 }}</span>&nbsp;
                      <a href="" target="_blank" rel="noopener noreferrer" style="color: var(--el-color-primary)">
                        {{ item?.project?.name }}
                      </a>
                    </span>
                  </div>
                  <div class="activity-description">
                    <span class="datetime" :title="item.updatedAt">
                      {{ item.updatedAt }}
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </el-card>
        </el-col>

        <el-col :span="8">
          <el-card :body-style="{ padding: '16px' }">
            <template #header>
              <div class="card-header">
                <span
                  class="quote-title"
                  @click="refreshQuote"
                  style="cursor: pointer"
                  >今日一言</span
                >
              </div>
            </template>
            <div class="daily-quote">
              <div class="quote-text">{{ currentQuote.text }}</div>
              <div class="quote-author">—— {{ currentQuote.author }}</div>
            </div>
          </el-card>

          <el-card style="margin-top: 16px;">
            <template #header>
              <div class="card-header"><span>日历</span></div>
            </template>
            <div class="compact-calendar">
              <div class="calendar-toolbar">
                <span class="calendar-month">{{ calendarYearMonth }}</span>
                <span class="calendar-ganzhi">{{ ganzhiFull }}</span>
                <div class="calendar-btns">
                  <el-button text size="small" @click="prevMonth">上个月</el-button>
                  <el-button text size="small" @click="goToday">今天</el-button>
                  <el-button text size="small" @click="nextMonth">下个月</el-button>
                </div>
              </div>
              <table class="compact-calendar-table">
                <thead>
                  <tr>
                    <th class="week-col">周数</th>
                    <th>一</th>
                    <th>二</th>
                    <th>三</th>
                    <th>四</th>
                    <th>五</th>
                    <th>六</th>
                    <th>日</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(week, wIdx) in calendarWeeks" :key="wIdx">
                    <td class="week-col">{{ week.weekNum }}</td>
                    <td
                      v-for="(day, dIdx) in week.days"
                      :key="dIdx"
                      :class="{
                        'other-month': !day.isCurrentMonth,
                        'is-today': day.isToday,
                        'is-selected': day.isSelected,
                      }"
                      @click="selectDate(day)"
                    >
                      <div class="day-circle">
                        {{ day.date ? day.date.getDate() : "" }}
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script setup>
import useSettingsStore from "@/store/modules/settings";

const settingsStore = useSettingsStore();

defineOptions({
  name: "DashBoard",
});

const currentUser = {
  avatar: "https://gw.alipayobjects.com/zos/rmsportal/BiazfanxmamNRoxxVxka.png",
  name: "吴彦祖",
  userid: "00000001",
  email: "antdesign@alipay.com",
  signature: "海纳百川，有容乃大",
  title: "地球online",
  group: "一定要好好的哦 ！！",
};

const projectNotice = [
  {
    id: "0001",
    title: "博客",
    logo: "https://myblog.3139822.xyz/favicon.ico", //https://gw.alipayobjects.com/zos/rmsportal/WdGqmHpayyMjiEhcKoVE.png
    description: "那是一种内在的东西，他们到达不了，也无法触及的",
    updatedAt: "几天前",
    member: "醉摘镜中花",
    href: "https://myblog.3139822.xyz/",
    memberLink: "https://myblog.3139822.xyz/",
  },
  {
    id: "0002",
    title: "Memos",
    logo: "http://memos.3139822.xyz/full-logo.webp", //https://gw.alipayobjects.com/zos/rmsportal/zOsKZmFRdUtvpqCImOVY.png
    description: "希望是一个好东西，也许是最好的，好东西是不会消亡的",
    updatedAt: "几天前",
    member: "你好呀",
    href: "http://memos.3139822.xyz/",
    memberLink: "http://memos.3139822.xyz/",
  },
  // {
  //   id: "0003",
  //   title: "XXX",
  //   logo: "https://gw.alipayobjects.com/zos/rmsportal/dURIMkkrRFpPgTuzkwnB.png",
  //   description: "城镇中有那么多的酒馆，她却偏偏走进了我的酒馆",
  //   updatedAt: "几月前",
  //   member: "中二少女团",
  //   href: "",
  //   memberLink: "",
  // },
  // {
  //   id: "0004",
  //   title: "XXX",
  //   logo: "https://gw.alipayobjects.com/zos/rmsportal/sfjbOqnsXXJgNCjCzDBL.png",
  //   description: "那时候我只会想自己想要什么，从不想自己拥有什么",
  //   updatedAt: "几年前",
  //   member: "程序员日常",
  //   href: "",
  //   memberLink: "",
  // },
  // {
  //   id: "0005",
  //   title: "XXX",
  //   logo: "https://gw.alipayobjects.com/zos/rmsportal/siCrBXXhmvTQGWPNLBow.png",
  //   description: "凛冬将至",
  //   updatedAt: "6 年前",
  //   member: "高逼格设计天团",
  //   href: "",
  //   memberLink: "",
  // },
  // {
  //   id: "0006",
  //   title: "XXX",
  //   logo: "https://gw.alipayobjects.com/zos/rmsportal/kZzEzemZyKLKFsojXItE.png",
  //   description: "生命就像一盒巧克力，结果往往出人意料",
  //   updatedAt: "几秒前",
  //   member: "天黑了",
  //   href: "",
  //   memberLink: "",
  // },
];

const activities = [
  {
    id: "trend-1",
    updatedAt: "几天前",
    user: {
      name: "大老虎",
      avatar: "https://gw.alipayobjects.com/zos/rmsportal/BiazfanxmamNRoxxVxka.png",
    },
    group: {
      name: "大山谷",
      // link: "http://github.com/",
    },
    project: {
      name: "Weblog",
      // link: "http://github.com/",
    },
    template1: "在",
    template2: "更新了",
  },
  // {
  //   id: "trend-2",
  //   updatedAt: "几秒前",
  //   user: {
  //     name: "付小小",
  //     avatar: "https://gw.alipayobjects.com/zos/rmsportal/cnrhVkzwxjPwAaCfPbdc.png",
  //   },
  //   group: {
  //     name: "高逼格设计天团",
  //     link: "http://github.com/",
  //   },
  //   project: {
  //     name: "六月迭代",
  //     link: "http://github.com/",
  //   },
  //   template1: "在",
  //   template2: "新建项目",
  // },
  // {
  //   id: "trend-3",
  //   updatedAt: "几秒前",
  //   user: {
  //     name: "林东东",
  //     avatar: "https://gw.alipayobjects.com/zos/rmsportal/gaOngJwsRYRaVAuXXcmB.png",
  //   },
  //   group: {
  //     name: "中二少女团",
  //     link: "http://github.com/",
  //   },
  //   project: {
  //     name: "六月迭代",
  //     link: "http://github.com/",
  //   },
  //   template1: "在",
  //   template2: "新建项目",
  // },
];

// ===================== 日历逻辑 =====================
const currentMonth = ref(new Date());
const selectedDate = ref(new Date());

const calendarYearMonth = computed(() => {
  const y = currentMonth.value.getFullYear();
  const m = currentMonth.value.getMonth() + 1;
  return `${y}年 ${m}月`;
});

const ganzhiFull = computed(() => {
  const date = selectedDate.value;
  const year = date.getFullYear();
  const month = date.getMonth() + 1;
  const day = date.getDate();

  const tiangan = ['甲', '乙', '丙', '丁', '戊', '己', '庚', '辛', '壬', '癸'];
  const dizhi = ['子', '丑', '寅', '卯', '辰', '巳', '午', '未', '申', '酉', '戌', '亥'];

  const yearTg = (year - 4) % 10;
  const yearDz = (year - 4) % 12;
  const yearGz = tiangan[yearTg] + dizhi[yearDz];

  let firstMonthGan;
  if (yearTg === 0 || yearTg === 5) firstMonthGan = 2;
  else if (yearTg === 1 || yearTg === 6) firstMonthGan = 4;
  else if (yearTg === 2 || yearTg === 7) firstMonthGan = 6;
  else if (yearTg === 3 || yearTg === 8) firstMonthGan = 8;
  else firstMonthGan = 0;

  const monthTg = (firstMonthGan + month - 1) % 10;
  const monthDz = (month + 1) % 12;
  const monthGz = tiangan[monthTg] + dizhi[monthDz];

  const baseDate = new Date(2026, 0, 1);
  const baseTg = 4;
  const baseDz = 2;
  const delta = Math.floor((date - baseDate) / (1000 * 60 * 60 * 24));
  const dayTg = (baseTg + delta) % 10;
  const dayDz = (baseDz + delta) % 12;
  const dayGz = tiangan[(dayTg + 10) % 10] + dizhi[(dayDz + 12) % 12];

  return `${yearGz}年 ${monthGz}月 ${dayGz}日`;
});

function getISOWeek(date) {
  const tmp = new Date(date.getTime());
  tmp.setHours(0, 0, 0, 0);
  tmp.setDate(tmp.getDate() + 4 - (tmp.getDay() || 7));
  const yearStart = new Date(tmp.getFullYear(), 0, 1);
  const weekNo = Math.ceil((((tmp - yearStart) / 86400000) + 1) / 7);
  return weekNo;
}

function isSameDay(d1, d2) {
  return d1 && d2 &&
    d1.getFullYear() === d2.getFullYear() &&
    d1.getMonth() === d2.getMonth() &&
    d1.getDate() === d2.getDate();
}

const calendarWeeks = computed(() => {
  const year = currentMonth.value.getFullYear();
  const month = currentMonth.value.getMonth();
  const firstDayOfMonth = new Date(year, month, 1);
  const lastDayOfMonth = new Date(year, month + 1, 0);

  let startDay = firstDayOfMonth.getDay();
  if (startDay === 0) startDay = 7;
  startDay = startDay - 1;

  const totalDays = lastDayOfMonth.getDate();
  const weeks = [];
  let currentWeek = [];
  let weekNum = null;

  for (let i = 0; i < startDay; i++) {
    currentWeek.push({ date: null, isCurrentMonth: false, isToday: false, isSelected: false });
  }

  for (let d = 1; d <= totalDays; d++) {
    const date = new Date(year, month, d);
    if (weekNum === null) weekNum = getISOWeek(date);
    currentWeek.push({
      date,
      isCurrentMonth: true,
      isToday: isSameDay(date, new Date()),
      isSelected: isSameDay(date, selectedDate.value),
    });
    if (currentWeek.length === 7) {
      weeks.push({ weekNum, days: currentWeek });
      currentWeek = [];
      weekNum = null;
    }
  }

  if (currentWeek.length > 0) {
    while (currentWeek.length < 7) {
      currentWeek.push({ date: null, isCurrentMonth: false, isToday: false, isSelected: false });
    }
    weeks.push({ weekNum: weekNum || getISOWeek(new Date(year, month, totalDays)), days: currentWeek });
  }

  return weeks;
});

function prevMonth() {
  const d = new Date(currentMonth.value);
  d.setMonth(d.getMonth() - 1);
  currentMonth.value = d;
}

function nextMonth() {
  const d = new Date(currentMonth.value);
  d.setMonth(d.getMonth() + 1);
  currentMonth.value = d;
}

function goToday() {
  currentMonth.value = new Date();
  selectedDate.value = new Date();
}

function selectDate(day) {
  if (day.date) {
    selectedDate.value = day.date;
  }
}

// ===================== 今日一言 =====================
const quotes = [
  { text: "种一棵树最好的时间是十年前，其次是现在。", author: "Dambisa Moyo" },
  { text: "生活不是等待风暴过去，而是学会在雨中翩翩起舞。", author: "Vivian Greene" },
  { text: "每一个不曾起舞的日子，都是对生命的辜负。", author: "尼采" },
  { text: "既然选择了远方，便只顾风雨兼程。", author: "汪国真" },
  { text: "世界上只有一种真正的英雄主义，那就是认清生活的真相后依然热爱生活。", author: "罗曼·罗兰" },
  { text: "你若盛开，蝴蝶自来。", author: "佚名" },
  { text: "路漫漫其修远兮，吾将上下而求索。", author: "屈原" },
  { text: "不积跬步，无以至千里；不积小流，无以成江海。", author: "荀子" },
  { text: "长风破浪会有时，直挂云帆济沧海。", author: "李白" },
  { text: "天行健，君子以自强不息。", author: "《周易》" },
  { text: "知之者不如好之者，好之者不如乐之者。", author: "孔子" },
  { text: "千里之行，始于足下。", author: "老子" },
  { text: "博学之，审问之，慎思之，明辨之，笃行之。", author: "《中庸》" },
  { text: "纸上得来终觉浅，绝知此事要躬行。", author: "陆游" },
];

const currentQuoteIndex = ref(Math.floor(Math.random() * quotes.length));
const currentQuote = computed(() => quotes[currentQuoteIndex.value]);

function refreshQuote() {
  let newIndex;
  do {
    newIndex = Math.floor(Math.random() * quotes.length);
  } while (newIndex === currentQuoteIndex.value && quotes.length > 1);
  currentQuoteIndex.value = newIndex;
}
</script>

<style scoped lang="less">
/* ========== 占满全宽：抵消外层 .app-container 的 20px padding ========== */
.dashboard-wrapper {
  width: 100%;
  margin: -20px;
  padding: 20px;
  box-sizing: content-box;
}

.main-content {
  padding: 0;
}

/* ========== Header ========== */
.pageHeaderContent {
  display: flex;
  padding: 12px 16px;
  margin-bottom: 0;
  box-shadow: var(--el-box-shadow-light);
  background: var(--el-bg-color);
  .avatar {
    flex: 0 1 72px;
    & > span {
      display: block;
      width: 72px;
      height: 72px;
      border-radius: 72px;
    }
  }
  .content {
    position: relative;
    top: 4px;
    flex: 1 1 auto;
    margin-left: 24px;
    color: var(--el-text-color-secondary);
    line-height: 22px;
    .contentTitle {
      margin-bottom: 12px;
      color: var(--el-text-color-primary);
      font-weight: 500;
      font-size: 20px;
      line-height: 28px;
    }
  }
}

.extraContent {
  float: right;
  white-space: nowrap;
  .statItem {
    position: relative;
    display: inline-block;
    padding: 0 32px;
    :deep(.el-statistic__head) {
      margin-bottom: 4px;
      color: var(--el-text-color-secondary);
      font-size: 14px;
      line-height: 22px;
    }
    :deep(.el-statistic__content) {
      margin: 0;
      color: var(--el-text-color-primary);
      font-size: 30px;
      line-height: 38px;
      .el-statistic__number {
        font-size: 30px;
        line-height: 38px;
      }
      .el-statistic__suffix {
        color: var(--el-text-color-secondary);
        font-size: 20px;
        margin-left: 4px;
      }
    }
    &::after {
      position: absolute;
      top: 8px;
      right: 0;
      width: 1px;
      height: 40px;
      background-color: var(--el-border-color);
      content: "";
    }
    &:last-child {
      padding-right: 0;
      &::after {
        display: none;
      }
    }
  }
}

/* ========== Card Header ========== */
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: 500;
}

/* ========== 项目网格 ========== */
.projectList {
  .project-grid-wrapper {
    display: flex;
    flex-wrap: wrap;
    border-top: 1px solid var(--el-border-color-lighter);
    border-left: 1px solid var(--el-border-color-lighter);
  }
  .projectGrid {
    width: 33.33%;
    border-right: 1px solid var(--el-border-color-lighter);
    border-bottom: 1px solid var(--el-border-color-lighter);
    box-sizing: border-box;
  }
  .grid-inner {
    padding: 20px;
  }
  .meta-description {
    height: 44px;
    overflow: hidden;
    color: var(--el-text-color-secondary);
    line-height: 22px;
    margin-top: 8px;
  }
  .cardTitle {
    font-size: 0;
    a {
      display: inline-block;
      height: 24px;
      margin-left: 12px;
      color: var(--el-text-color-primary);
      font-size: 14px;
      line-height: 24px;
      vertical-align: top;
      &:hover {
        color: var(--el-color-primary);
      }
    }
  }
  .projectItemContent {
    display: flex;
    flex-basis: 100%;
    height: 20px;
    margin-top: 16px;
    overflow: hidden;
    font-size: 12px;
    line-height: 20px;
    a {
      display: inline-block;
      flex: 1 1 0;
      color: var(--el-text-color-secondary);
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      word-break: break-all;
      &:hover {
        color: var(--el-color-primary);
      }
    }
    .datetime {
      flex: 0 0 auto;
      float: right;
      color: var(--el-text-color-placeholder);
    }
  }
}

/* ========== 动态列表 ========== */
.activitiesList {
  padding: 0 24px 8px 24px;
  .username {
    color: var(--el-text-color-regular);
  }
  .event {
    font-weight: normal;
  }
}

.activity-item {
  display: flex;
  align-items: flex-start;
  padding: 12px 0;
  border-bottom: 1px solid var(--el-border-color-lighter);
  &:last-child {
    border-bottom: none;
  }
}
.activity-avatar {
  flex-shrink: 0;
  margin-right: 16px;
}
.activity-content {
  flex: 1;
}
.activity-title {
  margin-bottom: 4px;
  color: var(--el-text-color-primary);
  font-size: 14px;
}
.activity-description {
  color: var(--el-text-color-secondary);
  font-size: 12px;
}

.datetime {
  color: var(--el-text-color-placeholder);
}

/* ========== 日历 ========== */
.compact-calendar {
  padding: 8px 12px;
}
.calendar-toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  gap: 8px;
}
.calendar-ganzhi {
  font-size: 13px;
  color: var(--el-color-primary);
  font-weight: 600;
  flex: 1;
  text-align: center;
}
.calendar-month {
  font-size: 16px;
  font-weight: 600;
  color: var(--el-text-color-primary);
}
.calendar-btns {
  display: flex;
  gap: 4px;
}
.calendar-btns .el-button {
  padding: 4px 10px;
  font-size: 13px;
}

.compact-calendar-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 2px;
  table-layout: fixed;
}
.compact-calendar-table th {
  text-align: center;
  font-size: 13px;
  color: var(--el-text-color-secondary);
  padding: 4px 2px;
  font-weight: 500;
}
.compact-calendar-table td {
  text-align: center;
  padding: 2px;
  cursor: pointer;
  vertical-align: middle;
}
.compact-calendar-table td.week-col {
  font-size: 13px;
  color: var(--el-text-color-secondary);
  width: 36px;
  cursor: default;
  vertical-align: middle;
  font-weight: 500;
  background-color: var(--el-color-primary-light-9);
  border-radius: 4px;
}
.day-circle {
  width: 32px;
  height: 32px;
  line-height: 32px;
  border-radius: 50%;
  margin: 0 auto;
  font-size: 13px;
  color: var(--el-text-color-primary);
  transition: all 0.2s;
}
.compact-calendar-table td:hover .day-circle {
  background-color: var(--el-color-primary-light-9);
}
.compact-calendar-table td.is-today .day-circle {
  color: var(--el-color-primary);
  font-weight: 700;
}
.compact-calendar-table td.is-selected .day-circle {
  background-color: var(--el-color-primary);
  color: #fff;
}
.compact-calendar-table td.other-month .day-circle {
  color: var(--el-text-color-placeholder);
}

/* ========== 今日一言 ========== */
.daily-quote {
  text-align: left;
}
.quote-title {
  cursor: pointer;
  user-select: none;
}
.quote-title:hover {
  color: var(--el-color-primary);
}
.quote-text {
  font-size: 14px;
  line-height: 1.5;
  color: var(--el-text-color-primary);
  margin-bottom: 8px;
}
.quote-author {
  font-size: 12px;
  color: var(--el-text-color-secondary);
  text-align: right;
}

/* ========== 响应式 ========== */
@media screen and (max-width: 1200px) and (min-width: 992px) {
  .extraContent {
    margin-left: -44px;
    .statItem {
      padding: 0 16px;
    }
  }
}

@media screen and (max-width: 992px) {
  .extraContent {
    float: none;
    margin-right: 0;
    .statItem {
      padding: 0 16px;
      text-align: left;
      &::after {
        display: none;
      }
    }
  }
  .projectList .projectGrid {
    width: 50%;
  }
}

@media screen and (max-width: 768px) {
  .extraContent {
    margin-left: -16px;
  }
}

@media screen and (max-width: 576px) {
  .pageHeaderContent {
    display: block;
    .content {
      margin-left: 0;
    }
  }
  .extraContent {
    .statItem {
      float: none;
    }
  }
  .projectList .projectGrid {
    width: 100%;
  }
}
</style>
