<template>
  <div>
    <b-container>
      <b-alert v-if="alertStatus === 1" show variant="success" :show="dismissCountDown" fade
        @dismiss-count-down="countDownChanged"><a href="#" style="text-decoration:none"
          class="alert-link">อัพโหลดข้อมูลสำเร็จ</a></b-alert>
      <b-alert v-if="alertStatus === 3" show variant="danger" :show="dismissCountDown" fade
        @dismiss-count-down="countDownChanged"><a href="#" style="text-decoration:none"
          class="alert-link">อัพโหลดข้อมูลล้มเหลว</a></b-alert>
      <b-row>
        <div>
          <h1>
            Upload Excel file BC
          </h1>
          <!-- <div id="container"></div> -->
        </div>
        <div>
          <br>
          <b-row>
            <b-col>
              <div style="font-weight: bold;font-size: 20px;text-align: right;">
                Model
              </div>
            </b-col>
            <b-col>
              <b-input v-model="model" placeholder="Type your model"></b-input>
            </b-col>
            <b-col></b-col>
          </b-row>
        </div>
        <br>
        <br>
        <input type="file" ref="fileInput" @change="handleFileChangeTnos" />
        <br>
        <br>
        <div>
          <b-button variant="outline-success" v-on:click="biasbc">Download Excel</b-button>
        </div>
        <br>
        <br>
        <br>
        <div>
          <b-button variant="outline-info" v-on:click="biasbc2">Detail Data</b-button>
        </div>
        <!-- <br>
        <div>
          <table>
            <tr v-for="(items, index) in alldatabiasbc" :key="index">
              <input v-model="items.msgno"></input>
              <input v-model="items.msgnodescr"></input>
              <input v-model="items.lengths"></input>
              <input v-model="items.sequence"></input>
              <input v-model="items.fieldname"></input>
              <input v-model="items.converttype"></input>
              <input v-model="items.fieldupdate"></input>
              <input v-model="items.alc_start_length" v-on:keyup.enter="edittable(items)"></input>
              <input v-model="items.remark" v-on:keyup.enter="edittable(items)"></input>
            </tr>
          </table>
        </div> -->
      </b-row>
      <br>
      <br>
      <b-row>
        <div style="text-align: left;">
          <b-table hover :items="items" :head-variant="headVariant" :bordered="true" fixed="fixed"
            :sticky-header="stickyHeader"></b-table>
        </div>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import * as XLSX from 'xlsx';
