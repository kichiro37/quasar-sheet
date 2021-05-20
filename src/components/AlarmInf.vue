<template>
  <div class="bg-red row q-pa-sm q-ma-sm">
  	<div class="col text-center">
  		{{alarmData.title}}	
  	</div>
  	<div class="col text-center">
  		{{alarmData.caption}}
  	</div>
  </div>
</template>

<script>
export default {
  name: 'Alarminf',
  props: {
  	alarmData: Object
  }
}
</script>
