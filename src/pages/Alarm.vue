<template>
  <div>
  	<div class="q-mx-md q-mt-md">
      <div class="row q-gutter-x-md">
        <q-input class="col" label="Cari Alarm" placeholder="kode QR" outlined v-model="search" >
          <template v-slot:append>
            <q-icon name="search" @click="SearchQR"/>
          </template>
        </q-input>
        <q-btn class="col-2" icon="send" color="primary" @click="SearchAlarm"/>
      </div>
      <hr class="q-my-lg" />
  		<div class="text-h5 text-weight-bold q-mb-md">
  			Tambah Alarm
  		</div>
  		<div class="row q-gutter-x-md">
  			<q-input type="text" class="col" v-model="title" placeholder="Judul Alarm" outlined/>
	      <q-input class="col-4" filled v-model="time" mask="time" :rules="['time']">
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
      <div class="row q-gutter-x-md">
        <q-input btn class="col" label="output" disable v-model="qrCode" />
         <q-btn class="col" color="positive" label="OpenQr" @click="OpenQr"/>
      </div>
      <div class="row">
        <q-btn v-if="alarmIndex != null" color="positive" class="col q-ma-md" label="update" @click="UpdateBaru(alarmDatas, alarmIndex)" 
        />
        <q-btn v-else color="primary" class="col q-ma-md" label="create" @click="CreateAlarm" />
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
  	QTime,
  },
  data () {
  	return {
  		alarmDatas: [],
  		title: null,
  		time: null,
      qrCode: null,
      search: null,
      alarmIndex: null
    }
  },
  created() {
    this.GetAlarmDatas()
  },
  methods: {
    ScanQR() {
      return new Promise((resolve, reject) => {
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
    async SearchQR() {
      this.search = await this.ScanQR()
      this.search = this.search.text
    },
    async OpenQr() {
      this.qrCode = await this.ScanQR()
      this.qrCode = this.qrCode.text
    },
    SearchAlarm() {
      const alarmData = this.alarmDatas.filter((alarmData, alarmIndex) => {
        const IS_FOUND = alarmData.qrCode === this.search 
        if (IS_FOUND) this.alarmIndex = alarmIndex
        return IS_FOUND
      })

      if(!alarmData.length) alert('Search not found')
      else {
        this.title = alarmData[0].title
        this.time = alarmData[0].time
        this.qrCode = alarmData[0].qrCode
      }

    },
    GetAlarmDatas() {
      this.alarmDatas = LocalStorage.getItem('alarmDatas') || []
    },
    SaveStorage() {
      LocalStorage.set('alarmDatas', this.alarmDatas)
    },
  	CreateAlarm() {
  		const params = {
  			title: this.title,
  			time: this.time,
        qrCode: this.qrCode
  		}
  		this.alarmDatas.push(params)
      this.SaveStorage()
  		this.title = ''
  		this.time = '00:00'
      this.qrCode = ''
  		alert('Tambah Data Sukses')
  	},
    OnDeleteAlarm(alarmIndex, title) {
      alert(`"${title}" - Berhasil Terhapus `)
      this.alarmDatas.splice(alarmIndex, 1)
      this.SaveStorage()
    },
    OnUpdateAlarm(alarmData) {
      this.alarmIndex = alarmData.index
      this.title = alarmData.title
      this.time = alarmData.time
      this.qrCode = alarmData.qrCode
    },
    UpdateBaru(alarmDatas, alarmIndex) {
      console.log(alarmDatas)
      console.log('this alarm datas', this.alarmDatas)
      console.log('this alarm index', alarmIndex)
      console.log('this title', this.title)
      this.alarmDatas[alarmIndex].title = this.title
      this.alarmDatas[alarmIndex].time = this.time
      this.alarmDatas[alarmIndex].qrCode = this.qrCode
      alert('Berhasil Update')
      this.SaveStorage()

      this.title = ''
      this.time = '00:00'
      this.qrCode = ''
      this.alarmIndex = null
    }
  }
}
</script>
