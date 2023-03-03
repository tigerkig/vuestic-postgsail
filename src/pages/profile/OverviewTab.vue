<template>
  <div class="overview-tab pt-2 layout">
    <div class="mb-2"></div>
    <!-- <div class="row">
      <div class="flex xs12 xl6 mb-5">
        <div class="overview-tab__item d-flex align--center">
          <div class="overview-tab__item-icon fill-height mr-2">
            <va-icon-vue />
          </div>
          <div class="text--bold">{{ t('dashboard.tabs.overview.built') }}</div>
        </div>
      </div>

      <div class="flex xs12 xl6 mb-5">
        <div class="overview-tab__item d-flex align--center">
          <div class="overview-tab__item-icon fill-height mr-2">
            <va-icon-responsive />
          </div>
          <div class="text--bold">{{ t('dashboard.tabs.overview.mobile') }}</div>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="flex xs12 xl6 mb-5">
        <div class="overview-tab__item d-flex align--center">
          <div class="overview-tab__item-icon fill-height mr-2">
            <va-icon-free />
          </div>
          <div class="text--bold">{{ t('dashboard.tabs.overview.free') }}</div>
        </div>
      </div>

      <div class="flex xs12 xl6 mb-5">
        <div class="overview-tab__item d-flex align--center">
          <div class="overview-tab__item-icon fill-height mr-2">
            <va-icon-rich />
          </div>
          <div class="text--bold">{{ t('dashboard.tabs.overview.components') }}</div>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="flex xs12 xl6 mb-5">
        <div class="overview-tab__item d-flex align--center">
          <div class="overview-tab__item-icon fill-height mr-2">
            <va-icon-fresh />
          </div>
          <div class="text--bold">{{ t('dashboard.tabs.overview.fresh') }}</div>
        </div>
      </div>

      <div class="flex xs12 xl6">
        <div class="overview-tab__item d-flex align--center">
          <div class="overview-tab__item-icon fill-height mr-2">
            <va-icon-clean-code />
          </div>
          <div class="text--bold">{{ t('dashboard.tabs.overview.nojQuery') }}</div>
        </div>
      </div>
    </div> -->

    <div class="va-table-responsive">
      <table class="va-table full-table va-table--striped">
        <tbody>
          <tr>
            <td>{{ t('profile.tabs.overview.name') }}</td>
            <td>{{ fullName }}</td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.preferredHomepage') }} </td>
            <td>
              <div>
                <va-select v-model="value" :options="options" :placeholder="value" @update:modelValue="changeEvent(value, 'preferredHomepage')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.userImperialUnits') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="imperialUnits" size="small" off-color="#c9ced5" @click="changeEvent(imperialUnits, 'imperialUnits')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.website') }} </td>
            <td>
              <div>
                <va-input v-model="websiteUrl" class="mb-6" outline="true" @blur="changeEvent(websiteUrl, 'websiteUrl')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.instagramHandle') }} </td>
            <td>
              <div>
                <va-input v-model="instagramUrl" class="mb-6" @blur="changeEvent(instagramUrl, 'instagramUrl')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.publicProfile') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="publicProfile" size="small" off-color="#c9ced5" @click="changeEvent(publicProfile, 'publicProfile')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.stats') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="stats" size="small" off-color="#c9ced5" @click="changeEvent(stats, 'stats')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.timelapse') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="timelapse" size="small" off-color="#c9ced5" @click="changeEvent(timelapse, 'timelapse')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.logList') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="loglist" size="small" off-color="#c9ced5" @click="changeEvent(loglist, 'loglist')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.overview.logDetails') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="logDetails" size="small" off-color="#c9ced5" @click="changeEvent(logDetails, 'logDetails')"/>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
  // import VaIconVue from '../../components/icons/VaIconVue.vue'
  // import VaIconFree from '../../components/icons/VaIconFree.vue'
  // import VaIconFresh from '../../components/icons/VaIconFresh.vue'
  // import VaIconResponsive from '../../components/icons/VaIconResponsive.vue'
  // import VaIconRich from '../../components/icons/VaIconRich.vue'
  // import VaIconCleanCode from '../../components/icons/VaIconCleanCode.vue'

  import { useI18n } from 'vue-i18n'
  import { ref, onMounted } from 'vue'
  import { useGlobalStore } from '../../stores/global-store'
  // import settingsData from '../../data/settings.json'

  const { t } = useI18n()

  const fullName = ref('')
  const options = ['Ship Logs', 'Stays', 'Units']
  const value = ref('Ship Logs')
  const websiteUrl = ref('')
  const imperialUnits = ref(false)
  const instagramUrl = ref('')
  const stats = ref(false)
  const timelapse = ref(false)
  const loglist = ref(false)
  const logDetails = ref(false)
  const publicProfile = ref(false)

  onMounted(async () => {
    const GlobalStore = useGlobalStore()
    try {
      const response = await GlobalStore.fetchSettings()
      fullName.value = response?.first + response?.last
      websiteUrl.value = response?.websiteUrl ? response?.websiteUrl : ''
      imperialUnits.value = response?.imperialUnits ? response?.imperialUnits : false
      instagramUrl.value = response?.instagramUrl ? response?.instagramUrl : ''
      stats.value = response?.stats ? response?.stats : false
      timelapse.value = response?.timelapse ? response?.timelapse : false
      loglist.value = response?.loglist ? response?.loglist : false
      logDetails.value = response?.logDetails ? response?.logDetails : false
      publicProfile.value = response?.publicProfile ? response?.publicProfile : false
    } catch (e) {
      console.log(e)
    }
  })

  const changeEvent = (e: any, type: string) => {
    console.log(type, e)
  }

</script>

<style lang="scss">
  .overview-tab {
    &__item {
      height: 55px;

      &-icon {
        min-width: 65px;
        max-width: 65px;
      }
    }
  }

  .centerConainer {
    display: flex;
    justify-content: center;
  }
  .full-table {
    width: 100%;
  } 
  .va-input-wrapper {
    --va-input-wrapper-background: rgba(67, 92, 161, 0.116);
  }
</style>
