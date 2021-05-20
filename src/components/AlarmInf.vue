<template>
  <div class="bg-red row q-pa-md q-my-xs ">
    <div class="col"> {{alarmData.title}} </div>
    <div class="col text-right"> {{alarmData.time}} </div>
  </div>
</template>

<script>

export default {
  name: 'AlarmInf',
  props: {
    alarmData: Object
  }
}
</script>
