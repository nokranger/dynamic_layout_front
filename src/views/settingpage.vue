<template>
  <div>
    <h1>
      Setting page
    </h1>
    <div>
      <b-container>
        <b-row style="text-align: left;">
          <b-col></b-col>
          <b-col>
            <!-- <div>
              <div>
                track sign
              </div>
              <div>
                <b-form-select v-model="selected" :options="options"></b-form-select>
              </div>
            </div> -->
            <!-- <div>
              <div>
                ID Serie
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div> -->
            <!-- <div>
              <div>
                Series Name
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div> -->
            <!-- <div>
              <div>
                Format
              </div>
              <div>
                <b-form-select v-model="selected" :options="options"></b-form-select>
              </div>
            </div> -->
            <!-- <div>
              <div>
                Conversion
              </div>
              <div>
                <b-form-select v-model="selected" :options="options"></b-form-select>
              </div>
            </div> -->
            <!-- <div>
              <div>
                Order
              </div>
              <div>
                <b-form-select v-model="selected" :options="options"></b-form-select>
              </div>
            </div> -->
            <div>
              <div>
                BC Format
              </div>
              <div>
                <b-input v-model="model" placeholder="Type BC Format"></b-input>
              </div>
            </div>
            <div>
              <div>
                Conversion
              </div>
              <div>
                <b-input v-model="conversion" placeholder="Type Conversion"></b-input>
              </div>
            </div>
            <div>
              <div>
                Version
              </div>
              <div>
                <b-input v-model="mversion" placeholder="Type Version"></b-input>
              </div>
            </div>
          </b-col>
          <b-col></b-col>
        </b-row>
        <br>
        <b-row>
          <b-col>
            <b-button variant="outline-primary" style="margin: 5px;" v-on:click="addSetting()">Add</b-button>
            <!-- <b-button variant="outline-danger" style="margin: 5px;">Copy to</b-button>
            <b-button variant="outline-info" style="margin: 5px;">Exit</b-button> -->
          </b-col>
        </b-row>
        <br>
        <br>
        <b-row>
          <b-col></b-col>
          <b-col>
            <div>
              <b-form-select v-model="selected" :options="options"></b-form-select>
            </div>
          </b-col>
          <b-col>
            <div style="text-align: left;">
              <b-button v-on:click="loadModel (selected)" variant="outline-success">Load Model</b-button>
            </div>
          </b-col>
        </b-row>
        <br>
        <br>
        <b-row>
          <div style="text-align: left;">
              <b-table hover :items="items" :head-variant="headVariant" :bordered="true" fixed="fixed" :sticky-header="stickyHeader"></b-table>
          </div>
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
      data: [],
      model: '',
      conversion: '',
      mversion: '',
      selected: null,
      options: [],
      fields: [],
      items: [],
      headVariant: 'dark',
      stickyHeader: true
    }
  },
  mounted() {
    axios.get('http://localhost:4000/allmodel', this.layout).then(response => {
      console.log(response.data);
      // this.options = response.data.result
      this.options = response.data.result.map((data, i) => {
        return {
          value: data.model,
          text: data.model + ' ' + data.version
        }
      })
      this.options.push({ "value": null, "text": "Please select an option" })
    }).catch(error => {
      console.error('Error fetching data:', error.message);
    });
  },
  methods: {
    addSetting() {
      // let result = this.options.find(obj => obj.value === this.selected);
      // console.log(result)
      this.data = {
        model: this.model,
        conversion: this.conversion,
        mversion: this.mversion
      }
      console.log('setting', this.data)
      axios.post('http://localhost:4000/settingmodel', this.data).then(response => {
        console.log(response.data);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    loadModel (select) {
      let data = {
        model: select
      }
      console.log('Load Model', select)
      axios.post('http://localhost:4000/alldiasbc2', data).then(response => {
        console.log(response.data);
        this.items = response.data.result
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    }
  }
}
</script>