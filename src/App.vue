<template>
  <div id="app">
    <input type="file" accept="application/pdf" @input="onInput">
    <Pdfreader :base64="file"/>
    
  </div>
</template>

<script>
import Pdfreader from './components/Pdfreader.vue'
export default {
  name: 'app',
  components: {
    Pdfreader
  },
  data(){
    return {
      file:null
    }
  },
  methods:{
    onInput(evt){
      const reader = new FileReader()
      reader.addEventListener('load',()=>{
        this.file = reader.result.split(',')[1]
      })
      const file = evt.target.files[0]
      reader.readAsDataURL(file)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
