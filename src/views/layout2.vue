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
              <h1>
                เลือกรูปที่ตรงกับชิ้นส่วน
              </h1>
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
        { No: 1, Code: 'เขียวขาว', Status: 'O' },
        { No: 2, Code: '', Status: '' },
        { No: 3, Code: '', Status: '' },
        { No: 4, Code: '', Status: '' },
        { No: 5, Code: '', Status: '' },
        { No: 6, Code: '', Status: '' }
      ]
    }
  }
}
</script>