<template>
  <canvas id="can" width="600" height="600"></canvas>
  <label for="count">Кол-во розеток</label>
  <input id="count" type="number" v-model="socketsNumber">
  <button @click="createElement">Создать элемент</button>
</template>

<script>
import { fabric } from 'fabric'
export default {
  name: 'App',
  data: () => ({
    canvas: null,
    socketsNumber: 1
  }),
  methods: {
    createElement() {
        var deleteImg = document.createElement('img')
        deleteImg.src = "/icons/delete.svg"

        const rect = new fabric.Rect({
        fill: 'white',
        stroke: 'back',
        strokeWidth: 1,
        width: 60,
        height: 60,
        originX: 'center',
        originY: 'center',
        subTargetCheck: true
      })
      var group = new fabric.Group([ rect, ...this.createSockets() ], {
        left: 150,
        top: 100,
        hasControls: false,
      })
      this.canvas.add(group)
      canvas.setActiveObject(group)
      fabric.Object.prototype.controls.deleteControl = new fabric.Control({
        x: 0.5,
        y: -0.5,
        offsetY: -16,
        offsetX: 16,
        cursorStyle: 'pointer',
        mouseUpHandler: deleteObject,
        render: renderIcon(deleteImg),
        cornerSize: 24
      })
      Add()
    },
    renderIcon(icon) {
      return function renderIcon(ctx, left, top, styleOverride, fabricObject) {
        var size = this.cornerSize;
        ctx.save();
        ctx.translate(left, top);
        ctx.rotate(fabric.util.degreesToRadians(fabricObject.angle));
        ctx.drawImage(icon, -size/2, -size/2, size, size);
        ctx.restore();
      }
    },
    createSockets() {
      var sockets = []
      for (let i = 0; i < this.socketsNumber; i++) {
        sockets.push(
          new fabric.Circle({
          radius: 5,
          fill: 'black',
          left: i < 2 ? i < 1 ? 30 : -30 : 0,
          top: i < 2 ? 0 : i === 3 ? 30 : -30,
          originX: 'center',
          originY: 'center',
          })
        )
      }
      return sockets
    },
    createLine() {
      const line = new fabric.Line([50, 10, 200, 150], {
      stroke: 'black'
    })
    this.canvas.add(line)
    },
    deleteObject(eventData, transform) {
      var target = transform.target;
      var canvas = target.canvas;
      canvas.remove(target);
      canvas.requestRenderAll();
    }
  },
  mounted() {
    this.canvas = new fabric.Canvas("can")
    this.canvas.getContext('2d')
  }
}
</script>

<style>

</style>
