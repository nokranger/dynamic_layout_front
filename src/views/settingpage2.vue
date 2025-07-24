<template>
  <div>
    <b-alert v-if="alertStatus === 1" show variant="success" :show="dismissCountDown" fade
      @dismiss-count-down="countDownChanged"><a href="#" style="text-decoration:none"
        class="alert-link">เพิ่มข้อมูลสำเร็จ</a></b-alert>
    <b-alert v-if="alertStatus === 3" show variant="danger" :show="dismissCountDown" fade
      @dismiss-count-down="countDownChanged"><a href="#" style="text-decoration:none"
        class="alert-link">เพิ่มข้อมูลล้มเหลว</a></b-alert>
    <h1>
      Setting page Working Station
    </h1>
    <div>
      <b-container>
        <b-row style="text-align: left;">
          <b-col></b-col>
          <b-col>
            <div>
              <div style="font-weight: bold;">
                Name Station
              </div>
              <div>
                <b-form-select v-model="selecteds" :options="stationO" v-on:change="CallStation"></b-form-select>
              </div>
              <br>
              <div>
                <div>
                  <b-input v-model="nameStation" placeholder="Type Your Name Station"></b-input>
                </div>
              </div>
            </div>
            <br>
            <div>
              <div style="font-weight: bold;">
                Picking Sequence
              </div>
              <div>
                <div>
                  <b-form-select v-model="selected" :options="options"></b-form-select>
                </div>
              </div>
            </div>
            <br>
            <div>
              <div style="font-weight: bold;">
                Lot Size
              </div>
              <div>
                <div>
                  <b-form-select v-model="selected2" :options="options2"></b-form-select>
                </div>
              </div>
            </div>
            <br>
            <div>
              <div style="font-weight: bold;">
                Condition
              </div>
              <div>
                <div>
                  <b-form-select v-model="selected3" :options="options3"></b-form-select>
                </div>
              </div>
            </div>
            <br>
            <div>
              <div style="font-weight: bold;">
                A
              </div>
              <div>
                <b-row>
                  <b-col cols="5">
                    <div>
                      <b-form-select v-model="selectmsgA" :options="submsgnoA"></b-form-select>
                    </div>
                  </b-col>
                  <b-col cols="5" style="margin-left: 50px;">
                    <div>
                      <b-form-select v-model="selectedkeyseqA" :options="keyseqA"
                        v-on:change="concatStringA(selectmsgA, selectedkeyseqA)"></b-form-select>
                    </div>
                  </b-col>
                </b-row>
              </div>
              <br>
              <div>
                <div v-for="(item, index) in stringA" :key="index">
                  <b-input :value="item.submsg + ', ' + item.part_name" placeholder="..."></b-input>
                </div>
              </div>
            </div>
            <br>
            <div>
              <div style="font-weight: bold;">
                B
              </div>
              <div>
                <b-row>
                  <b-col cols="5">
                    <div>
                      <b-form-select v-model="selectmsgB" :options="submsgnoB"></b-form-select>
                    </div>
                  </b-col>
                  <b-col cols="5" style="margin-left: 50px;">
                    <div>
                      <b-form-select v-model="selectedkeyseqB" :options="keyseqB"
                        v-on:change="concatStringB(selectmsgB, selectedkeyseqB)"></b-form-select>
                    </div>
                  </b-col>
                </b-row>
              </div>
              <br>
              <div>
                <div v-for="(item, index) in stringB" :key="index">
                  <b-input :value="item.submsg + ', ' + item.part_name" placeholder="..."></b-input>
                </div>
              </div>
            </div>
          </b-col>
          <b-col></b-col>
        </b-row>
        <br>
        <b-row>
          <b-col>
            <b-button variant="outline-primary" style="margin: 5px;" v-on:click="addSetting()">Add Station</b-button>
          </b-col>
          <b-col>
            <b-button variant="outline-primary" style="margin: 5px;" v-on:click="updateStation()">Update
              Station</b-button>
          </b-col>
          <b-col>
            <b-button variant="outline-danger" style="margin: 5px;" v-on:click="deleteStation()">Delete
              Station</b-button>
          </b-col>
        </b-row>
        {{ setting }}
        <br>
      </b-container>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      items: [
        { TEST: 1, test: '', testt: '' },
        { TEST: 2, test: '', testt: '' },
        { TEST: 3, test: '', testt: '' },
        { TEST: 4, test: '', testt: '' }
      ],
      setting: [],
      selected: null,
      options: [
        { value: null, text: 'Please select an option' },
        { value: 'Ascending', text: 'Ascending' },
        { value: 'Descending', text: 'Descending' },
        { value: 'Shoping', text: 'Shoping' }
      ],
      selected2: null,
      options2: [
        { value: null, text: 'Please select an option' },
        { value: '3', text: '3' },
        { value: '6', text: '6' },
        { value: '9', text: '9' },
        { value: '12', text: '12' }
      ],
      selected3: null,
      options3: [
        { value: null, text: 'Please select an option' },
        { value: 'Single', text: 'Single' },
        { value: 'Pairing', text: 'Pairing' }
      ],
      nameStation: '',
      alertStatus: 0,
      dismissSecs: 5,
      dismissCountDown: 0,
      showDismissibleAlert: false,
      selectmsgA: null,
      submsgnoA: [],
      selectmsgB: null,
      submsgnoB: [],
      stringA: "",
      stringB: "",
      selecteds: null,
      stationO: [],
      CallStationData: '',
      selectedkeyseqA: null,
      keyseqA: [],
      selectedkeyseqB: null,
      keyseqB: [],
    }
  },
  async mounted() {
    await this.selectsubmsgnoA()
    await this.selectsubmsgnoB()
    await this.selectkeyseqA()
    await this.selectkeyseqB()
    await this.loadlocation()
  },
  methods: {
    addSetting() {
      this.setting = {
        namestation: this.nameStation,
        picking: this.selected,
        lot_size: this.selected2,
        condition: this.selected3,
        sa: JSON.stringify(this.stringA),
        sb: JSON.stringify(this.stringB)
      }
      console.log('setting', this.setting)
      axios.post('http://localhost:4000/settingworking', this.setting).then(response => {
        console.log(response.data);
        this.alertStatus = 1
        this.dismissCountDown = this.dismissSecs
        this.nameStation = ''
      }).catch(error => {
        console.error('Error fetching data:', error.message);
        this.alertStatus = 3
        this.dismissCountDown = this.dismissSecs
        this.nameStation = ''
      });
    },
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown
    },
    CallStation() {
      console.log('allstation', this.selecteds)
      let dataStation = {
        station_id: this.selecteds
      }
      axios.post('http://localhost:4000/Callallstation', dataStation).then(response => {
        this.CallStationData = response.data.result
        console.log('allstation', response.data.result)
        this.nameStation = this.CallStationData[0].name
        this.selected = this.CallStationData[0].picking_sequence
        this.selected2 = this.CallStationData[0].lot_size
        this.selected3 = this.CallStationData[0].condition
        this.stringA = this.CallStationData[0].sa
        this.stringB = this.CallStationData[0].sb
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    updateStation() {
      let dataStationUpdate = {
        station_id: this.selecteds,
        name: this.nameStation,
        picking_sequence: this.selected,
        lot_size: this.selected2,
        condition: this.selected3,
        sa: JSON.stringify(this.stringA),
        sb: JSON.stringify(this.stringB)
      }
      axios.post('http://localhost:4000/updateallstation', dataStationUpdate).then(response => {
        // console.log('allstation', response.data.result)
        this.selecteds = null
        this.nameStation = ""
        this.selected = null
        this.selected2 = null
        this.selected3 = null
        this.stringA = ""
        this.stringB = ""
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    deleteStation() {
      let dataStationUpdate = {
        station_id: this.selecteds,
      }
      axios.post('http://localhost:4000/deletestation', dataStationUpdate).then(response => {
        // console.log('allstation', response.data.result)
        this.selecteds = null
        this.nameStation = ""
        this.selected = null
        this.selected2 = null
        this.selected3 = null
        this.stringA = ""
        this.stringB = ""
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
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

    },
    async selectsubmsgnoA() {
      console.log('AAAAAAAAA====')
      await axios.get('http://localhost:4000/allconversion').then(response => {
        // console.log(response.data);
        // this.options = response.data.result
        this.submsgnoA = response.data.result
        // console.log('mmm====', this.selectedm)
        // this.submsgno = this.submsgno.filter((i) => {
        //   console.log('ccc====', i.model)
        //   // console.log('ccc====', i.submsgno)
        //   return i.model == this.selectedm
        // })
        // console.log('allmsghno', this.submsgno)
        this.submsgnoA = [...new Set(this.submsgnoA.map(item => item.submsgno))].map((unique) => {
          return {
            value: unique,
            text: unique
          };
        });
        this.submsgnoA.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async selectsubmsgnoB() {
      console.log('BBBBBBBBB====')
      await axios.get('http://localhost:4000/allconversion').then(response => {
        // console.log(response.data);
        // this.options = response.data.result
        this.submsgnoB = response.data.result
        // console.log('mmm====', this.selectedm)
        // this.submsgno = this.submsgno.filter((i) => {
        //   console.log('ccc====', i.model)
        //   // console.log('ccc====', i.submsgno)
        //   return i.model == this.selectedm
        // })
        // console.log('allmsghno', this.submsgno)
        this.submsgnoB = [...new Set(this.submsgnoB.map(item => item.submsgno))].map((unique) => {
          return {
            value: unique,
            text: unique
          };
        });
        this.submsgnoB.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async selectkeyseqA() {
      await axios.get('http://localhost:4000/allkeyseq').then(response => {
        console.log('keysq=====', response.data.result)
        this.keyseqA = response.data.result
        this.keyseqA = this.keyseqA.map((data, i) => {
          return {
            value: data,
            text: data
          }
        })
        this.keyseqA.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async selectkeyseqB() {
      await axios.get('http://localhost:4000/allkeyseq').then(response => {
        console.log('keysq=====', response.data.result)
        this.keyseqB = response.data.result
        this.keyseqB = this.keyseqB.map((data, i) => {
          return {
            value: data,
            text: data
          }
        })
        this.keyseqB.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    concatStringA(value1, value2) {
      // ตรวจว่าถ้ายังไม่มี array ก็สร้างใหม่
      if (!Array.isArray(this.stringA)) {
        this.stringA = [];
      }

      // เพิ่ม object เข้า array เดิม
      this.stringA.push({
        submsg: value1,
        part_name: value2
      });

      console.log('stringA รวมทั้งหมด:', this.stringA);
    },
    concatStringB(value1, value2) {
      // ตรวจว่าถ้ายังไม่มี array ก็สร้างใหม่
      if (!Array.isArray(this.stringB)) {
        this.stringB = [];
      }

      // เพิ่ม object เข้า array เดิม
      this.stringB.push({
        submsg: value1,
        part_name: value2
      });

      console.log('stringA รวมทั้งหมด:', this.stringB);
    },
  }
}
</script>