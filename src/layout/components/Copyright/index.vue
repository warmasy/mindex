<template>
  <footer v-if="visible" class="copyright" :style="copyrightStyle">
    <span class="copyright-left">{{ copyrightText }}</span>
    <span class="copyright-right">{{ icpText }}</span>
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
  height: '36px',
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
}

.copyright-right {
  text-align: right;
}
</style>
