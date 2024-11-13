<template>
  <div>
    <b-container>
      <b-row>
        <b-alert v-if="alertStatus === 1" show variant="success" :show="dismissCountDown" fade
          @dismiss-count-down="countDownChanged"><a href="#" style="text-decoration:none"
            class="alert-link">อัพโหลดข้อมูลสำเร็จ</a></b-alert>
        <b-alert v-if="alertStatus === 3" show variant="danger" :show="dismissCountDown" fade
          @dismiss-count-down="countDownChanged"><a href="#" style="text-decoration:none"
            class="alert-link">อัพโหลดข้อมูลล้มเหลว</a></b-alert>
        <div>
          <h1>
            Upload Excel file Conversion
          </h1>
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
        <br>
        <div>
        </div>
      </b-row>
      <br>
      <br>
      <b-row>
        <div style="text-align: left;">
          <b-table hover :items="items" :head-variant="headVariant" :bordered="true" fixed="fixed"
            :sticky-header="stickyHeader"></b-table>
        </div>
      </b-row>
      <!-- <table>
        <tr v-for="(items, index) in alldatabiasbc" :key="index">
          <input v-model="items.idmsgno"></input>
          <input v-model="items.msgno"></input>
          <input v-model="items.conversioncharacter"></input>
          <input v-model="items.description"></input>
          <input v-model="items.mainpic"></input>
          <input v-model="items.timeadd"></input>
          <input v-model="items.og" v-on:keyup.enter="edittable(items)"></input>
          <input v-model="items.updates" v-on:keyup.enter="edittable(items)"></input>
        </tr>
      </table> -->
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
        const sheetName = workbook.SheetNames[1];
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
            msgno: data.item1,
            submsgno: data.item2,
            conversioncharacter: data.item3,
            description: data.item4,
            mainpic: data.item5,
            timeadd: data.item6,
            og: data.item7,
            updates: data.item8,
            model: this.model
          }
        })
        this.jsondata2Tnos5 = jsonMapTnos5
        console.log(this.jsondata2Tnos5)
        axios.post('http://localhost:4000/conversiondias', this.jsondata2Tnos5).then(response => {
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
      axios.get('http://localhost:4000/allconverdiasbc')
        .then(response => {
          console.log('resdata', response.data.result);
          let dataexcel = response.data.result
          this.alldatabiasbc = response.data.result.filter(obj => obj.model === this.model);
          // this.alldatabiasbc = response.data.result
          let jsonMaps = dataexcel.map((data, i) => {
            return {
              "idmsgno": data.msgno,
              "msgno": data.submsgno,
              "conversioncharacter": data.conversioncharacter,
              "description": data.description,
              "mainpic": data.mainpic,
              "timeadd": data.timeadd,
              "og": data.og,
              "updates": data.updates,
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
          XLSX.utils.book_append_sheet(workbook, worksheet, 'converbiasbc');

          // Save the workbook to a file
          XLSX.writeFile(workbook, 'exported_data.xlsx');
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    biasbc2() {
      axios.get('http://localhost:4000/allconverdiasbc')
        .then(response => {
          // console.log('resdata', response.data.result);
          let dataexcel = response.data.result
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
      axios.post('http://localhost:4000/updateconverbiasbc', items).then(response => {
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