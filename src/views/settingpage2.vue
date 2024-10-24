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
                Picking Sequence
              </div>
              <div>
                <select name="cars" id="cars">
                  <option value="volvo">Ascending</option>
                  <option value="saab">Descending</option>
                  <option value="opel">Shoping</option>
                  <option value="audi">Audi</option>
                </select>
              </div>
            </div>
            <div>
              <div>
                Lot Size
              </div>
              <div>
                <select name="cars" id="cars">
                  <option value="volvo">3</option>
                  <option value="saab">6</option>
                  <option value="opel">9</option>
                  <option value="audi">12</option>
                </select>
              </div>
            </div>
            <div>
              <div>
                Condition
              </div>
              <div>
                <select name="cars" id="cars">
                  <option value="volvo">Single</option>
                  <option value="saab">Parting</option>
                </select>
              </div>
            </div>
            <div>
              <div>
                Layout Level
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div>
            <div>
              <div>
                Layout Part number
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div>
            <div>
              <div>
                Layout Quantity
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div>
            <div>
              <div>
                Setting Part for picture
              </div>
              <div>
                <b-input placeholder="Setting picture part on conversion C:/E-telemail/Main Picture"></b-input>
              </div>
            </div>
            <!-- <div>
              <div>
                Version
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div> -->
          </b-col>
          <b-col></b-col>
        </b-row>
        <br>
        <b-row>
          <b-col>
            <b-button variant="outline-primary" style="margin: 5px;">Save Setting</b-button>
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
export default {
  data() {
    return {
      items: [
        { TEST: 1, test: '', testt: '' },
        { TEST: 2, test: '', testt: '' },
        { TEST: 3, test: '', testt: '' },
        { TEST: 4, test: '', testt: '' }
      ]
    }
  }
}
</script>