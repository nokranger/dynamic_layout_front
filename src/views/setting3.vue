<template>
  <div>
    <h1>
      Setting page Layout
    </h1>
    <div>
      <b-container>
        <b-row style="text-align: left;">
          <b-col></b-col>
          <b-col>
            <div>
              <div>
                Layout Level
              </div>
              <div>
                <b-input placeholder="Type Layout Level" v-model="layout_level"></b-input>
              </div>
            </div>
            <div>
              <div>
                Layout Part number
              </div>
              <div>
                <b-input placeholder="Type Layout Part number" v-model="layout_part_number"></b-input>
              </div>
            </div>
            <div>
              <div>
                Layout Quantity
              </div>
              <div>
                <b-input placeholder="Type Layout Quantity" v-model="layout_quantity"></b-input>
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
        <!-- <b-row>
          <b-table striped hover :items="items"></b-table>
        </b-row> -->
      </b-container>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      layout: [],
      layout_level: '',
      layout_part_number: '',
      layout_quantity: ''
    }
  },
  methods: {
    addSetting() {
      this.layout = {
        layout_level: this.layout_level,
        layout_part_number: this.layout_part_number,
        layout_quantity: this.layout_quantity
      }
      console.log('setting', this.setting)
      axios.post('http://localhost:4000/settinglayout', this.layout).then(response => {
        console.log(response.data);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    }
  }
}
</script>