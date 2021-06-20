<template>
  <q-item
  >
    <q-avatar size="48px" font-size="24px" style="margin-right: 16px" :style="`background-color: ${color}`"
              text-color="white" icon="far fa-user"/>

    <q-item-section>
      <q-item-label>{{ user_name }}</q-item-label>
      <q-item-label caption>
        {{ uuid.slice(0, 20) }}...

      </q-item-label>
    </q-item-section>
  </q-item>
</template>

<script>
import {defineComponent} from 'vue'

export default defineComponent({
  user_name: 'EssentialLink',
  props: {
    user_name: {
      type: String,
      required: true
    },

    uuid: {
      type: String,
      default: ''
    },
    color: {
      type: String,
      default: '#ffffff'
    }

  },
  // computed:{
  //   getColor(){
  //     return '#'+(Math.random()*0xFFFFFF<<0).toString(16)
  //     // return '#545454'
  //   }
  // }
})
</script>
