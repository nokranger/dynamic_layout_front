<template>
  <div>
    <b-container>
      <input type="text" id="textInput" placeholder="Enter Name Station" />
      <input type="text" id="textInput2" placeholder="Enter Model" />
      <input type="text" id="textInput3" placeholder="Enter Sub Message No" />
      <input type="text" id="textInput4" placeholder="Enter Conversion Character" />
      <input type="text" id="textInput5" placeholder="Priority" />
      <b-container>
        <b-modal ref="my-modal" hide-footer title="Using Component Methods" size="sm">
          <div class="d-block text-center">
            <h3>{{ dataModal }}</h3>
          </div>
        </b-modal>
      </b-container>
      <button id="addText">Add Text</button>
      <button id="editText">Edit Text</button>
      <button id="getJson">Get Canvas JSON</button>
      <button id="loadJson">Load Canvas from loadjson</button>
      <canvas id="myCanvas" ref="canvas" width="800" height="600"></canvas>
      <div>
        {{ canva }}
      </div>
      <button v-on:click="check()">GetAAAA</button>
    </b-container>

  </div>
</template>

<script>
import * as fabric from 'fabric';

export default {
  data() {
    return {
      canva: '',
      textTojson: '',
      loadjson: [
      ],
      showModal: false,
      modalText: '',
      activeObject: null, // store the Fabric object being edited
      showInfoModal: false,
      selectedObject: null,
      dataModal: ''
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
      const value2 = document.getElementById('textInput2').value || 'Default Text';
      const value3 = document.getElementById('textInput3').value || 'Default Text';
      const value4 = document.getElementById('textInput4').value || 'Default Text';
      const value5 = document.getElementById('textInput5').value || 'Default Text';

      // Generate unique ID for each text
      const uniqueId = generateUniqueId();

      const text = new fabric.Text(value + '\n' + value2 + '\n' + value3 + '\n' + value4 + '\n' + value5, {
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

      text.controls.infoIcon = new fabric.Control({
        x: -0.5, // Top-left corner
        y: -0.5,
        offsetY: -10,
        offsetX: -10,
        cornerSize: 24,
        cursorStyle: 'pointer',
        render: function (ctx, left, top, styleOverride, fabricObject) {
          const size = this.cornerSize;
          ctx.save();
          ctx.translate(left, top);
          ctx.rotate(fabric.util.degreesToRadians(fabricObject.angle));
          ctx.drawImage(iconImg, -size / 2, -size / 2, size, size);
          ctx.restore();
        },
        mouseUpHandler: (eventData, transform) => {
          const obj = transform.target;

          // Access the Vue context via closure or global handler
          window.__vueShowInfoModal?.(obj); // <- this will trigger modal
          return true;
        }
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

    const iconImg = new Image();
    iconImg.src = 'https://img.icons8.com/ios-glyphs/30/info.png'; // any icon

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
      // console.log('Canvas JSON:', JSON.stringify(json, null, 2));  // Pretty print JSON
      this.canva = JSON.stringify(json, null, 2)
      // Optionally, save JSON to a file or send it to the server
      // saveJsonToFile(json);
      // Convert each object to simplified info (id, text, left, top, fontSize)
      const objectsData = canvas.getObjects().map(obj => obj.toObject());
      this.loadjson = objectsData;
      console.log('loadjson:=========', this.loadjson);
    };

    // Hook up add text button
    document.getElementById('addText').onclick = () => Add();

    function loadCanvasFromArray(dataArray) {
      canvas.clear();

      dataArray.forEach((item) => {
        const type = item.type?.toLowerCase();
        const id = item.id || generateUniqueId();

        if (type === 'text') {
          const text = new fabric.Text(item.text, {
            ...item,
            id,
            type: 'text', // ensure proper type
            hasControls: true,
            cornerStyle: 'circle',
            cornerStrokeColor: 'blue',
            cornerColor: 'lightblue',
            padding: 10,
            transparentCorners: false,
            borderColor: 'orange',
            borderDashArray: [3, 1, 3],
            borderScaleFactor: 2,
          });

          // ✅ Re-attach the custom delete control
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
        }
      });

      canvas.renderAll();
    }

    const textJsonArray = [
      {
        "fontSize": 24,
        "fontWeight": "normal",
        "fontFamily": "Times New Roman",
        "fontStyle": "normal",
        "lineHeight": 1.16,
        "text": "AB",
        "charSpacing": 0,
        "textAlign": "left",
        "styles": [],
        "pathStartOffset": 0,
        "pathSide": "left",
        "pathAlign": "baseline",
        "underline": false,
        "overline": false,
        "linethrough": false,
        "textBackgroundColor": "",
        "direction": "ltr",
        "type": "Text",  // <-- Fabric expects "text" lowercase!
        "left": 100,
        "top": 100,
        "width": 33.33,
        "height": 27.12,
        "fill": "black",
        "stroke": null,
        "strokeWidth": 1,
        "scaleX": 1,
        "scaleY": 1,
        "angle": 0,
        "opacity": 1,
        "visible": true,
        "backgroundColor": ""
      }
    ];
    loadCanvasFromArray(this.loadjson); // ← use your extracted data


    document.getElementById('loadJson').onclick = () => {
      loadCanvasFromArray(this.loadjson);
    };
    window.__vueShowInfoModal = (obj) => {
      this.openInfoModal(obj); // your Vue method
    };
  },
  methods: {
    check() {
      console.log('divv', this.textTojson)
    },
    openInfoModal(obj) {
      console.log('obj===', obj)
      this.$refs['my-modal'].show()
      this.dataModal = obj.id + obj.text
    },
    closeInfoModal() {

    }
  }
};
</script>

<style>
canvas {
  border: 1px solid #ccc;
}

.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}
</style>
