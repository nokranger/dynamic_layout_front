<template>
  <div>
    <b-container>
      <div align="left">
        <div style="font-weight: bold;">
          Name Station
        </div>
        <div>
          <b-form-select id="textInput" v-model="selecteds" :options="stationO"></b-form-select>
        </div>
        <br>
        <div style="font-weight: bold;">
          Model
        </div>
        <div>
          <b-form-select id="textInput2" v-model="selectedm" :options="model"
            v-on:change="selectsubmsgno()"></b-form-select>
        </div>
        <br>
        <div style="font-weight: bold;">
          Sub message No
        </div>
        <div>
          <b-form-select id="textInput3" v-model="selectedmsg" :options="submsgno"
            v-on:change="selectconversion()"></b-form-select>
        </div>
        <br>
        <div style="font-weight: bold;">
          Conversion Character
        </div>
        <div>
          <b-form-select id="textInput4" v-model="selectedc" :options="conversion"></b-form-select>
        </div>
        <br>
        <div style="font-weight: bold;">
          Priority
        </div>
        <div>
          <b-input id="textInput5" style="width: 30%;" v-model="priority" placeholder="Priority"></b-input>
        </div>
      </div>
      <b-container>
        <b-modal ref="my-modal" id="bv-modal-example" size="xl" :title="'ID: ' + dataModal.id" hide-footer>
          <div style="font-weight: bold;">
            Name Station
          </div>
          <div>
            <b-form-select id="textInput" v-model="selecteds" :options="stationO"></b-form-select>
          </div>
          <br>
          <div style="font-weight: bold;">
            Model
          </div>
          <div>
            <b-form-select id="textInput2" v-model="selectedm" :options="model"
              v-on:change="selectsubmsgno()"></b-form-select>
          </div>
          <br>
          <div style="font-weight: bold;">
            Sub message No
          </div>
          <div>
            <b-form-select id="textInput3" v-model="selectedmsg" :options="submsgno"
              v-on:change="selectconversion()"></b-form-select>
          </div>
          <br>
          <div style="font-weight: bold;">
            Conversion Character
          </div>
          <div>
            <b-form-select id="textInput4" v-model="selectedc" :options="conversion"></b-form-select>
          </div>
          <br>
          <div style="font-weight: bold;">
            Priority
          </div>
          <div>
            <b-input id="textInput5" style="width: 30%;" v-model="priority" placeholder="Priority"></b-input>
          </div>
          <br>
          <div v-for="(databox, index) in showboxdetail2" :key="index">
            <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name
                }}</label></div>
            <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label>
            </div>
            <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg
                }}</label></div>
            <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{
              databox.conversion_char }}</label></div>
            <div style="font-weight: bold;">Priority: <label style="font-weight: normal;">{{
              databox.priority }}</label></div>
          </div>
          <br>
          <!-- <b-button variant="outline-primary"
            v-on:click="addBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
            style="margin: 10px;">Add</b-button> -->
          <b-button variant="outline-danger" @click="$bvModal.hide('bv-modal-example')"
            style="margin: 10px;">Close</b-button>
          <!-- <b-button variant="outline-warning" id="editText" style="margin: 10px;">Edit</b-button> -->
          <!-- <b-button variant="outline-danger" v-on:click="deleteActiveObject" style="margin: 10px;">Delete</b-button> -->
        </b-modal>
      </b-container>
      <br>
      <br>
      <div align="left">
        <b-button style="margin: 10px;" variant="outline-primary" id="addText">Add Text</b-button>
        <b-button style="margin: 10px;" variant="outline-secondary" id="editText">Edit Text</b-button>
        <b-button style="margin: 10px;" variant="outline-success" id="getJson">Save Layout</b-button>
        <b-button style="margin: 10px;" variant="outline-primary" id="loadJson">Load Layout</b-button>
      </div>
      <br>
      <br>
      <canvas id="myCanvas" ref="canvas" width="1200" height="800" style="outline: black 3px solid;"></canvas>
      <!-- <div>
        {{ canva }}
      </div> -->
      <!-- <button v-on:click="check()">GetAAAA</button> -->
      <!-- <div>
        {{ showobj }}
      </div> -->
    </b-container>

  </div>
</template>

