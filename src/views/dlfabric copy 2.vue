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
        <!-- <b-modal ref="my-modal" hide-footer title="Using Component Methods" size="sm">
          <div class="d-block text-center">
            <h3>{{ dataModal.id }}</h3>
          </div>
        </b-modal> -->
        <b-modal ref="my-modal" size="xl" :title="'ID: ' + dataModal.id" hide-footer>
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
          <b-button variant="outline-warning"
            v-on:click="editBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
            style="margin: 10px;">Edit</b-button>
          <b-button variant="outline-danger"
            v-on:click="delBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
            style="margin: 10px;">Delete</b-button>
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
      <div>
        {{ canva }}
      </div>
      <!-- <button v-on:click="check()">GetAAAA</button> -->
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
      priority: ''
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
        text11: value,
        text2: value2,
        text3: value3,
        text4: value4,
        text5: value5

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
        const newText = document.getElementById('textInput2').value + '\n' + document.getElementById('textInput3').value
        const newText11 = document.getElementById('textInput').value
        const newText2 = document.getElementById('textInput2').value
        const newText3 = document.getElementById('textInput3').value
        const newText4 = document.getElementById('textInput4').value
        const newText5 = document.getElementById('textInput5').value
        activeObject.set('text', newText); // Update the text value
        activeObject.set('text11', newText11); // Update the text value
        activeObject.set('text2', newText2); // Update the text value
        activeObject.set('text3', newText3); // Update the text value
        activeObject.set('text4', newText4); // Update the text value
        activeObject.set('text5', newText5); // Update the text value
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
          text11: this.text11 || '',
          text2: this.text2 || '',
          text3: this.text3 || '',
          text4: this.text4 || '',
          text5: this.text5 || ''
        };
      };
    })(fabric.Object.prototype.toObject);

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
        // console.log('loadjson:=========', 'item.text11' + 'item.text2' + item.text3);
        if (type === 'text') {
          console.log('loadjson:=========', 'item.text11' + item.text2);
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
            text11: item.text11,
            text2: item.text2,
            text3: item.text3,
            text4: item.text4,
            text5: item.text5
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
    // let test = [
    //   {
    //     fontSize,
    //     fontWeight,
    //     fontFamily,
    //     fontStyle,
    //     lineHeight,
    //     text,
    //     charSpacing,
    //     textAlign,
    //     styles,
    //     pathStartOffset,
    //     pathSide,
    //     pathAlign,
    //     underline,
    //     overline,
    //     linethrough,
    //     textBackgroundColor,
    //     direction,
    //     type,
    //     version,
    //     originX,
    //     originY,
    //     left,
    //     top,
    //     width,
    //     height,
    //     fill,
    //     stroke,
    //     strokeWidth,
    //     strokeDashArray,
    //     strokeLineCap,
    //     strokeDashOffset,
    //     strokeLineJoin,
    //     strokeUniform,
    //     strokeMiterLimit,
    //     scaleX,
    //     scaleY,
    //     angle,
    //     flipX,
    //     flipY,
    //     opacity,
    //     shadow,
    //     visible,
    //     backgroundColor,
    //     fillRule,
    //     paintFirst,
    //     globalCompositeOperation,
    //     skewX,
    //     skewY,
    //     text11,
    //     text2,
    //     text3,
    //     text4,
    //     text5
    //   }
    // ]
    // loadCanvasFromArray(this.loadjson); // ← use your extracted data


    document.getElementById('loadJson').onclick = () => {
      loadCanvasFromArray(this.loadjson);
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
        // console.log('getid', response.data.result);
        // console.log('getidModal', document.getElementById('modal-shelf-top1'));
        this.showboxdetail = response.data.result
        // this.showboxdetail = this.showboxdetail.filter((i) => {
        //   let divboxid = document.getElementById(i.layout_location)
        //   divboxid.style.backgroundColor = 'lightblue'
        //   return i.layout_location == document.getElementById(i.layout_location).id
        // })
        // console.log('getid=====', this.showboxdetail);
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
      this.selecteds = this.dataModal.text11
      this.selectedm = this.dataModal.text2
      this.selectedmsg = this.dataModal.text3
      this.selectedc = this.dataModal.text4
      this.priority = this.dataModal.text5
      this.selectsubmsgno()
      this.selectconversion()
      // console.log('getid=====2', this.showboxdetail2);
      // }).catch(error => {
      //   console.error('Error fetching data:', error.message);
      // });
    },
    sendCanvasDataToAPI() {
      const data = this.loadjson; // or use: canvas.getObjects().map(obj => obj.toObject());

      axios.post('https://example.com/api/canvas', data)
        .then((res) => {
          alert('Insert success!');
          console.log(res.data);
        })
        .catch((err) => {
          alert('Insert failed!');
          console.error(err);
        });
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
