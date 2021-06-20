<template>
  <div style="height: 100vh;width: 100vw; display: flex;justify-content: center;align-items: center; "
       class="bg-grey-1">
    <q-card style="width: 900px;height: 600px;display: flex">
      <div style="height: 100%;width: 200px;border-right: #1976D2 1px solid">
        <div
          style="height: 64px ; border-bottom: #31CCEC 1px solid; padding: 2px;display: flex;justify-content: center;align-items: center"
          v-for="(r,i) in rooms" :key="i">

          <q-btn @click="room=r" flat style="color: #31CCEC" :label="r"/>
        </div>

      </div>
      <div class="q-pa-md" style="max-width: 400px">

        <q-form
          @submit="onSubmit"
          @reset="onReset"
          class="q-gutter-md"
        >
          <q-input
            filled
            v-model="name"
            label="Your name *"
            hint="Name and surname"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
          />



          <div style="display: flex;align-items: center">
            <div style="font-size: 20px;color: gray">selected room:</div>
            <div style="font-size: 24px;color: #1976D2">{{room}}</div>

          </div>

          <div>
            <q-btn label="Submit" type="submit" color="primary"/>
            <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
          </div>
        </q-form>

      </div>

    </q-card>
  </div>
</template>

<script>
export default {
  name: "RoomPage",
  data() {
    return {
      rooms: [],
      name:'',
      room:''
    }
  },
  mounted() {
    this.$axios.get('http://localhost:3003/rooms').then(resp => {
      this.rooms = resp.data.rooms
    })
  },
  methods:{
    onSubmit(){
      console.log(1)
      if (this.room===''  || this.name===''){
        return
      }

      const p=`${this.room}/${this.name}`
      console.log(p)
      this.$router.push(p)
    },
    onReset(){
      this.room=''
      this.name=''
    }
  }
}
</script>

<style scoped>

</style>
