<script setup lang="ts">
import { defineComponent, h } from 'vue'
import type {
  GlobalTheme, GlobalThemeOverrides,
  NDateLocale, zhCN,
} from 'naive-ui'
import {
  NDialogProvider,
  NLoadingBarProvider,
  NMessageProvider,
  NNotificationProvider,
  useDialog,
  useLoadingBar,
  useMessage,
  useNotification,
} from 'naive-ui'

interface Props {
  themeOverrides?: GlobalThemeOverrides
  theme?: GlobalTheme
  locale?: typeof zhCN
  dateLocale?: NDateLocale
}
const props = defineProps<Props>()
// 挂载naive组件的方法至window, 以便在路由钩子函数和请求函数里面调用
function registerNaiveTools() {
  window.$loadingBar = useLoadingBar()
  window.$dialog = useDialog()
  window.$message = useMessage()
  window.$notification = useNotification()
}
const NaiveProviderContent = defineComponent({
  setup() {
    onMounted(() => {
      registerNaiveTools()
    })
  },
  render() {
    return h('div')
  },
})
</script>

<template>
  <n-config-provider
    abstract
    :theme="props.theme"
    :theme-overrides="props.themeOverrides"
    :locale="props.locale"
    :date-locale="props.dateLocale"
  >
    <NLoadingBarProvider>
      <NDialogProvider>
        <NNotificationProvider>
          <NMessageProvider>
            <slot />
            <NaiveProviderContent />
          </NMessageProvider>
        </NNotificationProvider>
      </NDialogProvider>
    </NLoadingBarProvider>
    <n-global-style />
  </n-config-provider>
</template>

<style scoped></style>
