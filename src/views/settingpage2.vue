<template>
  <div>
    <h1>
      Setting page Working Station
    </h1>
    <div>
      <b-container>
        <b-row style="text-align: left;">
          <b-col></b-col>
          <b-col>
            <div>
              <div>
                Name Station
              </div>
              <div>
                <div>
                  <b-input v-model="nameStation" placeholder="Type Your Name Station"></b-input>
                </div>
              </div>
            </div>
            <div>
              <div>
                Picking Sequence
              </div>
              <div>
                <div>
                  <b-form-select v-model="selected" :options="options"></b-form-select>
                </div>
              </div>
            </div>
            <div>
              <div>
                Lot Size
              </div>
              <div>
                <div>
                  <b-form-select v-model="selected2" :options="options2"></b-form-select>
                </div>
              </div>
            </div>
            <div>
              <div>
                Condition
              </div>
              <div>
                <div>
                  <b-form-select v-model="selected3" :options="options3"></b-form-select>
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
      nameStation: ''
    }
  },
  methods: {
    addSetting () {
      this.setting = {
        namestation: this.nameStation,
        picking: this.selected,
        lot_size: this.selected2,
        condition: this.selected3
      }
      console.log('setting', this.setting)
      axios.post('http://localhost:4000/settingworking', this.setting).then(response => {
          console.log(response.data);
        }).catch(error => {
          console.error('Error fetching data:', error.message);
        });
    }
  }
}
</script>