<script>
import * as fabric from 'fabric';
import axios from 'axios';
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
      dataModal: '',
      alldatabiasbc: '',
      model: [],
      selectedm: null,
      submsgno: [],
      selectedmsg: null,
      conversion: [],
      selectedc: null,
      lmodel: '',
      lwork: '',
      llayout: '',
      llevel: 0,
      bc: '',
      // conversion: '',
      boxstation: [],
      location: '',
      selecteds: null,
      stationO: [],
      showboxdetail: '',
      sname_work: '',
      smodel: '',
      ssubmsg: '',
      sconversion_char: '',
      showboxdetail2: '',
      showbox: '',
      priority: '',
      showobj: ''
    }
  },
  mounted() {
    this.loadlocation()
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
      console.log('value========', value + value2 + value3 + value4 + value5)

      // Generate unique ID for each text
      const uniqueId = generateUniqueId();

      const text = new fabric.Text(value2 + '\n' + value3, {
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
        station_id: value,
        model: value2,
        submsg: value3,
        conversion_char: value4,
        priority: value5,

        backgroundColor: 'rgba(255, 255, 255, 0.8)', // สีพื้นหลังขาวใส
        rx: 10,                                       // มุมโค้ง (optional)
        ry: 10

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
      // const canvas = transform.target.canvas;
      const target = transform.target;
      const canvas = target.canvas;
      canvas.remove(target);
      canvas.requestRenderAll();
      // 🧨 If object has an ID, call backend to delete
      if (target.id) {
        axios.delete(`http://localhost:4000/boxstation2/${target.id}`)
          .then(res => {
            console.log(`✅ Deleted from backend: ID = ${target.id}`);
          })
          .catch(err => {
            console.error(`❌ Failed to delete ID ${target.id} from backend`, err);
          });
      }
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
      console.log('Object flipped===========');
      const activeObject = canvas.getActiveObject();
      if (activeObject && activeObject.type === 'text') {
        const newText = document.getElementById('textInput2').value + '\n' + document.getElementById('textInput3').value
        const newstation_id = document.getElementById('textInput').value
        const newmodel = document.getElementById('textInput2').value
        const newsubmsg = document.getElementById('textInput3').value
        const newconversion_char = document.getElementById('textInput4').value
        const newpriority = document.getElementById('textInput5').value
        activeObject.set('text', newText); // Update the text value
        activeObject.set('station_id', newstation_id); // Update the text value
        activeObject.set('model', newmodel); // Update the text value
        activeObject.set('submsg', newsubmsg); // Update the text value
        activeObject.set('conversion_char', newconversion_char); // Update the text value
        activeObject.set('priority', newpriority); // Update the text value
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

    fabric.Object.prototype.toObject = (function (toObject) {
      return function (...args) {
        return {
          ...toObject.call(this, ...args),
          station_id: this.station_id || '',
          model: this.model || '',
          submsg: this.submsg || '',
          conversion_char: this.conversion_char || '',
          priority: this.priority || ''
        };
      };
    })(fabric.Object.prototype.toObject);

    // Add event listener for the new "Get Canvas JSON" button
    document.getElementById('getJson').onclick = () => {
      fabric.Object.prototype.toObject = (function (toObject) {
        return function (...args) {
          return {
            ...toObject.call(this, ...args),
            id: this.id || ''
          };
        };
      })(fabric.Object.prototype.toObject);
      const json = canvas.toJSON();
      // console.log('Canvas JSON:', JSON.stringify(json, null, 2));  // Pretty print JSON
      this.canva = JSON.stringify(json, null, 2)
      // Optionally, save JSON to a file or send it to the server
      // saveJsonToFile(json);
      // Convert each object to simplified info (id, text, left, top, fontSize)
      const objectsData = canvas.getObjects().map(obj => obj.toObject());
      this.loadjson = objectsData;
      // console.log('loadjson:=========', this.loadjson);
      this.sendCanvasDataToAPI(this.loadjson)
    };

    // Hook up add text button
    document.getElementById('addText').onclick = () => Add(
    );

    function loadCanvasFromArray(dataArray) {
      canvas.clear();

      dataArray.forEach((item) => {
        const type = item.type?.toLowerCase();
        const id = item.id || generateUniqueId();
        // console.log('loadjson:=========', 'item.station_id' + 'item.model' + item.submsg);
        if (type === 'text') {
          console.log('loadjson:=========', 'item.station_id' + item.model);
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
            station_id: item.station_id,
            model: item.model,
            submsg: item.submsg,
            conversion_char: item.conversion_char,
            priority: item.priority,
            backgroundColor: 'rgba(255, 255, 255, 0.8)', // สีพื้นหลังขาวใส
            rx: 10,                                       // มุมโค้ง (optional)
            ry: 10
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
    // loadCanvasFromArray(this.loadjson); // ← use your extracted data


    document.getElementById('loadJson').onclick = () => {
      axios.get('http://localhost:4000/allboxstationCanva')
        .then((res) => {
          // alert('Insert success!');
          console.log('getdata success======', res.data.result);
          this.loadjson = res.data.result.map(item => ({
            ...item,
            // Convert numbers to booleans
            underline: !!item.underline,
            overline: !!item.overline,
            linethrough: !!item.linethrough,
            strokeUniform: !!item.strokeUniform,
            flipX: !!item.flipX,
            flipY: !!item.flipY,
            visible: !!item.visible,

            // Convert JSON strings back to real objects/arrays
            styles: JSON.parse(item.styles || '[]'),
            strokeDashArray: JSON.parse(item.strokeDashArray || '[]'),
            shadow: JSON.parse(item.shadow || '{}'),

            // Ensure number types for numeric strings
            fontSize: parseFloat(item.fontSize),
            strokeWidth: parseFloat(item.strokeWidth),
            scaleX: parseFloat(item.scaleX),
            scaleY: parseFloat(item.scaleY),
            angle: parseFloat(item.angle),
            skewX: parseFloat(item.skewX),
            skewY: parseFloat(item.skewY),
            left: parseFloat(item.left),
            top: parseFloat(item.top),
            width: parseFloat(item.width),
            height: parseFloat(item.height),
            opacity: parseFloat(item.opacity),
            backgroundColor: 'rgba(255, 255, 255, 0.8)', // สีพื้นหลังขาวใส
            rx: 10,                                       // มุมโค้ง (optional)
            ry: 10
          }));
          this.showobj = this.loadjson
          loadCanvasFromArray(this.loadjson);
        })
        .catch((err) => {
          alert('Insert failed!');
          console.error(err);
        });
      // console.log('getdata success======', this.loadjson);
      // this.showobj = this.loadjson
      // loadCanvasFromArray(this.loadjson);
      this.showdetail()
    };
    window.__vueShowInfoModal = (obj) => {
      this.openInfoModal(obj); // your Vue method
      this.showdetail()
    };
  },
  methods: {
    check() {
      console.log('divv', this.textTojson)
    },
    openInfoModal(obj) {
      console.log('obj===', obj)
      this.$refs['my-modal'].show()
      this.dataModal = obj
    },
    closeInfoModal() {

    },
    loadlocation() {
      axios.get('http://localhost:4000/allstation')
        .then(response => {
          this.stationO = response.data.result
          this.stationO = response.data.result.map((data, i) => {
            return {
              value: data.station_id,
              text: data.name
            }
          })
          this.stationO.push({ "value": null, "text": "Please select an option" })
          // console.log('detail=====', this.stationO)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
      axios.get('http://localhost:4000/alldiasbc2').then(response => {
        console.log(response.data);
        // this.options = response.data.result
        // this.model = response.data.result
        this.model = response.data.result.map((data, i) => {
          return {
            value: data.model,
            text: data.model
          }
        })
        this.model.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
      axios.get('http://localhost:4000/allboxstation3').then(response => {
        this.showboxdetail = response.data.result
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    selectsubmsgno() {
      axios.get('http://localhost:4000/allconversion').then(response => {
        // console.log(response.data);
        // this.options = response.data.result
        this.submsgno = response.data.result
        console.log('mmm====', this.selectedm)
        this.submsgno = this.submsgno.filter((i) => {
          console.log('ccc====', i.model)
          // console.log('ccc====', i.submsgno)
          return i.model == this.selectedm
        })
        // console.log('allmsghno', this.submsgno)
        this.submsgno = this.submsgno.map((data, i) => {
          return {
            value: data.submsgno,
            text: data.submsgno
          }
        })
        this.submsgno.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    selectconversion() {
      axios.get('http://localhost:4000/allconversion').then(response => {
        console.log(response.data);
        // this.options = response.data.result
        this.conversion = response.data.result
        this.conversion = this.conversion.filter((i) => {
          return i.submsgno == this.selectedmsg
        })
        this.conversion = this.conversion.map((data, i) => {
          return {
            value: data.conversioncharacter,
            text: data.conversioncharacter
          }
        })
        this.conversion.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    showdetail() {
      // this.showbox = {
      //   show_detail: id
      // }
      // axios.post('http://localhost:4000/allboxstation4', this.showbox).then(response => {
      //   console.log('showdetail=======', response.data.result)
      this.selecteds = this.dataModal.station_id
      this.selectedm = this.dataModal.model
      this.selectedmsg = this.dataModal.submsg
      this.selectedc = this.dataModal.conversion_char
      this.priority = this.dataModal.priority
      this.selectsubmsgno()
      this.selectconversion()
      // console.log('getid=====2', this.showboxdetail2);
      // }).catch(error => {
      //   console.error('Error fetching data:', error.message);
      // });
    },
    sendCanvasDataToAPI(obj) {
      const data = obj // or use: canvas.getObjects().map(obj => obj.toObject());
      console.log('ss========', data)
      this.showobj = obj
      axios.post('http://localhost:4000/boxstation2', data)
        .then((res) => {
          alert('Insert success!');
          console.log(res.data);
        })
        .catch((err) => {
          alert('Insert failed!');
          console.error(err);
        });
    },
    deleteActiveObject() {
      const activeObject = this.canvas.getActiveObject(); // or use ref to canvas

      if (!activeObject) {
        alert('No object selected');
        return;
      }

      // Call the same logic as your Fabric deleteObject()
      this.canvas.remove(activeObject);
      this.canvas.requestRenderAll();

      // Delete from backend if has ID
      if (activeObject.id) {
        axios.delete(`http://localhost:4000/boxstation2/${activeObject.id}`)
          .then(() => {
            console.log(`✅ Deleted object with ID ${activeObject.id}`);
          })
          .catch((err) => {
            console.error(`❌ Failed to delete object ID ${activeObject.id}`, err);
          });
      }
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
