<template>
  <div class="bg-info row q-pa-sm q-ma-sm">
    <div class="col-md-1 text-center">
     <q-btn color="negative" @click="$emit('delete-alarm', alarmIndex, alarmData.title)"> X </q-btn>
    </div>
     <div class="col-md-1 text-center">
     <q-btn color="positive" @click="UpdateAlarm(alarmIndex, alarmData)"> EDIT </q-btn>
    </div>
  	<div class="col q-pa-sm text-center text-white">
  		{{alarmData.title}}	
  	</div>
  	<div class="col q-pa-sm text-center text-white">
  		{{alarmData.time}}
  	</div>
    <div class="col q-pa-sm text-center text-white">
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
