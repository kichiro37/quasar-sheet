<template>
  <q-layout view="lHh Lpr lFf"> 
    <q-header elevated class="bg-red-7">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          Bossun App
        </q-toolbar-title>

        <div>bloody</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1 text-weight-bold"
    >
      <q-list>
        <q-item-label
          header
          class="text-red-7 text-h5 text-weight-bold bg-black"
        >
          AplikasiKu <span class="text-caption"> {{version}}</span>
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'
import {version} from '../../package.json'

const linksData = [
  {
    title: 'Home',
    caption: '',
    icon: 'home',
    link: '/'
  },
  {
    title: 'Alarm',
    caption: '',
    icon: 'alarm',
    link: '/alarm'
  },
  {
    title: 'Support Us',
    caption: '',
    icon: 'volunteer_activism',
    link: '/donate'
  },
  {
    title: 'Settings',
    caption: '',
    icon: 'settings',
    link: '/settings'
  },
  {
    title: 'Facebook',
    caption: 'Aditya Andre',
    icon: 'public',
    link: 'https://web.facebook.com/Kichiro37/'
  },
];

export default {
  name: 'MainLayout',
  components: { EssentialLink },
  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData,
      version
    }
  }
}
</script>
