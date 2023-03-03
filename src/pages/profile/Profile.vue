<template>
  <div>
    <va-card>
      <va-card-title>
        {{ t('profile.title') }}
      </va-card-title>
      <va-card-content>
        <va-tabs v-model="activeTabName" grow>
          <template #tabs>
            <va-tab name="OverviewTab">
              {{ t('profile.tabs.overview.title') }}
            </va-tab>
            <va-tab name="NotificationsTab">
              {{ t('profile.tabs.notification.title') }}
            </va-tab>
          </template>
        </va-tabs>
        <va-separator />
        <component :is="tabs[activeTabName]" @submit="submit" />
      </va-card-content>
    </va-card>
  </div>
</template>

<script setup lang="ts">
  import { defineAsyncComponent, ref, onMounted } from 'vue'
  import { useI18n } from 'vue-i18n'
  import { useGlobalStore } from '../../stores/global-store'

  const { t } = useI18n()
  const isBusy = ref(false)
  const apiError = ref(false)
  const updateError = ref(null)

  const tabs = {
    OverviewTab: defineAsyncComponent(() => import('./OverviewTab.vue')),
    NotificationsTab: defineAsyncComponent(() => import('./NotificationsTab.vue')),
  }

  const emit = defineEmits<{
    (e: 'submit', data: any): void
  }>()

  const activeTabName = ref<keyof typeof tabs>('OverviewTab')

  function submit(data: any) {
    emit('submit', data)
  }

  onMounted(async () => {
    isBusy.value = true
    apiError.value = false
    const GlobalStore = useGlobalStore()
    try {
      GlobalStore.fetchSettings()
    } catch (e) {
      apiError.value = true
      console.warn('Get data from json...', apiError.value)
    } finally {
      isBusy.value = false
    }
  })
</script>

<style lang="scss"></style>
