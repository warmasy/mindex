<template>
  <footer v-if="visible" class="copyright" :style="copyrightStyle">
    <div class="copyright-left">
      <div class="copyright-badges">
        <a
          v-for="(badge, index) in badgeList"
          :key="index"
          :href="badge.href"
          target="_blank"
          rel="noopener noreferrer"
          class="badge-link"
        >
          <img :src="badge.img" :alt="badge.alt" class="badge-img" />
        </a>
      </div>
    </div>
    <div class="copyright-right">
      <a class="badge-tag icp-badge" href="https://beian.miit.gov.cn/" target="_blank" rel="noopener noreferrer">{{ icpText }}</a>
      <span class="badge-tag copyright-badge">{{ copyrightText }}</span>
    </div>
  </footer>
</template>

<script setup>
import useAppStore from '@/store/modules/app'
import useSettingsStore from '@/store/modules/settings'

const appStore = useAppStore()
const settingsStore = useSettingsStore()

const visible = computed(() => settingsStore.footerVisible)
const copyrightText = computed(() => settingsStore.footerContent || 'Copyright © 2024-2026 地球online 纯前端演示版 All Rights Reserved.')
const icpText = computed(() => settingsStore.footerIcp || '本项目为纯前端演示，数据均为本地Mock')
const badgeList = computed(() => settingsStore.footerBadges || [])

const sidebarOpened = computed(() => appStore.sidebar.opened)
const isMobile = computed(() => appStore.device === 'mobile')
const sidebarHide = computed(() => appStore.sidebar.hide)

const leftWidth = computed(() => {
  if (isMobile.value || sidebarHide.value) return '0px'
  return sidebarOpened.value ? '210px' : '54px'
})

const copyrightStyle = computed(() => ({
  position: 'fixed',
  bottom: '0',
  left: leftWidth.value,
  right: '0',
  height: '48px',
  padding: '0 20px',
  display: 'flex',
  alignItems: 'center',
  justifyContent: 'space-between',
  backgroundColor: 'var(--copyright-bg, #f8f8f8)',
  color: 'var(--copyright-color, #666)',
  fontSize: '13px',
  borderTop: '1px solid var(--copyright-border, #e7e7e7)',
  zIndex: 999,
  transition: 'left 0.28s'
}))
</script>

<style scoped>
.copyright-left {
  text-align: left;
  flex-shrink: 0;
}

.copyright-right {
  text-align: right;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  gap: 8px;
}

.copyright-badges {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
}

.badge-link {
  display: inline-flex;
  align-items: center;
  text-decoration: none;
  line-height: 0;
}

.badge-img {
  height: 20px;
  width: auto;
  display: block;
  border-radius: 3px;
}

/* ===== 右侧 badge 标签样式（类似 shields.io） ===== */
.badge-tag {
  display: inline-flex;
  align-items: center;
  height: 20px;
  padding: 0 8px;
  border-radius: 3px;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.2px;
  line-height: 1;
  white-space: nowrap;
  text-decoration: none;
  transition: opacity 0.2s;
}

.badge-tag:hover {
  opacity: 0.85;
}

/* 备案 badge - 蓝色系 */
.icp-badge {
  background-color: #007ec6;
  color: #fff;
  cursor: pointer;
}

/* 版权 badge - 灰色系 */
.copyright-badge {
  background-color: #555555;
  color: #fff;
}
</style>