import axios from 'axios';
export default {
  data() {
    return {
      alertStatus: 0,
      dismissSecs: 5,
      dismissCountDown: 0,
      showDismissibleAlert: false,
      alldatabiasbc: '',
      model: '',
      fields: [],
      items: [],
      headVariant: 'dark',
      stickyHeader: true
    }
  },
  mounted() {
    // let count = 5;
    // // Change this to any number of divs you want 
    // let container = document.getElementById('container');
    // for (let i = 1; i <= count; i++) {
    //   let div = document.createElement('div');
    //   div.textContent = 'Div ' + i;
    //   div.style.border = '1px solid black';
    //   div.style.padding = '10px';
    //   div.style.margin = '5px';
    //   let input = document.createElement('input');
    //   input.type = 'text';
    //   input.placeholder = 'Enter text here';
    //   input.id = 'input' + i;
    //   let button = document.createElement('button')
    //   button.id = 'button' + i
    //   button.textContent = 'click me'
    //   div.appendChild(input);
    //   div.appendChild(button);
    //   container.appendChild(div);
    //   button.onclick = function () {
    //     console.log('value input', input.value)
    //   };
    // }
  },
  methods: {
    handleFileChangeTnos(event) {
      console.log('tnos')
      const file = event.target.files[0];

      if (file) {
        this.readExcelTnos(file);
      }
    },
    async readExcelTnos(file) {
      const reader = new FileReader();

      reader.onload = (e) => {
        const data = e.target.result;
        const workbook = XLSX.read(data, { type: 'binary' });

        // Assume the first sheet is the one you want to read
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];

        // Convert the sheet data to JSON
        const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1 });

        // Set the JSON data to be displayed in the component
        this.jsonDataTnos = JSON.stringify(jsonData, null, 2);
        this.jsonDataTnos = JSON.parse(this.jsonDataTnos)
        this.jsonDataTnos.shift()
        let jsonobjectTnos = {}
        let jsonobject2Tnos = this.jsonDataTnos
        jsonobjectTnos = jsonobject2Tnos.map(innerarray => {
          return innerarray.reduce((acc, item, index) => {
            acc[`item${index + 1}`] = item;
            return acc
          }, {})
        })
        jsonobjectTnos = jsonobjectTnos.filter((i) => {
          return i.item1 !== null && i.item1 !== undefined
        })
        console.log('JSONTYPEOF2: ', jsonobjectTnos)
        const convertDate = (date) => {
          console.log('splitdata', data)
          let parts = date.split("/");
          return `${parts[2]}-${parts[1]}-${parts[0]}`;
        }
        let jsonMapTnos5 = jsonobjectTnos.map((data, i) => {
          return {
            idmsgno: data.item1,
            msgno: data.item2,
            msgnodescr: data.item3,
            lengths: data.item4,
            sequence: data.item5,
            fieldname: data.item6,
            converttype: data.item7,
            fieldupdate: data.item8,
            alc_start_length: data.item9,
            remark: data.item10,
            model: this.model
          }
        })
        this.jsondata2Tnos5 = jsonMapTnos5
        console.log(this.jsondata2Tnos5)
        axios.post('http://localhost:4000/diasbc', this.jsondata2Tnos5).then(response => {
          console.log(response.data);
          this.alertStatus = 1
          this.dismissCountDown = this.dismissSecs
        }).catch(error => {
          console.error('Error fetching data:', error.message);
          this.alertStatus = 3
          this.dismissCountDown = this.dismissSecs
        });
      };
      reader.readAsBinaryString(file);
    },
    biasbc() {
      axios.get('http://localhost:4000/alldiasbc')
        .then(response => {
          console.log('resdata', response.data.result);
          // let dataexcel = response.data.result
          // this.alldatabiasbc = response.data.result
          this.alldatabiasbc = response.data.result.filter(obj => obj.model === this.model);
          console.log('all data', this.alldatabiasbc)
          let jsonMaps = this.alldatabiasbc.map((data, i) => {
            return {
              "idmsgno": data.idmsgno,
              "msgno": data.msgno,
              "msgnodescr": data.msgnodescr,
              "lengths": data.lengths,
              "sequence": data.sequence,
              "fieldname": data.fieldname,
              "converttype": data.converttype,
              "fieldupdate": data.fieldupdate,
              "alc_start_length": data.alc_start_length,
              "remark": data.remark,
              "model": data.model
            }
          })
          // console.log('resdataExcel', jsonMaps);
          this.excelarray = Object.values(jsonMaps);
          // console.log('JSONTYPEOF2Aftermap23', this.excelarray)

          //export to excell
          const workbook = XLSX.utils.book_new();

          // Convert the JSON data to a worksheet
          const worksheet = XLSX.utils.json_to_sheet(this.excelarray);

          // Add the worksheet to the workbook
          XLSX.utils.book_append_sheet(workbook, worksheet, 'biasbc');

          // Save the workbook to a file
          XLSX.writeFile(workbook, 'exported_data.xlsx');
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    biasbc2() {
      axios.get('http://localhost:4000/alldiasbc')
        .then(response => {
          // console.log('resdata', response.data.result);
          let dataexcel = response.data.result
          // this.alldatabiasbc = response.data.result
          this.alldatabiasbc = response.data.result.filter(obj => obj.model === this.model);
          this.items = this.alldatabiasbc
          console.log('detail', this.alldatabiasbc)
          // let jsonMaps = dataexcel.map((data, i) => {
          //   return {
          //     "idmsgno": data.idmsgno,
          //     "msgno": data.msgno,
          //     "msgnodescr": data.msgnodescr,
          //     "lengths": data.lengths,
          //     "sequence": data.sequence,
          //     "fieldname": data.fieldname,
          //     "converttype": data.converttype,
          //     "fieldupdate": data.fieldupdate,
          //     "alc_start_length": data.alc_start_length,
          //     "remark": data.remark
          //   }
          // })
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    edittable(items) {
      console.log('edittable', items)
      axios.post('http://localhost:4000/updatebiasbc', items).then(response => {
        console.log(response.data);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown
    }
  }
}
</script>