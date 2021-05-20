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
    title: 'TANGI',
    caption: '06:00',
    icon: 'home',
  },
  {
    title: 'MASAK',
    caption: '08:00',
    icon: 'alarm',
  },
  {
    title: 'TIDUR',
    caption: '21:00',
    icon: 'volunteer_activism',
  },
]

export default {
  name: 'Alarm',
  components: {
  	AlarmInf,
  },
  data () {
  	return {
  		alarmDatas: alarmDatas
  	}
  }
}
</script>
