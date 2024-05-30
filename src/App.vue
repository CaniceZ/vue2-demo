<script>
  import  { fabric } from 'fabric'
  const Image = new URL('./assets/xxxxx.png', import.meta.url).href
  const qrcode = new URL('./assets/logo.svg', import.meta.url).href
  export default {
    data (){
      return {
        canvas: null,
        url: ''
      }
    },
    methods: {
      toImage(){
        this.url = this.canvas.toDataURL();
      }
    },
    mounted(){
      this.canvas = new fabric.Canvas('my-canvas', {
        width: 300,
        height: 518,
        selection: false
      })
      fabric.Image.fromURL(Image, (bg)=>{
        bg.scaleToWidth(300);
        bg.scaleToHeight(518);
        // canvas.add(bg);
        
        this.canvas.setBackgroundImage(bg, 
          this.canvas.renderAll.bind(this.canvas),
          {
            crossOrigin: 'anonymous'
          }  
      );
        
      });
      fabric.Image.fromURL(qrcode, (product)=> {
        product.scaleToWidth(145);
        product.scaleToHeight(145);
        product.left = 60;
        product.top = 195;
        product.selectable =false
        this.canvas.add(product);
      });
      var productTitle = new fabric.Text('高质量运动T恤', {
        fontSize: 30,
        fontFamily: 'Arial',
        fill: '#fff',
        left: 40,
        top: 50,
        zIndex: 999,
        selectable: false
      });
      this.canvas.add(productTitle);
      var productDescription = new fabric.Text('采用优质面料,舒适透气,耐穿耐洗。', {
        fontSize: 14,
        fontFamily: 'Arial',
        fill: 'red',
        left: 40,
        top: 100,
        zIndex: 999,
        selectable: false
      });
      this.canvas.add(productDescription);
      this.canvas.bringToFront(productTitle);
      this.canvas.bringToFront(productDescription);
    }
  }

  
</script>

<template>
  <div id="app">
    <main>
      <canvas id="my-canvas"></canvas>
      <br>
      <br>
      <div><button @click="toImage"> 下一步</button></div>
      <br>
      <br>
      <img width="300" height="518" v-if="url" :src="url"></img>
      
    </main>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
