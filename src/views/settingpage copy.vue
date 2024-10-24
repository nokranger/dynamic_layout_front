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
            <div>
              <div>
                track sign
              </div>
              <div>
                <b-select>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                </b-select>
              </div>
            </div>
            <div>
              <div>
                ID Serie
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div>
            <div>
              <div>
                Series Name
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div>
            <div>
              <div>
                Format
              </div>
              <div>
                <b-select>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                </b-select>
              </div>
            </div>
            <div>
              <div>
                Conversion
              </div>
              <div>
                <b-select>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                </b-select>
              </div>
            </div>
            <div>
              <div>
                Order
              </div>
              <div>
                <b-select>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                  <option value="1">1</option>
                </b-select>
              </div>
            </div>
            <div>
              <div>
                Model
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div>
            <div>
              <div>
                Version
              </div>
              <div>
                <b-input></b-input>
              </div>
            </div>
          </b-col>
          <b-col></b-col>
        </b-row>
        <br>
        <b-row>
          <b-col>
            <b-button variant="outline-primary" style="margin: 5px;">Add</b-button>
            <b-button variant="outline-danger" style="margin: 5px;">Copy to</b-button>
            <b-button variant="outline-info" style="margin: 5px;">Exit</b-button>
          </b-col>
        </b-row>
        <br>
        <b-row>
          <b-table striped hover :items="items"></b-table>
        </b-row>
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