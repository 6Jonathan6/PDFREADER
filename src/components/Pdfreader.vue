<template>
<div class="root">
  <button @click.prevent="zoomUp">Agrandar</button>
  <button @click.prevent="zoomDown">pequena</button>
  <div class="canvas-container">
    <canvas ref="vCanvas" width="300"></canvas>
  </div>
</div>
</template>
<script>
import pdfjs from 'pdfjs-dist'
export default {
  name: 'Pdfreader',
  props: {
    base64: String,
    default:null,
    required: true
  },
  data(){
    return {
      scale:1
    }
  },
  watch:{
    base64(newValue){
      this.renderPreview(newValue)
    }
  },
  methods:{
    async renderPreview(fileBase64,page=1){
      const canvas = this.$refs.vCanvas
        const file = window.atob(fileBase64)
        const pdfFile = await pdfjs.getDocument({data:file}).promise
        console.log(pdfFile)
        const actualPage = await pdfFile.getPage(page)
        const scale = canvas.width / actualPage.getViewport(1).width
        const viewport = actualPage.getViewport(scale)
        const context = canvas.getContext('2d')
        canvas.height = viewport.height
        const renderContext ={
          canvasContext: context,
          viewport: viewport
        }
        actualPage.render(renderContext) 
    },
    zoomUp(){
      this.scale = this.scale + 0.5
      this.renderPdf(this.base64,1,this.scale)
    },
    zoomDown(){
      this.scale = this.scale - 0.2
      this.renderPdf(this.base64,1,this.scale)
    },
    async renderPdf(fileBase64,page=1,scale=1){
      try {
        const canvas = this.$refs.vCanvas
        const file = window.atob(fileBase64)
        const pdfFile = await pdfjs.getDocument({data:file}).promise
        console.log(pdfFile)
        const actualPage = await pdfFile.getPage(page)
        const viewport = actualPage.getViewport(scale)
        console.log(viewport)
        const context = canvas.getContext('2d')
        canvas.height = viewport.height
        canvas.width = viewport.width
        const renderContext ={
          canvasContext: context,
          viewport: viewport
        }
        actualPage.render(renderContext) 
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.canvas-container{
  width: 100%;
  overflow-x: scroll;
  border: 1px solid blue
  
}
.root {
  width: 100%;
  overflow: hidden;
  border: 1px solid black
}
</style>
