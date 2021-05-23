<template>
  <div>
  	<div>
  		<div class="q-ma-sm h4 text-weight-bold">
  			Tambah Alarm
  		</div>
  		<div class="row">
  			<input type="text" class="col-sm-4 q-ma-md" v-model="title" placeholder="Judul Alarm">
	      <q-input filled v-model="caption" mask="time" :rules="['time']">
	        <template v-slot:append>
	          <q-icon name="access_time" class="cursor-pointer">
	            <q-popup-proxy transition-show="scale" transition-hide="scale">
	              <q-time v-model="caption">
	                <div class="row items-center justify-end">
	                  <q-btn v-close-popup label="Close" color="primary" flat />
	                </div>
	              </q-time>
	            </q-popup-proxy>
	          </q-icon>
	        </template>
	      </q-input>
      </div>
      <div class="row">
        <q-btn class="col" @click="OpenQr" label="OpenQr" />
        <q-input class="col" label="output" disable v-model="qrCode" />
      </div>
      <div class="row">
	      <q-btn color="primary" class="col q-ma-md" label="create" @click="CreateAlarm"  />
      </div>
  	</div>
  	<AlarmInf 
  		v-for="(alarmData, alarmIndex) in alarmDatas"
  		v-bind:key="alarmData.id"
  		v-bind:alarmData="alarmData"
      v-bind:alarmIndex="alarmIndex"
      @delete-alarm="OnDeleteAlarm"
      @update-alarm="OnUpdateAlarm"
  	/>
  </div>
</template>

<script>

import {Quasar, QTime, LocalStorage} from 'quasar'
import AlarmInf from 'components/AlarmInf'

export default {
  name: 'Alarm',
  components: {
  	AlarmInf,
  	QTime
  },
  data () {
  	return {
  		alarmDatas: [],
  		title: null,
  		caption: null,
      qrCode: null
  	}
  },
  created () {
    this.GetAlarmDatas()
  },
  methods: {
    GetAlarmDatas () {
      this.alarmDatas = LocalStorage.getItem('alarmDatas') || []
    },
    SaveToStorage () {
      LocalStorage.set('alarmDatas', this.alarmDatas)
    },
  	CreateAlarm() {
  		const params = {
  			title: this.title,
  			caption: this.caption,
        qrCode: this.qrCode
  		}
  		this.alarmDatas.push(params)
      this.SaveToStorage()
  		this.title = ''
  		this.caption = ''
      this.qrCode = ''
  		alert('Tambah Data Sukses')
  	},
    OnDeleteAlarm(alarmIndex, title) {
      alert(`Berhasil Terhapus ${title}`)
      this.alarmDatas.splice(alarmIndex, 1)
      this.SaveToStorage()
    },
    OnUpdateAlarm(alarmData) {
      this.alarmDatas[alarmData.index].title = alarmData.title
      this.SaveToStorage()
    },
    OpenQr() {
      cordova.plugins.barcodeScanner.scan(
        result => {
          alert(`SCAN SUKSES ${JSON.stringify(result)}`)
          console.log(`SCAN SUKSES`, result)
          this.qrCode = result.text
        },
        error => {
          alert(`SCAN ERROR ${error}`)
          console.log(`SCAN ERROR`, error)
        }
      )
    }
  }
}
</script>
