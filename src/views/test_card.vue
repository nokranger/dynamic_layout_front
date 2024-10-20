<template>
  <div>
    <b-container>
      <b-row>
        <b-col>
          <div>
            Name: <b-input v-model="shelf1.name"></b-input>
          </div>
        </b-col>
        <b-col><b-form-select v-model="selectOption" :options="options"></b-form-select></b-col>
        <b-col></b-col>
      </b-row>
      <b-row>
        <b-col>
          <div>
            Serial Number: <b-input v-model="shelf1.serial_number"></b-input>
          </div>
        </b-col>
        <b-col></b-col>
        <b-col></b-col>
      </b-row>
      <b-row>
        <b-col>
          <div>
            Price: <b-input v-model="shelf1.price"></b-input>
          </div>
        </b-col>
        <b-col></b-col>
        <b-col></b-col>
      </b-row>
      <b-row>
        <b-col>
          <div>
            Quantity: <b-input v-model="shelf1.quantity"></b-input>
          </div>
        </b-col>
        <b-col></b-col>
        <b-col></b-col>
      </b-row>
    </b-container>

    <!-- <div>
      shelf
    </div> -->
    <div>
      <b-button variant="outline-primary" v-on:click="addShelf(selectOption)">Add item</b-button>
    </div>
    <div style="border: 2px solid gray;border-radius: 10px;box-shadow: 5px 5px 5px #888888;margin: 20px;">
      <div
        style="display: inline-block;border: 2px solid gray;border-radius: 10px;height: 450px;width: 200px;box-shadow: 5px 5px 5px #888888;margin: 10px;"
        v-b-modal.modal-shelf1>
        <div>ตู้ที่ 1</div>
        <div>
          Quantity: {{ shelf1_quantity }}
        </div>
        <b-modal id="modal-shelf1" size="xl" title="ข้อมูลตู้สินค้าที่ 1" hide-footer>
          <tr style="border: 1px solid;">
            <td style="border: 1px solid;">NO.</td>
            <td style="border: 1px solid;">Name</td>
            <td style="border: 1px solid;">Serial Number</td>
            <td style="border: 1px solid;">Price</td>
            <td style="border: 1px solid;">Quantity</td>
          </tr>
          <tr v-for="(item, index) in shelf1" :key="index" style="border: 1px solid;">
            <td style="border: 1px solid;">{{ index + 1 }}</td>
            <td style="border: 1px solid;">{{ item.name }}</td>
            <td style="border: 1px solid;">{{ item.serial_number }}</td>
            <td style="border: 1px solid;">{{ item.price }}</td>
            <td style="border: 1px solid;">{{ item.quantity }}</td>
          </tr>
        </b-modal>
      </div>
      <div
        style="display: inline-block;border: 2px solid gray;border-radius: 10px;height: 450px;width: 200px;box-shadow: 5px 5px 5px #888888;margin: 10px;"
        v-b-modal.modal-shelf2>
        <div>ตู้ที่ 2</div>
        <div>
          Quantity: {{ shelf2_quantity }}
        </div>
        <b-modal id="modal-shelf2" size="xl" title="ข้อมูลตู้สินค้าที่ 2" hide-footer>
          <tr style="border: 1px solid;">
            <td style="border: 1px solid;">NO.</td>
            <td style="border: 1px solid;">Name</td>
            <td style="border: 1px solid;">Serial Number</td>
            <td style="border: 1px solid;">Price</td>
            <td style="border: 1px solid;">Quantity</td>
          </tr>
          <tr v-for="(item, index) in shelf2" :key="index" style="border: 1px solid;">
            <td style="border: 1px solid;">{{ index + 1 }}</td>
            <td style="border: 1px solid;">{{ item.name }}</td>
            <td style="border: 1px solid;">{{ item.serial_number }}</td>
            <td style="border: 1px solid;">{{ item.price }}</td>
            <td style="border: 1px solid;">{{ item.quantity }}</td>
          </tr>
        </b-modal>
      </div>
      <div
        style="display: inline-block;border: 2px solid gray;border-radius: 10px;height: 450px;width: 200px;box-shadow: 5px 5px 5px #888888;margin: 10px;"
        v-b-modal.modal-shelf3>
        <div>ตู้ที่ 3</div>
        <div>
          Quantity: {{ shelf3_quantity }}
        </div>
        <b-modal id="modal-shelf3" size="xl" title="ข้อมูลตู้สินค้าที่ 3" hide-footer>
          <tr style="border: 1px solid;">
            <td style="border: 1px solid;">NO.</td>
            <td style="border: 1px solid;">Name</td>
            <td style="border: 1px solid;">Serial Number</td>
            <td style="border: 1px solid;">Price</td>
            <td style="border: 1px solid;">Quantity</td>
          </tr>
          <tr v-for="(item, index) in shelf3" :key="index" style="border: 1px solid;">
            <td style="border: 1px solid;">{{ index + 1 }}</td>
            <td style="border: 1px solid;">{{ item.name }}</td>
            <td style="border: 1px solid;">{{ item.serial_number }}</td>
            <td style="border: 1px solid;">{{ item.price }}</td>
            <td style="border: 1px solid;">{{ item.quantity }}</td>
          </tr>
        </b-modal>
      </div>
      <div
        style="display: inline-block;border: 2px solid gray;border-radius: 10px;height: 450px;width: 200px;box-shadow: 5px 5px 5px #888888;margin: 10px;"
        v-b-modal.modal-shelf4>
        <div>ตู้ที่ 4</div>
        <div>
          Quantity: {{ shelf4_quantity }}
        </div>
        <b-modal id="modal-shelf4" size="xl" title="ข้อมูลตู้สินค้าที่ 4" hide-footer>
          <tr style="border: 1px solid;">
            <td style="border: 1px solid;">NO.</td>
            <td style="border: 1px solid;">Name</td>
            <td style="border: 1px solid;">Serial Number</td>
            <td style="border: 1px solid;">Price</td>
            <td style="border: 1px solid;">Quantity</td>
          </tr>
          <tr v-for="(item, index) in shelf4" :key="index" style="border: 1px solid;">
            <td style="border: 1px solid;">{{ index + 1 }}</td>
            <td style="border: 1px solid;">{{ item.name }}</td>
            <td style="border: 1px solid;">{{ item.serial_number }}</td>
            <td style="border: 1px solid;">{{ item.price }}</td>
            <td style="border: 1px solid;">{{ item.quantity }}</td>
          </tr>
        </b-modal>
      </div>
    </div>

  </div>
