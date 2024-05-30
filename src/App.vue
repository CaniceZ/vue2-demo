<script>
  import  { fabric } from 'fabric'
  const Image2 = new URL('./assets/xxxxx.png', import.meta.url).href
import QRCode from 'qrcode';
 

  export default {
    data (){
      return {
        canvas: null,
        url: '',
        productTitle: null,
        productDescription: null,
      }
    },
    methods: {
      async toImage(){
        this.productTitle.set({
          text: '自定义标题'
        })
        this.productDescription.set({
          text: '自定义描述'
        })
        // 设置二维码
        const opts = { errorCorrectionLevel: 'H'};
        const qrUrl = await QRCode.toDataURL('http://www.baidu.com', opts);
        console.log(qrUrl)
        fabric.Image.fromURL(qrUrl, (product)=> {
          product.scaleToWidth(145);
          product.scaleToHeight(145);
          product.left = 85;
          product.top = 185;
          product.selectable =false
          this.canvas.add(product);
          this.canvas.renderAll()
          this.url = this.canvas.toDataURL();
        });
      }
    },
    async mounted(){
      this.canvas = new fabric.Canvas('my-canvas', {
        width: 300,
        height: 518,
        selection: false
      })
      fabric.Image.fromURL(Image2, (bg)=>{
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
      // 设置二维码图片
      // const opts = { errorCorrectionLevel: 'H'};
      // const qrUrl = await QRCode.toDataURL('https://developers.weixin.qq.com/miniprogram/dev/framework/', opts);
      // console.log(qrUrl)
      // fabric.Image.fromURL(qrUrl, (product)=> {
      //   product.scaleToWidth(145);
      //   product.scaleToHeight(145);
      //   product.left = 85;
      //   product.top = 185;
      //   product.selectable =false
      //   this.canvas.add(product);
      // });
      this.productTitle = new fabric.Text('高质量运动T恤', {
        fontSize: 30,
        fontFamily: 'Arial',
        fill: '#fff',
        left: 40,
        top: 50,
        zIndex: 999,
        selectable: false
      });
      this.canvas.add(this.productTitle);
      this.productDescription = new fabric.Text('采用优质面料,舒适透气,耐穿耐洗。', {
        fontSize: 14,
        fontFamily: 'Arial',
        fill: 'red',
        left: 40,
        top: 100,
        zIndex: 999,
        selectable: false
      });
      this.canvas.add(this.productDescription);
      this.canvas.bringToFront(this.productTitle);
      this.canvas.bringToFront(this.productDescription);
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
