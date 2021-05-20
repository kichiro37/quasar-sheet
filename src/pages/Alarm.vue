<template>
  <div>
    <AlarmInf
      v-for="alarmData in alarmDatas"
      v-bind:key="alarmData.title"
      v-bind:alarmData="alarmData"
    />
  </div>
</template>

<script>
import AlarmInf from 'components/AlarmInf'

const alarmDatas = [
  {
    title: 'Wake Up',
    time: '04:30',
    day: ['Mon'],
    isActive: true
  },
  {
    title: 'Buncin',
    time: '17:30',
    day: ['Mon'],
    isActive: true
  },
  {
    title: 'Kirim Paket',
    time: '12:30',
    day: ['Mon'],
    isActive: true
  },
]

export default {
  name: 'Alarm',
  components: {
    AlarmInf
  },
  data () {
    return {
      alarmDatas: alarmDatas
    }
  }
}
</script>
