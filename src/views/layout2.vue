<template>
  <div>
    <b-container>
      <b-row>
        <b-col>
          <b-row>
            <b-table striped hover :items="items"></b-table>
          </b-row>
        </b-col>
        <b-col>
          <b-row>
            <div>
              เลือกรูปที่ตรงกับชิ้นส่วน
            </div>
            <div>
              <b-row>
                <b-col>
                  <div>
                    <img src="https://i.imgur.com/qx2zdy1.jpeg" style="width: 100%;">
                  </div>
                  <b-input></b-input>
                </b-col>
                <b-col>
                  <div>
                    <img src="https://i.imgur.com/qx2zdy1.jpeg" style="width: 100%;">
                  </div>
                  <b-input></b-input>
                </b-col>
              </b-row>
            </div>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
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