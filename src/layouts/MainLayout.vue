<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Quasar App
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
          v-if="getUsers.length<1"
        >
          you are alone in this room
        </q-item-label>

        <EssentialLink
          v-for="u in getUsers"
          :key="u.uuid"
          v-bind="u"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view/>
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'


import {defineComponent, ref} from 'vue'

export default defineComponent({
  user_name: 'MainLayout',

  components: {
    EssentialLink
  },
  computed: {
    getUsers() {

      return this.$store.state.websocket.users


    }
  },

  setup() {
    const leftDrawerOpen = ref(false)

    return {

      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>
