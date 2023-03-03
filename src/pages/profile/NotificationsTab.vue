<template>
  <div class="pt-2">
    <div class="mb-2"></div>
    <!-- <div class="row">
      <div class="flex sm12 md6"></div>
    </div> -->
    <div class="va-table-responsive">
      <table class="va-table full-table va-table--striped">
        <tbody>
          <tr>
            <td>{{ t('profile.tabs.notification.emailNotifications') }}</td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="emailNotifications" size="small" off-color="#c9ced5" @click="changeNotificationEvent(emailNotifications, 'emailNotifications')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.phoneNotifications') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="phoneNotifications" size="small" off-color="#c9ced5" @click="changeNotificationEvent(phoneNotifications, 'phoneNotifications')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.alerting') }} </td>
            <td>
              <div class="centerConainer">
                <va-switch v-model="alerting" size="small" off-color="#c9ced5" @click="changeNotificationEvent(alerting, 'alerting')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.minNotificationInterval') }}(h)</td>
            <td>
              <div>
                <va-input v-model="minNotificationInterval" class="mb-6" @blur="changeNotificationEvent(minNotificationInterval, 'minNotificationInterval')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.windSpeed') }}  (kts)</td>
            <td>
              <div>
                <va-input v-model="windSpeed" class="mb-6" @blur="changeNotificationEvent(windSpeed, 'windSpeed')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.outdoorTemperature') }} (C)</td>
            <td>
              <div>
                <va-input v-model="outdoorTemp" class="mb-6" @blur="changeNotificationEvent(outdoorTemp, 'outdoorTemp')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.indoorTemperature') }}(C)</td>
            <td>
              <div>
                <va-input v-model="indoorTemp" class="mb-6" @blur="changeNotificationEvent(indoorTemp, 'indoorTemp')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.waterTemperature') }} </td>
            <td>
              <div>
                <va-input v-model="waterTemp" class="mb-6" @blur="changeNotificationEvent(waterTemp, 'waterTemp')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.waterDepth') }}(m)</td>
            <td>
              <div>
                <va-input v-model="waterDepth" class="mb-6" @blur="changeNotificationEvent(waterDepth, 'waterDepth')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.pressure') }} (hpa)</td>
            <td>
              <div>
                <va-input v-model="pressure" class="mb-6" @blur="changeNotificationEvent(pressure, 'pressure')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.pressureDeop') }}  12 {{ t('profile.tabs.notification.hours') }}  (hpa)</td>
            <td>
              <div>
                <va-input v-model="presureDrop" class="mb-6" @blur="changeNotificationEvent(presureDrop, 'presureDrop')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.batteryCharge') }} (%)</td>
            <td>
              <div>
                <va-input v-model="batteryCharge" class="mb-6" @blur="changeNotificationEvent(batteryCharge, 'batteryCharge')"/>
              </div>
            </td>
          </tr>
          <tr>
            <td>{{ t('profile.tabs.notification.batteryVoltage') }} (V)</td>
            <td>
              <div>
                <va-input v-model="batteryBoltage" class="mb-6" @blur="changeNotificationEvent(batteryBoltage, 'batteryBoltage')"/>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { useI18n } from 'vue-i18n'
  import { ref, onMounted } from 'vue'
  import { useGlobalStore } from '../../stores/global-store'

  const { t } = useI18n()

  const emailNotifications = ref(false)
  const phoneNotifications = ref(false)
  const alerting = ref(false)
  const minNotificationInterval = ref(6)
  const windSpeed = ref(0)
  const outdoorTemp = ref(0)
  const indoorTemp = ref(0)
  const waterTemp = ref(0)
  const waterDepth = ref(0)
  const pressure = ref()
  const presureDrop = ref()
  const batteryCharge = ref()
  const batteryBoltage = ref()

  onMounted(async () => {
    const GlobalStore = useGlobalStore()
    try {
      const response = await GlobalStore.fetchSettings()
      console.log("fetchSettings: ", response)
      emailNotifications.value = response.preferences.email_notifications
      phoneNotifications.value = response?.phone_notifications ? response?.phone_notifications : false
      alerting.value = response?.alerting ? response?.alerting : false
      minNotificationInterval.value = response?.preferences?.alerting?.minNotificationInterval ? response?.preferences?.alerting?.minNotificationInterval : ''
      windSpeed.value = response?.preferences?.alerting?.high_wind_speed_threshold ? response?.preferences?.alerting?.high_wind_speed_threshold : ''
      outdoorTemp.value = response?.preferences?.alerting?.low_outdoor_temperature_threshold ? response?.preferences?.alerting?.low_outdoor_temperature_threshold : ''
      indoorTemp.value = response?.preferences?.alerting?.low_indoor_temperature_threshold ? response?.preferences?.alerting?.low_indoor_temperature_threshold : ''
      waterTemp.value = response?.preferences?.alerting?.low_water_temperature_threshold ? response?.preferences?.alerting?.low_water_temperature_threshold : ''
      waterDepth.value = response?.preferences?.alerting?.low_water_depth_threshold ? response?.preferences?.alerting?.low_water_depth_threshold : ''
      pressure.value = response?.preferences?.alerting?.low_pressure_threshold ? response?.preferences?.alerting?.low_pressure_threshold : ''
      presureDrop.value = response?.preferences?.alerting?.high_pressure_drop_threshold ? response?.preferences?.alerting?.high_pressure_drop_threshold : ''
      batteryCharge.value = response?.preferences?.alerting?.low_battery_charge_threshold ? response?.preferences?.alerting?.low_battery_charge_threshold : ''
      batteryBoltage.value = response?.preferences?.alerting?.low_battery_voltage_threshold ? response?.preferences?.alerting?.low_battery_voltage_threshold : ''
    } catch (e) {
      console.log(e)
    }
  })

  const changeNotificationEvent = (e: any, type: string) => {
    console.log(type, e)
  }
</script>

<style lang="scss" scoped>
  .va-input-wrapper {
    margin-bottom: 1rem;
  }
  .centerConainer {
    display: flex;
    justify-content: center;
  }
  .full-table {
    width: 100%;
  }
</style>