</template>
<script>
export default {
  data() {
    return {
      status_shelf: 0,
      shelf1: [],
      shelf1_quantity: 0,
      shelf2: [],
      shelf2_quantity: 0,
      shelf3: [],
      shelf3_quantity: 0,
      shelf4: [],
      shelf4_quantity: 0,
      selectOption: null,
      options: [
        { value: null, text: 'เลือก' },
        { value: 1, text: 'ตู้ที่ 1' },
        { value: 2, text: 'ตู้ที่ 2' },
        { value: 3, text: 'ตู้ที่ 3' },
        { value: 4, text: 'ตู้ที่ 4' },
      ],
    }
  },
  mounted() {
  },
  methods: {
    addShelf(selectOption) {
      console.log('option: ', selectOption)
      if (selectOption == 1) {
        let shelfItem1 = {
        name: this.shelf1.name,
        serial_number: this.shelf1.serial_number,
        price: this.shelf1.price,
        quantity: this.shelf1.quantity,
        shelf: this.shelf1.shelf
      }
      this.shelf1.push(shelfItem1)
      this.shelf1_quantity = 0
      // this.shelf1_quantity = this.shelf1.reduce((acc, product) => acc + product.quantity, 0);
      let sumValue = this.shelf1.reduce((acc, obj) => acc + parseInt(obj.quantity), 0);
      this.shelf1_quantity = sumValue
      console.log('aaaa', this.shelf1)
      console.log('q', sumValue)
      } else if (selectOption == 2) {
        let shelfItem1 = {
        name: this.shelf1.name,
        serial_number: this.shelf1.serial_number,
        price: this.shelf1.price,
        quantity: this.shelf1.quantity,
        shelf: this.shelf1.shelf
      }
      this.shelf2.push(shelfItem1)
      this.shelf2_quantity = 0
      // this.shelf1_quantity = this.shelf1.reduce((acc, product) => acc + product.quantity, 0);
      let sumValue = this.shelf2.reduce((acc, obj) => acc + parseInt(obj.quantity), 0);
      this.shelf2_quantity = sumValue
      console.log('aaaa', this.shelf1)
      console.log('q', sumValue)
      } else if (selectOption == 3) {
        let shelfItem1 = {
        name: this.shelf1.name,
        serial_number: this.shelf1.serial_number,
        price: this.shelf1.price,
        quantity: this.shelf1.quantity,
        shelf: this.shelf1.shelf
      }
      this.shelf3.push(shelfItem1)
      this.shelf3_quantity = 0
      // this.shelf1_quantity = this.shelf1.reduce((acc, product) => acc + product.quantity, 0);
      let sumValue = this.shelf3.reduce((acc, obj) => acc + parseInt(obj.quantity), 0);
      this.shelf3_quantity = sumValue
      console.log('aaaa', this.shelf1)
      console.log('q', sumValue)
      } else if (selectOption == 4) {
        let shelfItem1 = {
        name: this.shelf1.name,
        serial_number: this.shelf1.serial_number,
        price: this.shelf1.price,
        quantity: this.shelf1.quantity,
        shelf: this.shelf1.shelf
      }
      this.shelf4.push(shelfItem1)
      this.shelf4_quantity = 0
      // this.shelf1_quantity = this.shelf1.reduce((acc, product) => acc + product.quantity, 0);
      let sumValue = this.shelf4.reduce((acc, obj) => acc + parseInt(obj.quantity), 0);
      this.shelf4_quantity = sumValue
      console.log('aaaa', this.shelf1)
      console.log('q', sumValue)
      }
    }
  },
}
</script>