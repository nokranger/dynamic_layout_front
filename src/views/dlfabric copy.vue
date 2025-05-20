<template>
  <div>
    <input type="text" id="textInput" placeholder="Enter new text" />
    <button id="addText">Add Text</button>
    <button id="editText">Edit Text</button>
    <button id="getJson">Get Canvas JSON</button>
    <canvas id="myCanvas" ref="canvas" width="800" height="600"></canvas>
    <div>
      {{ canva }}
    </div>
  </div>
</template>

<script>
import * as fabric from 'fabric';

export default {
  data() {
    return {
      canva: '',
      textTojson: ''
    }
  },
  mounted() {
    const canvas = new fabric.Canvas(this.$refs.canvas);

    // Function to generate unique ID for each object
    function generateUniqueId() {
      return 'id_' + Math.random().toString(36).substr(2, 9);
    }

    // Add text function
    function Add() {
      const value = document.getElementById('textInput').value || 'Default Text';

      // Generate unique ID for each text
      const uniqueId = generateUniqueId();

      const text = new fabric.Text(value, {
        left: 100,
        top: 100,
        fill: 'black',
        fontSize: 24,
        id: uniqueId,  // Attach unique ID to the object
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
      // Add a custom delete control (e.g., trash icon) to the text object
      text.controls.deleteControl = new fabric.Control({
        x: 0.5,
        y: -0.5,
        offsetY: -16,
        cursorStyle: 'pointer',
        mouseUpHandler: deleteObject,
        render: renderIcon,
        cornerSize: 24,
      });
      
      // const rect = new fabric.Rect({
      //   left: 100,
      //   top: 100,
      //   fill: 'yellow',
      //   width: 200,
      //   height: 100,
      //   objectCaching: false,
      //   stroke: 'lightgreen',
      //   strokeWidth: 4,
      // });

      // // Group the rectangle and the text together
      // const group = new fabric.Group([rect, text], {
      //   left: 100,
      //   top: 100,
      //   selectable: true,      // Allow selection of the whole group
      // });

      canvas.add(text);
      canvas.setActiveObject(text);
      canvas.requestRenderAll();

      // Log the unique ID for debugging purposes
      console.log('New object created with ID:', uniqueId);
      console.log('New object created with ID:', text);
      // Add event listener for the new "Get Canvas JSON" button
    }
    // Delete control handler
    function deleteObject(eventData, transform) {
      const canvas = transform.target.canvas;
      canvas.remove(transform.target);
      canvas.requestRenderAll();
    }

    // Custom control icon renderer
    const deleteImg = new Image();
    deleteImg.src = 'https://img.icons8.com/ios-glyphs/30/trash--v1.png'; // example icon

    function renderIcon(ctx, left, top, styleOverride, fabricObject) {
      const size = this.cornerSize;
      ctx.save();
      ctx.translate(left, top);
      ctx.rotate(fabric.util.degreesToRadians(fabricObject.angle));
      ctx.drawImage(deleteImg, -size / 2, -size / 2, size, size);
      ctx.restore();
    }

    // Handle text editing
    document.getElementById('editText').onclick = () => {
      const activeObject = canvas.getActiveObject();
      if (activeObject && activeObject.type === 'text') {
        const newText = document.getElementById('textInput').value;
        activeObject.set('text', newText); // Update the text value
        canvas.renderAll(); // Re-render canvas to show the updated text
        console.log(`Updated text for object with ID: ${activeObject.id}`);
      }
    };

    // // Track resizing (scaling)
    // canvas.on('object:scaling', function (e) {
    //   const obj = e.target;
    //   console.log('Object resized');
    //   console.log(`New width: ${obj.width}, New height: ${obj.height}`);

    //   // Optionally, log canvas JSON after resizing
    //   const json = canvas.toJSON();
    //   console.log('Canvas JSON after scaling:', JSON.stringify(json, null, 2));
    // });

    // // Track modifications (including flips)
    // canvas.on('object:modified', function (e) {
    //   const obj = e.target;

    //   // Check for flip (based on angle)
    //   if (obj.angle === 0 || obj.angle === 180) {
    //     console.log('Object flipped');
    //     console.log(`Object angle: ${obj.angle}`);
    //   }

    //   // Log updated position, size, etc.
    //   console.log(`Updated position: left=${obj.left}, top=${obj.top}`);
    //   console.log(`Updated size: width=${obj.width}, height=${obj.height}`);

    //   // Optionally, log canvas JSON after any modification
    //   const json = canvas.toJSON();
    //   console.log('Canvas JSON after modification:', JSON.stringify(json, null, 2));
    // });

    // Add event listeners for scaling and modifications
    canvas.on('object:scaling', function (e) {
      const obj = e.target;
      console.log('Object resized');
      console.log(`New width: ${obj.width}, New height: ${obj.height}`);
    });

    canvas.on('object:modified', function (e) {
      const obj = e.target;

      // Check for flip (based on angle)
      if (obj.angle === 0 || obj.angle === 180) {
        console.log('Object flipped');
        console.log(`Object angle: ${obj.angle}`);
      }

      console.log(`Updated position: left=${obj.left}, top=${obj.top}`);
      console.log(`Updated size: width=${obj.width}, height=${obj.height}`);
    });

    // Add event listener for the new "Get Canvas JSON" button
    document.getElementById('getJson').onclick = () => {
      const json = canvas.toJSON();
      console.log('Canvas JSON:', JSON.stringify(json, null, 2));  // Pretty print JSON
      this.canva = JSON.stringify(json, null, 2)
      // Optionally, save JSON to a file or send it to the server
      // saveJsonToFile(json);
    };

    // Hook up add text button
    document.getElementById('addText').onclick = () => Add();
  },
  methods: {
    check() {
      let divvv = document.getElementById('mydiv')
      console.log('divv', divvv)
    }
  }
};
</script>

<style>
canvas {
  border: 1px solid #ccc;
}
</style>