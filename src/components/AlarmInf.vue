<template>
  <div class="bg-info row q-py-md q-my-sm q-gutter-x-xs">
    <div class="col-2 text-center q-ma-auto q-gutter-x-xs">
     <q-btn color="negative" padding="none" icon="delete" @click="$emit('delete-alarm', alarmIndex, alarmData.title)"> </q-btn>
     <q-btn color="positive " padding="none" icon="edit" @click="UpdateAlarm(alarmIndex, alarmData)"> </q-btn>
    </div>
  	<div class="col-4 text-center text-white">
  		{{alarmData.title}}	
  	</div>
  	<div class="col-1 text-center text-white">
  		{{alarmData.time}}
  	</div>
    <div class="col text-center text-white">
      {{alarmData.qrCode}}
    </div>
  </div>
</template>

<script>
export default {
  name: 'Alarminf',
  props: {
  	alarmData: Object,
    alarmIndex: Number
  },
  methods: {
    UpdateAlarm(alarmIndex, alarmData) {
      const params = {
        index : alarmIndex,
        title : alarmData.title,
        time : alarmData.time,
        qrCode : alarmData.qrCode
      }
      this.$emit('update-alarm', params)
    }
  }
}
</script>
