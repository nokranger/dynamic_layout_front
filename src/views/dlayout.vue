<template>
  <div>
    <b-container>
      <b-row>
        <div>
          <!-- <div id="container"></div> -->
          <!-- <b-button v-b-modal.modal-1>Launch demo modal</b-button> -->
          <!-- <div v-for="(alldatabiasbcs, index) in alldatabiasbc" :key="index">
            <b-modal :id="'modal-' + index" title="BootstrapVue">
              {{ index }}
              <p class="my-4">Hello from modal!</p>
            </b-modal>
          </div> -->
          <b-button v-on:click="reloadWork" variant="outline-primary">Reload</b-button>
          <div
            style="border: 2px solid gray;border-radius: 10px;box-shadow: 5px 5px 5px #888888;margin: 20px;width: 100%;height: 100%">
            <div v-for="(lworks, index) in lwork" :key="index"
              style="position: static;display: inline-block;border: 2px solid gray;border-radius: 10px;height: 450px;width: 250px;box-shadow: 5px 5px 5px #888888;margin: 10px;"
              v-b-modal="'modal-shelf' + index" v-on:click="loadLayout(lworks)">
              <div v-on:click="loadLayout(index)">Name: {{ lworks.name }}</div>
              <div v-on:click="loadLayout(index)">Picking Sequence: {{ lworks.picking_sequence }}</div>
              <div v-on:click="loadLayout(index)">Lot Size{{ lworks.lot_size }}</div>
              <div v-on:click="loadLayout(index)">Condition{{ lworks.condition }}</div>
              <div >BC Model: {{ bc[0].model }}</div>
              <b-modal :id="'modal-shelf' + index" :title="lworks.name" size="lg" hide-footer hide-header>
                <div v-for="n in llevel" :key="n">
                  <div v-for="(llayouts, index) in llayout" :key="index"
                    style="position: static;display: inline-block;border: 2px solid gray;border-radius: 10px;height: 450px;width: 250px;box-shadow: 5px 5px 5px #888888;margin: 10px;">
                    <div>Level: {{ n }}</div>
                    <div>Name Station: {{ llayouts.name_station }}</div>
                    <div>Layout Part Number: {{ llayouts.layout_part_number }}</div>
                    <div>Layout Quantity: {{ llayouts.layout_quantity }}</div>
                  </div>
                </div>
              </b-modal>
            </div>
          </div>
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
      lmodel: '',
      lwork: '',
      llayout: '',
      llevel: 0,
      bc: '',
      conversion: ''
    }
  },
  async mounted() {
    // await axios.get('http://localhost:4000/allworkstation')
    //   .then(response => {
    //     this.alldatabiasbc = response.data.result
    //     console.log('detail', this.alldatabiasbc.length)
    //   })
    //   .catch(error => {
    //     console.error('Error fetching data:', error.message);
    //   });
    // let count = 4;
    // // Change this to any number of divs you want 
    // let container = document.getElementById('container');
    // for (let i = 0; i <= this.alldatabiasbc.length; i++) {
    //   let div = document.createElement('div');
    //   div.innerHTML = 'Name Station: ' + this.alldatabiasbc[i].name + '<br>' + 'Sequence: ';
    //   div.style.border = '1px solid black';
    //   div.style.padding = '10px';
    //   div.style.margin = '5px';
    //   // let input = document.createElement('input');
    //   // input.type = 'text';
    //   // input.placeholder = 'Enter text here';
    //   // input.id = 'input' + i;
    //   let button = document.createElement('button')
    //   button.id = 'button' + i
    //   button.textContent = 'click me'
    //   // div.appendChild(input);
    //   div.appendChild(button);
    //   container.appendChild(div);
    //   button.onclick = function () {

    //     // console.log('value input', input.value)
    //   };
    // }
  },
  methods: {
    async reloadWork() {
      await axios.get('http://localhost:4000/allworkstation')
        .then(response => {
          this.lwork = response.data.result
          console.log('detail', this.lwork.length)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
        await axios.get('http://localhost:4000/allmodel')
        .then(response => {
          this.bc = response.data.result
          console.log('detail', this.bc[0].model)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    async loadLayout(lworks) {
      console.log('loadLayout', lworks)
      await axios.get('http://localhost:4000/alllayout')
        .then(response => {
          this.llayout = response.data.result
          this.llayout = this.llayout.filter((i) => {
            return i.station_id == lworks.station_id
          })
          console.log('detail', typeof (this.llayout[0].layout_level))
          this.llevel = Number(this.llayout[0].layout_level)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
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
          this.alldatabiasbc = response.data.result
          console.log('detail', this.alldatabiasbc)
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