<template>
  <div>
    <b-container>
      <div
        style="border: 2px solid gray;border-radius: 10px;box-shadow: 5px 5px 5px #888888;margin: 20px;width: 100%;height: 1200px">
        <b-row>
          <b-col>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'top' + index"
                v-b-modal="'modal-shelf-top' + index" v-on:click="showdetail()"></div>
              <b-modal :id="'modal-shelf-top' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <!-- name: '',
                name_work: '',
                model: '',
                subsmg: '',
                conversion_char: '',
                desc: '',
                layout_location: '' -->
                <div>
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <div>
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <div>
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <div>
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div :id="'div-shelf-top' + index">
                  <div>Name Station: {{ sname_work }}</div>
                  <div>Model: {{ smodel }}</div>
                  <div>Sub Message: {{ ssubmsg }}</div>
                  <div>Conversion Character: {{ sconversion_char }}</div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)">Add</b-button>
              </b-modal>
            </div>
            <br>
            <br>
            <br>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top2, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;" :id="'top2' + index"
                v-on:click="edittable('top2' + index)"></div>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top3, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;" :id="'top3' + index"
                v-on:click="edittable('top3' + index)"></div>
            </div>
            <br>
            <br>
            <br>
            <br>
            <div align="left" style="display: inline-block;" v-for="(bot, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;" :id="'bot' + index"
                v-on:click="edittable('bot' + index)"></div>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(bot2, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;" :id="'bot2' + index"
                v-on:click="edittable('bot2' + index)"></div>
            </div>
          </b-col>
          <b-col>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'top' + index"
                v-on:click="edittable('top' + index)"></div>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px" :id="'top' + index"
                v-on:click="edittable('top' + index)"></div>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;" :id="'top' + index"
                v-on:click="edittable('top' + index)"></div>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px" :id="'top' + index"
                v-on:click="edittable('top' + index)"></div>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 2" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px" :id="'top' + index"
                v-on:click="edittable('top' + index)"></div>
            </div>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 2" :key="index">
              <div style="width: 70px;height: 70px" :id="'top' + index"></div>
            </div>
          </b-col>
        </b-row>
        <b-row>
          <b-col>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 10" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 70px;" :id="'top' + index"
                v-on:click="edittable('top' + index)"></div>
            </div>
          </b-col>
        </b-row>
        <br>
      </div>
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
      conversion: '',
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
    }
  },
  async mounted() {
    await axios.get('http://localhost:4000/allstation')
      .then(response => {
        this.stationO = response.data.result.map((data, i) => {
          return {
            value: data.name,
            text: data.name
          }
        })
        this.stationO.push({ "value": null, "text": "Please select an option" })
        console.log('detail', this.location)
      })
      .catch(error => {
        console.error('Error fetching data:', error.message);
      });
    await axios.get('http://localhost:4000/alldiasbc2').then(response => {
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
    await axios.get('http://localhost:4000/allboxstation').then(response => {
      console.log('getid', response.data.result);
      // console.log('getidModal', document.getElementById('modal-shelf-top1'));
      this.showboxdetail = response.data.result
      this.showboxdetail = this.showboxdetail.filter((i) => {
        console.log('fasfasf', i.show_detail)
        console.log('fasfasfsssss', document.getElementById(i.show_detail))
        // console.log('fasfasf', document.getElementById(i.show_detail).id)
        let divboxid = document.getElementById(i.layout_location)
        divboxid.style.backgroundColor = 'lightblue'
        // let detailboxid = document.getElementById(i.show_detail)
        // detailboxid.style.backgroundColor = 'lightblue'
        // detailboxid.innerHTML = `<br><div>Name Station: {{ sname_work }}</div><br>
        //           <div>Model: {{ sModel }}</div><br>
        //           <div>Sub Message: {{ ssubmsg }}</div><br>
        //           <div>Conversion Character: {{ sconversion_char }}</div>`
        return i.layout_location == document.getElementById(i.layout_location).id
      })
      console.log('getid=====', this.showboxdetail);
    }).catch(error => {
      console.error('Error fetching data:', error.message);
    });
  },
  methods: {
    addBoxstation(id1, id2, id3) {
      this.boxstation = {
        name: '',
        // station_id: BIconHandThumbsDown,
        name_work: this.selecteds,
        model: this.selectedm,
        submsg: this.selectedmsg,
        conversion_char: this.selectedc,
        layout_location: id1,
        show_detail: id3
      }
      console.log('locationBox=========', this.boxstation)
      axios.post('http://localhost:4000/boxstation', this.boxstation)
        .then(response => {
          this.location = response.data.result
          console.log('detail', this.location)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    async showdetail() {
      await axios.get('http://localhost:4000/allboxstation').then(response => {
        console.log('getid', response.data.result);
        // console.log('getidModal', document.getElementById('modal-shelf-top1'));
        this.showboxdetail2 = response.data.result
        this.showboxdetail2 = this.showboxdetail2.filter((i) => {
          // let detailboxid = document.getElementById(i.show_detail)
          // detailboxid.style.backgroundColor = 'lightblue'
          // console.log('conver====', i.conversion_char)
          // console.log('conver====S', i.show_detail)
          // console.log('conver====D', document.getElementById(i.show_detail).id)
          // if (i.show_detail == document.getElementById(i.show_detail).id) {
            this.sname_work = i.name_work,
            this.smodel = i.model,
            this.ssubmsg = i.submsg,
            this.sconversion_char = i.conversion_char
          // }
          return i.show_detail == document.getElementById(i.show_detail).id && i.layout_location == document.getElementById(i.layout_location).id
        })
        console.log('getid=====2', this.showboxdetail2);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async selectsubmsgno() {
      await axios.get('http://localhost:4000/allconversion').then(response => {
        console.log(response.data);
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
    async selectconversion() {
      await axios.get('http://localhost:4000/allconversion').then(response => {
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
    edittable(items) {
      console.log('tei=======', items)
      let mydiv = document.getElementById('top4')
      for (let i = 0; i < 4; i++) {
        let mydiv = document.getElementById('top' + i)
        mydiv.innerHTML = 'a' + i

        console.log('fdasfas======', mydiv)
      }
      let aa = 'top4'
      if (aa == mydiv.id) {
        mydiv.style.backgroundColor = 'lightblue'
      }
      // document.getElementById('top0') = 'ASAFASFSFSF'
    },
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown
    }
  }
}
</script>