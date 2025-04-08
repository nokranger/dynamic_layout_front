<template>
  <div>
    <button id="add">Add Text</button>
    <canvas id="myCanvas" ref="canvas" width="1000" height="1000"></canvas>
  </div>
</template>

<script>
import * as fabric from 'fabric';

export default {
  mounted() {
    const canvas = new fabric.Canvas(this.$refs.canvas);

    const deleteImg = new Image();
    deleteImg.src = 'https://img.icons8.com/ios-glyphs/30/trash--v1.png'; // example icon

    // Add text function
    function Add() {
      const text = new fabric.Text('Dynamic TMT', {
        left: 100,
        top: 100,
        fill: 'black',
        fontSize: 24,
        cornerStyle: 'circle',
        cornerStrokeColor: 'blue',
        cornerColor: 'lightblue',
        padding: 10,
        transparentCorners: false,
        borderColor: 'orange',
        borderDashArray: [3, 1, 3],
        borderScaleFactor: 2,
        hasControls: true,
      });

      text.controls.deleteControl = new fabric.Control({
        x: 0.5,
        y: -0.5,
        offsetY: -16,
        cursorStyle: 'pointer',
        mouseUpHandler: deleteObject,
        render: renderIcon,
        cornerSize: 24,
      });

      canvas.add(text);
      canvas.setActiveObject(text);
      canvas.requestRenderAll();
    }

    // Delete control handler
    function deleteObject(eventData, transform) {
      const canvas = transform.target.canvas;
      canvas.remove(transform.target);
      canvas.requestRenderAll();
    }

    // Custom control icon renderer
    function renderIcon(ctx, left, top, styleOverride, fabricObject) {
      const size = this.cornerSize;
      ctx.save();
      ctx.translate(left, top);
      ctx.rotate(fabric.util.degreesToRadians(fabricObject.angle));
      ctx.drawImage(deleteImg, -size / 2, -size / 2, size, size);
      ctx.restore();
    }

    // Hook up the button
    document.getElementById('add').onclick = () => Add();

    // Initial text
    Add();
  }
};
</script>

<style>
canvas {
  border: 1px solid #ccc;
}
</style>