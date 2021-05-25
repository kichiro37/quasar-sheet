<template>
  <div>
  	<div class="q-mx-md">
      <div class="row q-gutter-x-sm q-my-sm">
        <q-input type="text" outlined class="col" v-model="search" label="Cari Alarm" >
          <template v-slot:append>
            <q-icon name="search" @click="SearchQr" class="cursor-pointer" />
          </template>
        </q-input>
        <q-btn @click="SearchAlarm" class="col-2" icon-right="send" color="positive" />
      </div>
      <hr class="q-my-lg" />
  		<div class="h4 text-weight-bold">
  			Tambah Alarm
  		</div>
  		<div class="row q-gutter-x-sm">
  			<q-input type="text" class="col" v-model="title" label="Judul Alarm" />
	      <q-input class="col" v-model="time" mask="time" label="Waktu" :rules="['time']">
	        <template v-slot:append>
	          <q-icon name="access_time" class="cursor-pointer">
	            <q-popup-proxy transition-show="scale" transition-hide="scale">
	              <q-time v-model="time">
	                <div class="row items-center justify-end">
	                  <q-btn v-close-popup label="Close" color="primary" flat />
	                </div>
	              </q-time>
	            </q-popup-proxy>
	          </q-icon>
	        </template>
	      </q-input>
      </div>
      <div class="row q-gutter-sm">
        <q-input class="col" label="output" v-model="qrCode" />
        <q-btn class="col" @click="OpenQr" color="positive" label="OpenQr" />
      </div>
      <div class="row q-py-md">
	      <q-btn v-if="indexAlarm" color="positive" class="col" label="Update" @click="UpdateAlarm"  />
	      <q-btn v-else color="primary" class="col" label="create" @click="CreateAlarm"  />
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
  		time: null,
      qrCode: null,
      search: null,
      indexAlarm: null
  	}
  },
  created () {
    this.GetAlarmDatas()
  },
  methods: {
    ScanQr () {
      return new Promise ((resolve, reject) => {
        cordova.plugins.barcodeScanner.scan(
          result => {
            resolve(result)
          },
          error => {
            reject(error)
          }
        )
      })
    },
    async SearchQr () {
      this.search = await this.ScanQr()
      this.search = this.search.text
    },
    SearchAlarm () {
      console.log('SearchAlarm 0', this.alarmDatas)
      console.log('SearchAlarm 1', this.search)
      const alarmData = this.alarmDatas.filter((alarmData, indexAlarm) => {
        console.log('SearchAlarm 2', alarmData.qrCode === this.search)
        this.indexAlarm = indexAlarm
        return alarmData.qrCode === this.search
      })
      console.log('SearchAlarm 3', alarmData)
      if (!alarmData.length) alert('data Not Found')
      else {
        this.title = alarmData[0].title
        this.time = alarmData[0].time
        this.qrCode = alarmData[0].qrCode
      }
    },
    GetAlarmDatas () {
      this.alarmDatas = LocalStorage.getItem('alarmDatas') || []
    },
    SaveToStorage () {
      LocalStorage.set('alarmDatas', this.alarmDatas)
    },
  	CreateAlarm() {
  		const params = {
  			title: this.title,
  			time: this.time,
        qrCode: this.qrCode
  		}
  		this.alarmDatas.push(params)
      this.SaveToStorage()
  		this.title = ''
  		this.time = ''
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
    async OpenQr() {
      this.qrCode = await this.ScanQr()
      this.qrCode = this.qrCode.text
    }
  }
}
</script>
