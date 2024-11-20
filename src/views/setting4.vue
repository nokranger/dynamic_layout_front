<template>
  <div>
    <h1>
      Setting page Part
    </h1>
    <div>
      <b-container>
        <b-row style="text-align: left;">
          <b-col></b-col>
          <b-col>
            <div>
              <div style="font-weight: bold;">
                Setting Part for picture
              </div>
              <div>
                <b-input placeholder="Setting picture part on conversion C:/E-telemail/Main Picture" v-model="part_picture"></b-input>
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
      part: [],
      part_picture: ''
    }
  },
  methods: {
    addSetting() {
      this.part = {
        part_picture: this.part_picture
      }
      console.log('setting', this.setting)
      axios.post('http://localhost:4000/settingpart', this.part).then(response => {
        console.log(response.data);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    }
  }
}
</script>