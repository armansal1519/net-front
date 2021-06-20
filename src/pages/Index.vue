<template>
  <div style="height: 80vh">
    <div class="q-pa-md row justify-center">
      <div style="width: 100%">

        <div v-if="connection==''" class="q-pa-md" style="max-width: 400px">

          <q-form
            @submit="onConnect"
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

            <q-input
              filled
              v-model="room"
              label="room name"
              lazy-rules
              :rules="[
          val => val !== null && val !== '' || 'Please type your age',

        ]"
            />


            <div>
              <q-btn label="Submit" type="submit" color="primary"/>
            </div>
          </q-form>

        </div>


        <q-chat-message
          v-for="(c,i) in chats"
          :key="i"
          :name="c.name"
          :sent="c.send"
          :style="`color: ${c.color}` "
          :bg-color="c.send?'blue-2 ':'light-blue-3 '"

        >
          <div v-if="!c.send" style="display: flex;">
            <div>
              <div>{{ c.text }}</div>
              <div>{{ c.file_name }}</div>
            </div>
            <q-avatar @click="downloadWithAxios(c.link,c.file_name)" v-if="c.link!==''" style="margin-left: 32px"
                      rounded :style="`background-color: ${c.file_color}`" text-color="white"
                      icon="fas fa-file-download"/>
          </div>
          <div v-else style="display: flex;padding: 6px">
            <q-avatar @click="downloadWithAxios(c.link,c.file_name)" v-if="c.link!==''" style="margin-right: 32px"
                      rounded :style="`background-color: ${c.file_color}`" text-color="white"
                      icon="fas fa-file-download"/>
            <div>
              <div>{{ c.text }}</div>
              <div>{{ c.file_name }}</div>
            </div>
          </div>
        </q-chat-message>
      </div>
    </div>
  </div>
  <div>
    <div style="width: 100%;display: flex;justify-content: space-around;padding: 12px 32px">
      <q-input outlined style="width: 100%" v-model="text" label="massage"/>
      <FileReader @load="byteFile = $event" @name="name=$event"/>

      <q-btn @click="onFile" outline color="primary" style="width: 244px" label="send"/>
      <!--      <q-btn @click="onTest" outline style="color: goldenrod;" label="test" />-->

    </div>
  </div>
</template>

<script>
import {defineComponent} from 'vue';
import FileReader from "components/FileReader";
import {api} from "boot/axios";


export default defineComponent({
  user_name: 'PageIndex',
  components: {FileReader},
  data() {
    return {
      name:'',
      room:'',
      connection:'',
      text: '',
      model: null,
      byteFile: '',
      chats: [
        // {
        //   text: "arman salehi",
        //   link: 'asdasd',
        //   name: 'arman',
        //   send: true
        // },
        // {
        //   text: "arman salehi",
        //   link: '',
        //   name: 'arman',
        //   send: false
        // }
      ]
    }
  },
  mounted() {

  },
  methods: {
    onConnect() {
      console.log("Starting connection to WebSocket Server")
      this.connection = new WebSocket(`ws://localhost:3003/ws/${this.room}/${this.name}`)
      const store = this.$store
      const chats = this.chats

      this.connection.onmessage = function (event) {
        console.log(event)
        const resp = JSON.parse(event.data);
        console.log(resp)
        if (resp.action === 'addUser') {

          store.commit('websocket/addUser', resp)
        } else if (resp.action === 'newMsg') {
          const c = {
            text: resp.user_msg,
            send: false,
            link: resp.link,
            name: resp.name,
            color: resp.color,
            file_color: resp.file_color,
            file_name: resp.file_name
          }
          chats.push(c)
        }
      }

    },
    onFile() {
      const typedArray = new Uint8Array(this.byteFile);
      const array = [...typedArray];
      console.log(array)
      const c = {
        text: this.text,
        send: true,
        link: '',
        name: "me",
        color: "black"
      }
      this.chats.push(c)
      const a = JSON.stringify({
        name: this.name,
        arr: array,
        msg: this.text
      })
      this.connection.send(a)
    },
    // onDownload(path){
    //   const url='http://localhost:3003'+path
    //   console.log(3,url)
    //   // window.open(url, '_blank').focus();
    //   this.$axios.get(url, {
    //
    //     responseType: 'arraybuffer'
    //   }).then((response) => {
    //     console.log(response)
    //     var fileURL = window.URL.createObjectURL(new Blob([response.data]));
    //     var fileLink = document.createElement('a');
    //
    //     fileLink.href = fileURL;
    //     fileLink.setAttribute('download', 'img.png');
    //     document.body.appendChild(fileLink);
    //
    //     fileLink.click()
    //   })
    //
    // },
    forceFileDownload(response, title) {
      console.log(title)
      const url = window.URL.createObjectURL(new Blob([response.data]))
      const link = document.createElement('a')
      link.href = url
      link.setAttribute('download', title)
      document.body.appendChild(link)
      link.click()
    },
    downloadWithAxios(path, title) {
      const url = 'http://localhost:3003' + path
      window.open(url, '_blank').focus();

      //   this.$axios({
      //     method: 'get',
      //     url,
      //     responseType: 'arraybuffer',
      //   })
      //     .then((response) => {
      //       this.forceFileDownload(response, title)
      //     })
      //     .catch(() => console.log('error occured'))
    },
  }

})
</script>
