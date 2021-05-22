<template>
  <div class="bg-red row q-pa-sm q-ma-sm">
    <div class="col-md-1 text-center">
     <button @click="$emit('delete-alarm', alarmIndex)"> X </button>
    </div>
     <div class="col-md-1 text-center">
     <button @click="UpdateAlarm(alarmIndex, alarmData.title)"> EDIT </button>
    </div>
  	<div class="col text-center text-white">
  		{{alarmData.title}}	
  	</div>
  	<div class="col text-center text-white">
  		{{alarmData.caption}}
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
    UpdateAlarm(alarmIndex, title) {
      const new_title = prompt ('Judul Baru', title)
      const params = {
        index : alarmIndex,
        title : new_title
      }
      this.$emit('update-alarm', params)
    }
  }
}
</script>
