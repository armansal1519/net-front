<template>
  <label class="text-reader">
    <input type="file" name="file" id="file" class="inputfile" @change="loadTextFromFile"/>
    <!--    <q-file outlined v-model="model" label="Outlined" />-->
    <label for="file">Choose a file</label>
  </label>
</template>

<script>
export default {
  user_name: "FileReader",
  data() {
    return {
      model: ''
    }
  },
  methods: {
    loadTextFromFile(ev) {


      const file = ev.target.files[0];
      // console.log(fm,file)
      this.$emit('name', file.name)
      const reader = new FileReader();

      reader.onload = e => this.$emit("load", e.target.result);
      reader.readAsArrayBuffer(file);
    }
  }
};
</script>

<style scoped>


</style>
