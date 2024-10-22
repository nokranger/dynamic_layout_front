<template>
  <div>
    Setting page
    <div>
      <b-container>
        <b-row>
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
        </b-row>
        <br>
        <b-row>
          <b-col>
            <b-button>Add</b-button>
            <b-button>Copy to</b-button>
            <b-button>Exit</b-button>
          </b-col>
        </b-row>
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
        { age: 40, first_name: 'Dickerson', last_name: 'Macdonald' },
        { age: 21, first_name: 'Larsen', last_name: 'Shaw' },
        { age: 89, first_name: 'Geneva', last_name: 'Wilson' },
        { age: 38, first_name: 'Jami', last_name: 'Carney' }
      ]
    }
  }
}
</script>