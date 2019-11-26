<template>
  <div class="index-container">
    <br>
    <br>
    <canvas ref="canvas1"></canvas>
    <br>
  </div>
</template>

<script>
export default {
  name: 'Index',
  data() {
    return {
      isAuth: false,
      buttonImages: {
        'bgImage': {
          src: "static/img/bg.png",
          pos: {x:0, y:0}
        },
        'image500Btn': {
          src: "static/img/btn_500@3x.png",
          pos: {x:50, y:120}
        },
        'image100Btn': {
          src: "static/img/btn_100@3x.png",
          pos: {x:225, y:120}
        },
        'image50Btn': {
          src: "static/img/btn_50@3x.png",
          pos: {x:400, y:120}
        },
        'window': {
          src: "static/img/window@3x.png",
          pos: {x:50, y:300}
        },
        'iconSuccess': {
          src: "static/img/check@3x.png",
          pos: {x:70, y:325}
        },
        'finger': {
          src: "static/img/finger@3x.png",
          pos: {x:260, y:325}
        },
      }
    }
  },
  methods: {
    init() {
      for (let key in this.buttonImages) {
        this.buttonImages[key].hasImage = false
        this.buttonImages[key].image = new Image()
        this.buttonImages[key].image.src = this.buttonImages[key].src
      }
    },
    onloadBtnImages(f) {
      const max = Object.keys(this.buttonImages).length
      let count = 0
      for (let key in this.buttonImages) {
        this.buttonImages[key].image.onload = () =>  {
          count++
          if (max === count) {
            f()
          }
        }
      }
    },
    drawBaseImages() {
      const canvas = this.$refs['canvas1']
      const ctx = canvas.getContext("2d")
      // bg
      const images = this.buttonImages
      const imageBg = images['bgImage'].image
      const pos = this.buttonImages['bgImage'].pos
      canvas.width = imageBg.width
      canvas.height = imageBg.height
      ctx.drawImage(imageBg, pos.x, pos.y, imageBg.width, imageBg.height)
      // other
      ctx.drawImage(
        images['image500Btn'].image,
        images['image500Btn'].pos.x,
        images['image500Btn'].pos.y
      )
      ctx.drawImage(
        images['image100Btn'].image,
        images['image100Btn'].pos.x,
        images['image100Btn'].pos.y
      )
      ctx.drawImage(
        images['image50Btn'].image,
        images['image50Btn'].pos.x,
        images['image50Btn'].pos.y
      )
      ctx.drawImage(
        images['window'].image,
        images['window'].pos.x,
        images['window'].pos.y
      )
    },
    auth() {
      const canvas = this.$refs['canvas1']
      const ctx = canvas.getContext("2d")
      const image = this.buttonImages['finger']
      ctx.drawImage(image.image, image.pos.x, image.pos.y)
    },
    initBtnEvents() {
      const canvas = this.$refs['canvas1']
      const ctx = canvas.getContext("2d")
      const triggerEvent = (x, y, cx, cy, r, f) => {
        const d = Math.sqrt((cx - x) ** 2 + (cy - y) ** 2)
        if (d < r) {
          f()
        }
      }
      const drawSuccess = (msg) => {
        this.drawBaseImages()
        const image = this.buttonImages['iconSuccess']
        ctx.fillStyle = "#fff"
        ctx.font = "30px 'ＭＳ ゴシック'"
        ctx.textAlign = "left";
        ctx.textBaseline = "top";
        ctx.fillText(msg , 140, 340)
        ctx.drawImage(image.image, image.pos.x, image.pos.y)
      }
      const authSuccess = (msg) => {
        this.drawBaseImages()
        ctx.fillStyle = "#fff"
        ctx.font = "30px 'ＭＳ ゴシック'"
        ctx.textAlign = "left";
        ctx.textBaseline = "top";
        ctx.fillText(msg , 140, 340)
      }
      const btnEvents = (x, y) => {
        const r = 75
        const images = this.buttonImages
        if (this.isAuth) {
          triggerEvent(x, y, images['image500Btn'].pos.x + r, images['image500Btn'].pos.y + r, r, () => {
            alert(JSON.stringify({'data':'500'}))
            drawSuccess("500円送金しました")
            setTimeout(this.drawBaseImages, 2000)
          })
          triggerEvent(x, y, images['image100Btn'].pos.x + r, images['image100Btn'].pos.y + r, r, () => {
            alert(JSON.stringify({'data':'100'}))
            this.drawBaseImages()
            drawSuccess("100円送金しました")
            setTimeout(this.drawBaseImages, 2000)
          })
          triggerEvent(x, y, images['image50Btn'].pos.x + r, images['image50Btn'].pos.y + r, r, () => {
            alert(JSON.stringify({'data':'50'}))
            this.drawBaseImages()
            drawSuccess("50円送金しました")
            setTimeout(this.drawBaseImages, 2000)
          })
        } else {
          triggerEvent(x, y, images['finger'].pos.x + r, images['finger'].pos.y + r, r, () => {
            this.drawBaseImages()
            this.isAuth = true
            authSuccess("認証成功")
            setTimeout(this.drawBaseImages, 700)
          })
        }
      }
      canvas.addEventListener("click", (e) => {
        const rect = e.target.getBoundingClientRect()
        const x = e.clientX - rect.left
        const y = e.clientY - rect.top
        btnEvents(x, y)
      })
    }
  },
  mounted() {
    this.init()
    this.onloadBtnImages(() => {
      this.drawBaseImages()
      this.auth()
    })
    this.initBtnEvents()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.index-container {
  width: 100%;
  background-color: #2f2f2f;
}

#canvas1 {
  width:600px;
  height: 600px;
  background-color: #2f2f2f;
}


</style>
