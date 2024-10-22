<template>
  <div>
    <div>Seq Input</div>
    <div>
      <b-container>
        <div>
          <b-row>
            <b-col></b-col>
            <b-col>
              <b-input style="width: 100%;" v-model="seqnum"></b-input>
            </b-col>
            <b-col></b-col>
          </b-row>
        </div>
        <br>
        <div>
          <b-row>
            <b-col></b-col>
            <b-col>
              <b-button class="button-number" variant="outline-primary" v-on:click="addnum(1)">1</b-button><b-button class="button-number"
                variant="outline-primary"  v-on:click="addnum(2)">2</b-button><b-button class="button-number"
                variant="outline-primary"  v-on:click="addnum(3)">3</b-button>
            </b-col>
            <b-col></b-col>
          </b-row>
          <b-row>
            <b-col></b-col>
            <b-col>
              <b-button class="button-number" variant="outline-primary"  v-on:click="addnum(4)">4</b-button><b-button class="button-number"
                variant="outline-primary"  v-on:click="addnum(5)">5</b-button><b-button class="button-number"
                variant="outline-primary"  v-on:click="addnum(6)">6</b-button>
            </b-col>
            <b-col></b-col>
          </b-row>
          <b-row>
            <b-col></b-col>
            <b-col>
              <b-button class="button-number" variant="outline-primary"  v-on:click="addnum(7)">7</b-button><b-button class="button-number"
                variant="outline-primary"  v-on:click="addnum(8)">8</b-button><b-button class="button-number"
                variant="outline-primary"  v-on:click="addnum(9)">9</b-button>
            </b-col>
            <b-col></b-col>
          </b-row>
          <b-row>
            <b-col></b-col>
            <b-col>
              <b-button class="button-number">-</b-button><b-button class="button-number"
                variant="outline-primary"  v-on:click="addnum(0)">0</b-button><b-button class="button-number">-</b-button>
            </b-col>
            <b-col></b-col>
          </b-row>
        </div>
        <br>
        <div>Select Process</div>
        <div>
          <b-row>
            <b-col></b-col>
            <b-col>
              <b-input style="width: 100%;"></b-input>
            </b-col>
            <b-col></b-col>
          </b-row>
        </div>
        <br>
        <div>
          <b-button variant="outline-primary" style="width: max;">Enter</b-button>
        </div>
      </b-container>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      items: [
        { age: 40, first_name: 'Dickerson', last_name: 'Macdonald' },
        { age: 21, first_name: 'Larsen', last_name: 'Shaw' },
        { age: 89, first_name: 'Geneva', last_name: 'Wilson' },
        { age: 38, first_name: 'Jami', last_name: 'Carney' }
      ],
      seqnum: ''
    }
  },
  methods: {
    addnum (num) {
      this.seqnum = this.seqnum + num
    }
  }
}
</script>
<style>
.button-number {
  width: 50px;
  margin: 5px;
}
</style>