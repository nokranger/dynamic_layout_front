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
                <div>
                  <b-form-select v-model="selectmsgA" :options="submsgnoA" v-on:change="concatStringA(selectmsgA)"></b-form-select>
                </div>
              </div>
              <br>
              <div>
                <div>
                  <b-input v-model="stringA" placeholder="Type Your Name Station"></b-input>
                </div>
              </div>
            </div>
            <br>
            <div>
              <div style="font-weight: bold;">
                B
              </div>
              <div>
                <div>
                  <b-form-select v-model="selectmsgB" :options="submsgnoB" v-on:change="concatStringB(selectmsgB)"></b-form-select>
                </div>
              </div>
              <br>
              <div>
                <div>
                  <b-input v-model="stringB" placeholder="Type Your Name Station"></b-input>
                </div>
              </div>
            </div>
          </b-col>
          <b-col></b-col>
        </b-row>
        <br>
        <b-row>
          <b-col>
            <b-button variant="outline-primary" style="margin: 5px;" v-on:click="addSetting()">Save Setting</b-button>
          </b-col>
        </b-row>
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
      stringB: ""
    }
  },
  async mounted() {
    await this.selectsubmsgnoA()
    await this.selectsubmsgnoB()
  },
  methods: {
    addSetting() {
      this.setting = {
        namestation: this.nameStation,
        picking: this.selected,
        lot_size: this.selected2,
        condition: this.selected3,
        sa: this.stringA,
        sb: this.stringB
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
        this.submsgnoA = this.submsgnoA.map((data, i) => {
          return {
            value: data.submsgno,
            text: data.submsgno
          }
        })
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
        this.submsgnoB = this.submsgnoB.map((data, i) => {
          return {
            value: data.submsgno,
            text: data.submsgno
          }
        })
        this.submsgnoB.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    concatWithComma(existingString, newString) {
      if (existingString === "" || existingString === null || existingString === undefined) {
        return newString;
      }
      return existingString + ", " + newString;
    },
    concatStringA (value) {
      this.stringA = this.concatWithComma(this.stringA, value)
    },
    concatStringB (value) {
      this.stringB = this.concatWithComma(this.stringB, value)
    }
  }
}
</script>