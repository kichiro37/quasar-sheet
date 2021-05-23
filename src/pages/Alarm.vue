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
        <q-input class="col" label="output" v-model="qrCode" />
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

import {Quasar, QTime} from 'quasar'
import AlarmInf from 'components/AlarmInf'

const alarmDatas = [
  {
    id: '1',
    title: 'TANGI',
    caption: '06:00',
    icon: 'home',
    qrCode: 'abb'
  },
  {
    id: '2',
    title: 'MASAK',
    caption: '08:00',
    icon: 'alarm',
    qrCode: 'abc'
  },
  {
    id: '3',
    title: 'TIDUR',
    caption: '21:00',
    icon: 'volunteer_activism',
    qrCode: 'abd'
  },
]

export default {
  name: 'Alarm',
  components: {
  	AlarmInf,
  	QTime
  },
  data () {
  	return {
  		alarmDatas: alarmDatas,
  		title: null,
  		caption: null,
      qrCode: null
  	}
  },
  methods: {
  	CreateAlarm() {
  		const params = {
  			title: this.title,
  			caption: this.caption,
        qrCode: this.qrCode
  		}
  		alarmDatas.push(params)
  		this.title = ''
  		this.caption = ''
      this.qrCode = ''
  		alert('Tambah Data Sukses')
  	},
    OnDeleteAlarm(alarmIndex, title) {
      alert(`Berhasil Terhapus ${title}`)
      this.alarmDatas.splice(alarmIndex, 1)
    },
    OnUpdateAlarm(alarmData) {
      this.alarmDatas[alarmData.index].title = alarmData.title
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
