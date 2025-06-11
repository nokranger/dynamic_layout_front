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
        <br>
        <!-- เปลี่ยนเป็นส่วนเลือกรูป background -->
        <div style="font-weight: bold;">
          Background Image (Optional)
        </div>
        <div>
          <input type="file" id="imageInput" accept="image/*" @change="handleImageUpload"
            style="margin-bottom: 10px; padding: 5px; border: 1px solid #ccc; border-radius: 4px; width: 30%;">
          <div v-if="uploadedImageSrc" style="margin-top: 10px;">
            <img :src="uploadedImageSrc" style="max-width: 100px; max-height: 100px; border: 1px solid #ccc;">
            <span style="margin-left: 10px; color: green;">✓ Image ready to use as background</span>
          </div>
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
          <b-button variant="outline-danger" @click="$bvModal.hide('bv-modal-example')"
            style="margin: 10px;">Close</b-button>
        </b-modal>
      </b-container>

      <br>
      <br>
      <div align="left">
        <b-button style="margin: 10px;" variant="outline-primary" id="addElement">Add Element</b-button>
        <b-button style="margin: 10px;" variant="outline-secondary" id="editText">Edit Element</b-button>
        <b-button style="margin: 10px;" variant="outline-success" id="getJson">Save Layout</b-button>
        <b-button style="margin: 10px;" variant="outline-primary" id="loadJson">Load Layout</b-button>
      </div>
      <br>
      <br>

      <canvas id="myCanvas" ref="canvas" width="1200" height="800" style="outline: black 3px solid;"></canvas>
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
      loadjson: [],
      showModal: false,
      modalText: '',
      activeObject: null,
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
      showobj: '',
      uploadedImageSrc: null
    }
  },

  mounted() {
    this.loadlocation()
    const canvas = new fabric.Canvas(this.$refs.canvas);

    // เก็บ reference ของ canvas ไว้ใน component
    this.canvas = canvas;

    // Function to generate unique ID for each object
    function generateUniqueId() {
      return 'id_' + Math.random().toString(36).substr(2, 9);
    }

    // รวมฟังก์ชัน Add text และ image เป็นฟังก์ชันเดียว
    function AddElement() {
      const value = document.getElementById('textInput').value || 'Default Text';
      const value2 = document.getElementById('textInput2').value || 'Default Text';
      const value3 = document.getElementById('textInput3').value || 'Default Text';
      const value4 = document.getElementById('textInput4').value || 'Default Text';
      const value5 = document.getElementById('textInput5').value || 'Default Text';

      console.log('value========', value + value2 + value3 + value4 + value5)

      const uniqueId = generateUniqueId();
      const textContent = value2 + '\n' + value3;

      // ถ้ามีรูป background
      if (window.vueInstance.uploadedImageSrc) {
        const imgElement = new Image();
        const uploadedSrc = window.vueInstance.uploadedImageSrc; // เก็บไว้กันเปลี่ยนตอน async

        imgElement.onload = function () {
          const backgroundImage = new fabric.Image(imgElement, {
            left: 0,
            top: 0,
            scaleX: 0.3,
            scaleY: 0.3,
            opacity: 0.7,
            selectable: false
          });

          const text = new fabric.Text(textContent, {
            left: backgroundImage.width * backgroundImage.scaleX / 2,
            top: backgroundImage.height * backgroundImage.scaleY / 2,
            fill: 'black',
            fontSize: 24,
            textAlign: 'center',
            originX: 'center',
            originY: 'center',
            selectable: false,
            fontWeight: 'bold',
            stroke: 'white',
            strokeWidth: 1
          });

          const group = new fabric.Group([backgroundImage, text], {
            left: 100,
            top: 100,
            id: uniqueId,
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
            hasBackgroundImage: true,
            backgroundImageSrc: uploadedSrc  // ✅ ใช้จากตัวแปรที่เรากำหนดไว้ก่อน
          });
          // เพิ่ม control ให้ group ทันที
          group.controls.deleteControl = new fabric.Control({
            x: 0.5,
            y: -0.5,
            offsetY: -16,
            cursorStyle: 'pointer',
            mouseUpHandler: deleteObject,
            render: renderIcon,
            cornerSize: 24,
          });

          group.controls.infoIcon = new fabric.Control({
            x: -0.5,
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
              window.__vueShowInfoModal?.(obj);
              return true;
            }
          });

          console.log('📸 Group created with backgroundImageSrc:', uploadedSrc);

          // Add controls ...
          // (เหมือนเดิม)

          canvas.add(group);
          canvas.setActiveObject(group);
          canvas.requestRenderAll();
        };

        imgElement.src = uploadedSrc;
      } else {
        // สร้างแค่ข้อความอย่างเดิมถ้าไม่มีรูป
        const text = new fabric.Text(textContent, {
          left: 100,
          top: 100,
          fill: 'black',
          fontSize: 24,
          id: uniqueId,
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
          backgroundColor: 'rgba(255, 255, 255, 0.8)',
          rx: 10,
          ry: 10,
          hasBackgroundImage: false
        });

        // Add custom controls
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
          x: -0.5,
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
            window.__vueShowInfoModal?.(obj);
            return true;
          }
        });

        canvas.add(text);
        canvas.setActiveObject(text);
        canvas.requestRenderAll();
        console.log('New text element created with ID:', uniqueId);
      }

      // ล้างรูปที่อัพโหลดหลังจากใช้แล้ว
      window.vueInstance.uploadedImageSrc = null;
      document.getElementById('imageInput').value = '';
    }

    // Delete control handler
    function deleteObject(eventData, transform) {
      const target = transform.target;
      const canvas = target.canvas;
      canvas.remove(target);
      canvas.requestRenderAll();

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
    deleteImg.src = 'http://localhost:4000/uploads/trash--v1.png';

    const iconImg = new Image();
    iconImg.src = 'http://localhost:4000/uploads/info.png';

    function renderIcon(ctx, left, top, styleOverride, fabricObject) {
      const size = this.cornerSize;
      ctx.save();
      ctx.translate(left, top);
      ctx.rotate(fabric.util.degreesToRadians(fabricObject.angle));
      ctx.drawImage(deleteImg, -size / 2, -size / 2, size, size);
      ctx.restore();
    }

    // Handle element editing
    document.getElementById('editText').onclick = () => {
      const activeObject = canvas.getActiveObject();
      if (activeObject) {
        const newText = document.getElementById('textInput2').value + '\n' + document.getElementById('textInput3').value
        const newstation_id = document.getElementById('textInput').value
        const newmodel = document.getElementById('textInput2').value
        const newsubmsg = document.getElementById('textInput3').value
        const newconversion_char = document.getElementById('textInput4').value
        const newpriority = document.getElementById('textInput5').value

        // ถ้าเป็น group (มี background image)
        if (activeObject.type === 'group') {
          // อัพเดทข้อความใน group
          const textObject = activeObject.getObjects().find(obj => obj.type === 'text');
          if (textObject) {
            textObject.set('text', newText);
          }
        } else if (activeObject.type === 'text') {
          // อัพเดทข้อความปกติ
          activeObject.set('text', newText);
        }

        // อัพเดท properties อื่นๆ
        activeObject.set('station_id', newstation_id);
        activeObject.set('model', newmodel);
        activeObject.set('submsg', newsubmsg);
        activeObject.set('conversion_char', newconversion_char);
        activeObject.set('priority', newpriority);

        canvas.renderAll();
        console.log(`Updated element with ID: ${activeObject.id}`);
      }
    };

    // Event listeners
    canvas.on('object:scaling', function (e) {
      const obj = e.target;
      console.log('Object resized');
      console.log(`New width: ${obj.width}, New height: ${obj.height}`);
    });

    canvas.on('object:modified', function (e) {
      const obj = e.target;
      if (obj.angle === 0 || obj.angle === 180) {
        console.log('Object flipped');
        console.log(`Object angle: ${obj.angle}`);
      }
      console.log(`Updated position: left=${obj.left}, top=${obj.top}`);
      console.log(`Updated size: width=${obj.width}, height=${obj.height}`);
    });

    // Extend toObject method for custom properties
    // fabric.Object.prototype.toObject = (function (toObject) {
    //   return function (...args) {
    //     return {
    //       ...toObject.call(this, ...args),
    //       station_id: this.station_id || '',
    //       model: this.model || '',
    //       submsg: this.submsg || '',
    //       conversion_char: this.conversion_char || '',
    //       priority: this.priority || '',
    //       // objects: this._objects.map(obj => obj.toObject()),  // ✅ สำคัญมาก
    //       hasBackgroundImage: this.hasBackgroundImage || false,
    //       backgroundImageSrc: this.backgroundImageSrc || ''
    //     };
    //   };
    // })(fabric.Object.prototype.toObject);

    // // เก็บ custom field สำหรับทุก object
    // fabric.Object.prototype.toObject = (function (toObject) {
    //   return function (...args) {
    //     return {
    //       ...toObject.call(this, ...args),
    //       station_id: this.station_id || '',
    //       model: this.model || '',
    //       submsg: this.submsg || '',
    //       conversion_char: this.conversion_char || '',
    //       priority: this.priority || '',
    //       hasBackgroundImage: this.hasBackgroundImage || false,
    //       backgroundImageSrc: this.backgroundImageSrc || ''
    //     };
    //   };
    // })(fabric.Object.prototype.toObject);

    // // เก็บ object ภายในกลุ่มด้วย
    // fabric.Group.prototype.toObject = (function (toObject) {
    //   return function (...args) {
    //     return {
    //       ...toObject.call(this, ...args),
    //       objects: this._objects.map(obj => obj.toObject()),
    //       backgroundImageSrc: this.backgroundImageSrc || '',
    //       hasBackgroundImage: this.hasBackgroundImage || false
    //     };
    //   };
    // })(fabric.Group.prototype.toObject);

    // Save canvas JSON
    document.getElementById('getJson').onclick = async () => {
      fabric.Object.prototype.toObject = (function (toObject) {
        return function (...args) {
          return {
            ...toObject.call(this, ...args),
            id: this.id || '',
            station_id: this.station_id || '',
            model: this.model || '',
            submsg: this.submsg || '',
            conversion_char: this.conversion_char || '',
            priority: this.priority || '',
            hasBackgroundImage: this.hasBackgroundImage || false,
            backgroundImageSrc: this.backgroundImageSrc || ''
          };
        };
      })(fabric.Object.prototype.toObject);

      // เก็บ object ภายในกลุ่มด้วย
      fabric.Group.prototype.toObject = (function (toObject) {
        return function (...args) {
          return {
            ...toObject.call(this, ...args),
            id: this.id || '',
            objects: this._objects.map(obj => obj.toObject()),
            backgroundImageSrc: this.backgroundImageSrc || '',
            hasBackgroundImage: this.hasBackgroundImage || false
          };
        };
      })(fabric.Group.prototype.toObject);

      const json = canvas.toJSON();
      this.canva = JSON.stringify(json, null, 2);
      const objectsData = canvas.getObjects().map(obj => obj.toObject());
      this.loadjson = objectsData;
      await this.sendCanvasDataToAPI(this.loadjson);
    };

    // Load canvas from array
    // ปรับ loadCanvasFromArray ให้ใช้ image path แทน base64
    function loadCanvasFromArray(dataArray) {
      canvas.clear();

      dataArray.forEach((item) => {
        const type = item.type?.toLowerCase();
        const id = item.id || generateUniqueId();

        // 👉 TEXT ONLY
        if (type === 'text') {
          const text = new fabric.Text(item.text || '', {
            ...item,
            id,
            cornerStyle: 'circle',
            cornerStrokeColor: 'blue',
            cornerColor: 'lightblue',
            padding: 10,
            transparentCorners: false,
            borderColor: 'orange',
            borderDashArray: [3, 1, 3],
            borderScaleFactor: 2,
            backgroundColor: 'rgba(255, 255, 255, 0.8)',
            rx: 10,
            ry: 10
          });

          // Custom controls
          text.controls.deleteControl = new fabric.Control({
            x: 0.5, y: -0.5, offsetY: -16,
            cursorStyle: 'pointer',
            mouseUpHandler: deleteObject,
            render: renderIcon,
            cornerSize: 24,
          });

          text.controls.infoIcon = new fabric.Control({
            x: -0.5, y: -0.5, offsetY: -10, offsetX: -10,
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
              window.__vueShowInfoModal?.(obj);
              return true;
            }
          });

          canvas.add(text);
        }

        // 👉 GROUP WITH BACKGROUND IMAGE
        else if (type === 'group') {
          console.log('🧠 Loading group text:', item.objects?.[1]?.text);
          const imgElement = new Image();
          imgElement.onload = function () {
            const backgroundImage = new fabric.Image(imgElement, {
              ...item.objects?.[0],
              left: 0,
              top: 0,
              scaleX: 0.3,
              scaleY: 0.3,
              opacity: 0.7,
              selectable: false
            });

            // ✅ สร้างข้อความจาก model + submsg
            const textContent = `${item.model || ''}\n${item.submsg || ''}`;

            const text = new fabric.Text(textContent, {
              left: backgroundImage.width * backgroundImage.scaleX / 2,
              top: backgroundImage.height * backgroundImage.scaleY / 2,
              fill: 'black',
              fontSize: 24,
              textAlign: 'center',
              originX: 'center',
              originY: 'center',
              selectable: false,
              fontWeight: 'bold',
              stroke: 'white',
              strokeWidth: 1
            });

            const group = new fabric.Group([backgroundImage, text], {
              ...item,
              id,
              text
            });

            // Controls
            group.controls.deleteControl = new fabric.Control({
              x: 0.5, y: -0.5, offsetY: -16,
              cursorStyle: 'pointer',
              mouseUpHandler: deleteObject,
              render: renderIcon,
              cornerSize: 24,
            });

            group.controls.infoIcon = new fabric.Control({
              x: -0.5, y: -0.5, offsetY: -10, offsetX: -10,
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
                window.__vueShowInfoModal?.(obj);
                return true;
              }
            });

            canvas.add(group);
            canvas.renderAll();
          };

          imgElement.onerror = () => {
            console.error('❌ Image load failed:', item.backgroundImageSrc);
          };

          imgElement.src = 'http://localhost:4000' + item.backgroundImageSrc;
        }
      });

      canvas.renderAll();
    }

    // Load JSON button
    document.getElementById('loadJson').onclick = () => {
      let datastation = {
        station: this.selecteds
      }
      axios.post('http://localhost:4000/allboxstationCanva', datastation)
        .then((res) => {
          console.log('getdata success======', res);
          this.loadjson = res.data.result.map(item => ({
            ...item,
            text: item.model + '\n' + item.submsg,
            underline: !!item.underline,
            overline: !!item.overline,
            linethrough: !!item.linethrough,
            strokeUniform: !!item.strokeUniform,
            flipX: !!item.flipX,
            flipY: !!item.flipY,
            visible: !!item.visible,
            styles: JSON.parse(item.styles || '[]'),
            strokeDashArray: JSON.parse(item.strokeDashArray || '[]'),
            shadow: JSON.parse(item.shadow || '{}'),
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
            backgroundColor: 'rgba(255, 255, 255, 0.8)',
            rx: 10,
            ry: 10,
            hasBackgroundImage: !!item.hasBackgroundImage,
            backgroundImageSrc: item.backgroundImageSrc,
          }));
          this.showobj = this.loadjson
          // console.log('getdata success======', item.type, item.objects);
          loadCanvasFromArray(this.loadjson);
        })
        .catch((err) => {
          alert('Load Canva Fail!');
          console.error(err);
          console.log('err:', err);
        });
    };

    // Hook up buttons - เปลี่ยนเป็นฟังก์ชันเดียว
    document.getElementById('addElement').onclick = () => AddElement();

    // Set up Vue modal handler
    window.__vueShowInfoModal = (obj) => {
      this.openInfoModal(obj);
      this.showdetail()
    };

    // เก็บ reference ของ Vue instance สำหรับใช้ใน AddElement function
    window.vueInstance = this;
  },

  methods: {
    // เมธอดสำหรับจัดการการอัพโหลดรูป
    handleImageUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.uploadedImageSrc = e.target.result;
          console.log('Image uploaded and ready to use as background');
        };
        reader.readAsDataURL(file);
      }
    },

    check() {
      console.log('divv', this.textTojson)
    },

    openInfoModal(obj) {
      console.log('obj===', obj)
      this.$refs['my-modal'].show()
      this.dataModal = obj
    },

    closeInfoModal() {
      // Modal close logic
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
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });

      axios.get('http://localhost:4000/alldiasbc2').then(response => {
        console.log(response.data);
        this.model = response.data.result.map((data, i) => {
          return {
            value: data.model,
            text: data.model
          }
        })
        this.model.push({ "value": null, "text": "Please select an option" })
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
        this.submsgno = response.data.result
        console.log('mmm====', this.selectedm)
        this.submsgno = this.submsgno.filter((i) => {
          console.log('ccc====', i.model)
          return i.model == this.selectedm
        })
        this.submsgno = this.submsgno.map((data, i) => {
          return {
            value: data.submsgno,
            text: data.submsgno
          }
        })
        this.submsgno.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },

    selectconversion() {
      axios.get('http://localhost:4000/allconversion').then(response => {
        console.log(response.data);
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
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },

    showdetail() {
      this.selecteds = this.dataModal.station_id
      this.selectedm = this.dataModal.model
      this.selectedmsg = this.dataModal.submsg
      this.selectedc = this.dataModal.conversion_char
      this.priority = this.dataModal.priority
      this.selectsubmsgno()
      this.selectconversion()
    },

    // async sendCanvasDataToAPI(obj) {
    //   const formData = new FormData();
    //   const canvasData = [];

    //   for (let i = 0; i < obj.length; i++) {
    //     const item = obj[i];
    //     const data = { ...item };

    //     // ถ้ามีรูป base64 ให้อัปโหลดเป็นไฟล์
    //     if (item.hasBackgroundImage && item.backgroundImageSrc?.startsWith('data:image/')) {
    //       const blob = await (await fetch(item.backgroundImageSrc)).blob();
    //       const filename = `bg_${Date.now()}_${i}.png`;
    //       formData.append('images', blob, filename);
    //       data.backgroundImageSrc = `/uploads/${filename}`; // เก็บ path ไว้ใน DB
    //     }

    //     canvasData.push(data);
    //   }

    //   formData.append('canvasData', JSON.stringify(canvasData));
    //   console.log('showimage=====', canvasData)
    //   for (let i = 0; i < obj.length; i++) {
    //     const item = obj[i];

    //     if (item.hasBackgroundImage && item.backgroundImageSrc?.startsWith('data:image/')) {
    //       const blob = await (await fetch(item.backgroundImageSrc)).blob();
    //       const filename = `bg_${Date.now()}_${i}.png`;
    //       formData.append('images', blob, filename);

    //       // ✅ สำคัญ! ตั้งค่าชื่อไฟล์สำหรับเก็บใน DB
    //       item.backgroundImageSrc = `/uploads/${filename}`;
    //     }
    //   }
    //   axios.post('http://localhost:4000/boxstation2/upload', formData, {
    //     headers: { 'Content-Type': 'multipart/form-data' }
    //   })
    //     .then((res) => {
    //       alert('Insert success!');
    //       console.log(res.data);
    //     })
    //     .catch((err) => {
    //       alert('Insert failed!');
    //       console.error(err);
    //     });
    // },
    async sendCanvasDataToAPI(obj) {
      const formData = new FormData();
      const canvasData = [];

      for (let i = 0; i < obj.length; i++) {
        const item = { ...obj[i] };

        // ✅ ดึง objects จาก group
        if (item.type === 'group' && typeof item.objects === 'undefined' && obj[i]._objects) {
          item.objects = obj[i]._objects.map(o => o.toObject());
          const textObj = item.objects.find(obj => obj.type === 'text');
          if (textObj) {
            item.text = textObj.text || '';
            item.fontSize = textObj.fontSize || '';
            item.fontWeight = textObj.fontWeight || '';
            item.fontFamily = textObj.fontFamily || '';
            item.fontStyle = textObj.fontStyle || '';
            item.lineHeight = textObj.lineHeight || '';
            item.charSpacing = textObj.charSpacing || '';
            item.textAlign = textObj.textAlign || '';
            item.styles = textObj.styles || [];
            item.fill = textObj.fill || '';
            item.stroke = textObj.stroke || '';
            item.strokeWidth = textObj.strokeWidth || '';
          }
        }

        if (item.hasBackgroundImage && item.backgroundImageSrc?.startsWith('data:image/')) {
          const blob = await (await fetch(item.backgroundImageSrc)).blob();
          const filename = `bg_${Date.now()}_${i}.png`;

          formData.append('images', blob, filename);
          item.backgroundImageSrc = `/uploads/${filename}`;
        }

        canvasData.push(item);
      }

      formData.append('canvasData', JSON.stringify(canvasData));
      axios.post('http://localhost:4000/boxstation2/upload', formData, {
        headers: { 'Content-Type': 'multipart/form-data' }
      })
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
      const activeObject = this.canvas.getActiveObject();

      if (!activeObject) {
        alert('No object selected');
        return;
      }

      this.canvas.remove(activeObject);
      this.canvas.requestRenderAll();

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

/* ปรับ style สำหรับการอัพโหลดรูป */
#imageInput {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 30%;
}

/* เพิ่ม style สำหรับ preview รูป */
.image-preview {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #f9f9f9;
}

.image-preview img {
  max-width: 100px;
  max-height: 100px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.image-ready-indicator {
  margin-left: 10px;
  color: green;
  font-weight: bold;
}
</style>