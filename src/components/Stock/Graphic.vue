<template>
 <div class="Graphic">
     <canvas ref="canvasGraph">
         Your browser does not support the HTML5 canvas tag.
         </canvas>
 </div>
</template>

<script>

export default {
  name: 'Graphic',
  props: {
    negative: Boolean,
    high: Number,
    low: Number,
    currPrice: Number
  },
  mounted () {
    this.buildGradient()
  },
  methods: {
    buildGradient () {
      console.log(this.$refs)
      const canvasObj = this.$refs.canvasGraph
      canvasObj.width = canvasObj.height *
    (canvasObj.clientWidth / canvasObj.clientHeight)
      console.log(canvasObj)
      const context = canvasObj.getContext('2d')
      const gradient = context.createLinearGradient(0, 100, 0, 0)
      let startColor = 'green'
      let endColor = 'yellow'
      if (this.negative) {
        startColor = 'red'
        endColor = 'orange'
      }
      gradient.addColorStop(0, startColor)
      gradient.addColorStop(1, endColor)

      context.fillStyle = gradient
      context.fillRect(0, 0, canvasObj.width, canvasObj.height)

      const context2 = canvasObj.getContext('2d')
      context2.beginPath()
      context2.moveTo(canvasObj.width / 3, canvasObj.height / 6)
      context2.lineTo(canvasObj.width / 3, (5 * canvasObj.height) / 6)
      context2.lineWidth = 3
      context2.strokeStyle = 'white'
      context2.stroke()

      const letterHeight = 12

      const context3 = canvasObj.getContext('2d')
      context3.font = `${letterHeight}px Sans`
      context3.fillStyle = 'white'
      context3.fillText(String(this.high), (canvasObj.width / 3) + letterHeight, (canvasObj.height / 6) + letterHeight)

      const context4 = canvasObj.getContext('2d')
      context4.font = `${letterHeight}px Sans`
      context4.fillStyle = 'white'
      context4.fillText(String(this.low), (canvasObj.width / 3) + letterHeight, (5 * canvasObj.height / 6))

      const context5 = canvasObj.getContext('2d')
      const triangleSize = 1
      const widthTriangleStart = (canvasObj.width / 4) - triangleSize
      const widthTriangleEnd = (canvasObj.width / 4)

      const topLine = (canvasObj.height / 6)
      const bottomLine = (5 * canvasObj.height / 6)
      const currPostion = (bottomLine - topLine)
      console.log('currposition', ((this.currPrice - this.low) / (this.high - this.low)) * currPostion)
      const placeOnLine = bottomLine - ((this.currPrice - this.low) / (this.high - this.low)) * currPostion
      console.log(placeOnLine)
      const heightTriangleStart = placeOnLine + triangleSize
      const heightTriangleEnd = placeOnLine - triangleSize

      // the triangle
      context5.beginPath()
      context5.moveTo(widthTriangleStart, heightTriangleStart)
      context5.lineTo(widthTriangleEnd, placeOnLine)
      context5.lineTo(widthTriangleStart, heightTriangleEnd)
      context5.closePath()

      // the outline
      context5.lineWidth = 10
      context5.strokeStyle = 'white'
      context5.stroke()

      // the fill color
      context5.fillStyle = 'white'
      context5.fill()
    }
  }
}
</script>

<style lang="scss">
.Graphic {
    canvas {
        width:100%;
        height: $desktopHeight;
        fill:red;
    }
}
</style>
