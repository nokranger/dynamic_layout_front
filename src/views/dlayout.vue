<template>
  <div>
    <b-container>
      <div
        style="border: 2px solid gray;border-radius: 10px;box-shadow: 5px 5px 5px #888888;margin: 20px;width: 100%;height: 1200px">
        <b-row>
          <b-col>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'top' + index"
                v-b-modal="'modal-shelf-top' + index" v-on:click="showdetail('div-shelf-top' + index)"></div>
              <b-modal :id="'modal-shelf-top' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
                  style="margin: 10px;">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('top' + index, 'modal-shelf-top' + index, 'div-shelf-top' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <br>
            <br>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'B' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'topp' + index"
                v-b-modal="'modal-shelf-topp' + index" v-on:click="showdetail('div-shelf-topp' + index)"></div>
              <b-modal :id="'modal-shelf-topp' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'BB' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('topp' + index, 'modal-shelf-topp' + index, 'div-shelf-topp' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('topp' + index, 'modal-shelf-topp' + index, 'div-shelf-topp' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('topp' + index, 'modal-shelf-topp' + index, 'div-shelf-topp' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'C' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'toppp' + index"
                v-b-modal="'modal-shelf-toppp' + index" v-on:click="showdetail('div-shelf-toppp' + index)"></div>
              <b-modal :id="'modal-shelf-toppp' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'CC' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('toppp' + index, 'modal-shelf-toppp' + index, 'div-shelf-toppp' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('toppp' + index, 'modal-shelf-toppp' + index, 'div-shelf-toppp' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('toppp' + index, 'modal-shelf-toppp' + index, 'div-shelf-toppp' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <br>
            <br>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'D' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'topppp' + index"
                v-b-modal="'modal-shelf-topppp' + index" v-on:click="showdetail('div-shelf-topppp' + index)"></div>
              <b-modal :id="'modal-shelf-topppp' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'DD' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('topppp' + index, 'modal-shelf-topppp' + index, 'div-shelf-topppp' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('topppp' + index, 'modal-shelf-topppp' + index, 'div-shelf-topppp' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('topppp' + index, 'modal-shelf-topppp' + index, 'div-shelf-topppp' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'E' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'toppppp' + index"
                v-b-modal="'modal-shelf-toppppp' + index" v-on:click="showdetail('div-shelf-toppppp' + index)"></div>
              <b-modal :id="'modal-shelf-toppppp' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'EE' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('toppppp' + index, 'modal-shelf-toppppp' + index, 'div-shelf-toppppp' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('toppppp' + index, 'modal-shelf-toppppp' + index, 'div-shelf-toppppp' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('toppppp' + index, 'modal-shelf-toppppp' + index, 'div-shelf-toppppp' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
          </b-col>
          <b-col>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'F' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'topr' + index"
                v-b-modal="'modal-shelf-topr' + index" v-on:click="showdetail('div-shelf-topr' + index)"></div>
              <b-modal :id="'modal-shelf-topr' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'FF' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('topr' + index, 'modal-shelf-topr' + index, 'div-shelf-topr' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('topr' + index, 'modal-shelf-topr' + index, 'div-shelf-topr' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('topr' + index, 'modal-shelf-topr' + index, 'div-shelf-topr' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'G' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'toprr' + index"
                v-b-modal="'modal-shelf-toprr' + index" v-on:click="showdetail('div-shelf-toprr' + index)"></div>
              <b-modal :id="'modal-shelf-toprr' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'GG' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('toprr' + index, 'modal-shelf-toprr' + index, 'div-shelf-toprr' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('toprr' + index, 'modal-shelf-toprr' + index, 'div-shelf-toprr' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('toprr' + index, 'modal-shelf-toprr' + index, 'div-shelf-toprr' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'H' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'toprrr' + index"
                v-b-modal="'modal-shelf-toprrr' + index" v-on:click="showdetail('div-shelf-toprrr' + index)"></div>
              <b-modal :id="'modal-shelf-toprrr' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'HH' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('toprrr' + index, 'modal-shelf-toprrr' + index, 'div-shelf-toprrr' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('toprrr' + index, 'modal-shelf-toprrr' + index, 'div-shelf-toprrr' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('toprrr' + index, 'modal-shelf-toprrr' + index, 'div-shelf-toprrr' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 4" :key="'I' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'toprrrr' + index"
                v-b-modal="'modal-shelf-toprrrr' + index" v-on:click="showdetail('div-shelf-toprrrr' + index)"></div>
              <b-modal :id="'modal-shelf-toprrrr' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'II' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('toprrrr' + index, 'modal-shelf-toprrrr' + index, 'div-shelf-toprrrr' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('toprrrr' + index, 'modal-shelf-toprrrr' + index, 'div-shelf-toprrrr' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('toprrrr' + index, 'modal-shelf-toprrrr' + index, 'div-shelf-toprrrr' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <br>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 2" :key="'J' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 10px;" :id="'toprrrrr' + index"
                v-b-modal="'modal-shelf-toprrrrr' + index" v-on:click="showdetail('div-shelf-toprrrrr' + index)"></div>
              <b-modal :id="'modal-shelf-toprrrrr' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'JJ' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('toprrrrr' + index, 'modal-shelf-toprrrrr' + index, 'div-shelf-toprrrrr' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('toprrrrr' + index, 'modal-shelf-toprrrrr' + index, 'div-shelf-toprrrrr' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('toprrrrr' + index, 'modal-shelf-toprrrrr' + index, 'div-shelf-toprrrrr' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 2" :key="index">
              <div style="width: 70px;height: 70px" :id="'top' + index"></div>
            </div>
          </b-col>
        </b-row>
        <b-row>
          <b-col>
            <div align="left" style="display: inline-block;" v-for="(top, index) in 10" :key="'K' + index">
              <div style="border: 2px solid gray;width: 70px;height: 70px;margin-top: 70px;" :id="'bot' + index"
                v-b-modal="'modal-shelf-bot' + index" v-on:click="showdetail('div-shelf-bot' + index)"></div>
              <b-modal :id="'modal-shelf-bot' + index" size="xl" :title="'โลเคชั่น' + index" hide-footer>
                <div style="font-weight: bold;">
                  Name Station
                </div>
                <div>
                  <b-form-select v-model="selecteds" :options="stationO"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Model
                </div>
                <div>
                  <b-form-select v-model="selectedm" :options="model" v-on:change="selectsubmsgno()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Sub message No
                </div>
                <div>
                  <b-form-select v-model="selectedmsg" :options="submsgno"
                    v-on:change="selectconversion()"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Conversion Character
                </div>
                <div>
                  <b-form-select v-model="selectedc" :options="conversion"></b-form-select>
                </div>
                <br>
                <div style="font-weight: bold;">
                  Priority
                </div>
                <div>
                  <b-input style="width: 30%;" v-model="priority"></b-input>
                </div>
                <br>
                <div v-for="(databox, index) in showboxdetail2" :key="'KK' + index">
                  <div style="font-weight: bold;">Name Station: <label style="font-weight: normal;">{{ databox.name }}</label></div>
                  <div style="font-weight: bold;">Model: <label style="font-weight: normal;">{{ databox.model }}</label></div>
                  <div style="font-weight: bold;">Sub Message: <label style="font-weight: normal;">{{ databox.submsg }}</label></div>
                  <div style="font-weight: bold;">Conversion Character: <label style="font-weight: normal;">{{ databox.conversion_char }}</label></div>
                </div>
                <br>
                <b-button variant="outline-primary"
                  v-on:click="addBoxstation('bot' + index, 'modal-shelf-bot' + index, 'div-shelf-bot' + index)">Add</b-button>
                <b-button variant="outline-warning"
                  v-on:click="editBoxstation('bot' + index, 'modal-shelf-bot' + index, 'div-shelf-bot' + index)"
                  style="margin: 10px;">Edit</b-button>
                <b-button variant="outline-danger"
                  v-on:click="delBoxstation('bot' + index, 'modal-shelf-bot' + index, 'div-shelf-bot' + index)"
                  style="margin: 10px;">Delete</b-button>
              </b-modal>
            </div>
          </b-col>
        </b-row>
        <br>
      </div>
    </b-container>
  </div>
</template>
<script>
import * as XLSX from 'xlsx';
import axios from 'axios';
export default {
  data() {
    return {
      alertStatus: 0,
      dismissSecs: 5,
      dismissCountDown: 0,
      showDismissibleAlert: false,
      alldatabiasbc: '',
      model: [],
      selectedm: null,
      submsgno: [],
      selectedmsg: null,
      conversion: [],
      selectedc: null,
      lmodel: '',
      lwork: '',
      llayout: '',
      llevel: 0,
      bc: '',
      conversion: '',
      boxstation: [],
      location: '',
      selecteds: null,
      stationO: [],
      showboxdetail: '',
      sname_work: '',
      smodel: '',
      ssubmsg: '',
      sconversion_char: '',
      showboxdetail2: '',
      showbox: '',
      priority: '',
    }
  },
  async mounted() {
    await axios.get('http://localhost:4000/allstation')
      .then(response => {
        this.stationO = response.data.result
        this.stationO = response.data.result.map((data, i) => {
          return {
            value: data.station_id,
            text: data.name
          }
        })
        this.stationO.push({ "value": null, "text": "Please select an option" })
        // console.log('detail=====', this.stationO)
      })
      .catch(error => {
        console.error('Error fetching data:', error.message);
      });
    await axios.get('http://localhost:4000/alldiasbc2').then(response => {
      console.log(response.data);
      // this.options = response.data.result
      // this.model = response.data.result
      this.model = response.data.result.map((data, i) => {
        return {
          value: data.model,
          text: data.model
        }
      })
      this.model.push({ "value": null, "text": "Please select an option" })
      // this.model.push({ "value": null, "text": "Please select an option" })
    }).catch(error => {
      console.error('Error fetching data:', error.message);
    });
    await axios.get('http://localhost:4000/allboxstation3').then(response => {
      // console.log('getid', response.data.result);
      // console.log('getidModal', document.getElementById('modal-shelf-top1'));
      this.showboxdetail = response.data.result
      this.showboxdetail = this.showboxdetail.filter((i) => {
        let divboxid = document.getElementById(i.layout_location)
        divboxid.style.backgroundColor = 'lightblue'
        return i.layout_location == document.getElementById(i.layout_location).id
      })
      // console.log('getid=====', this.showboxdetail);
    }).catch(error => {
      console.error('Error fetching data:', error.message);
    });
  },
  methods: {
    addBoxstation(id1, id2, id3) {
      this.boxstation = {
        name: '',
        station_id: this.selecteds,
        name_work: this.selecteds,
        model: this.selectedm,
        submsg: this.selectedmsg,
        conversion_char: this.selectedc,
        layout_location: id1,
        show_detail: id3,
        priority: this.priority
      }
      // console.log('locationBox=========', this.boxstation)
      axios.post('http://localhost:4000/boxstation', this.boxstation)
        .then(response => {
          this.location = response.data.result
          this.$bvModal.hide(id2)
          // console.log('detail', this.location)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    async showdetail(id) {
      this.showbox = {
        show_detail: id
      }
      await axios.post('http://localhost:4000/allboxstation4', this.showbox).then(response => {
        this.showboxdetail2 = response.data.result
        // console.log('getid=====2', this.showboxdetail2);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async editBoxstation(id1, id2, id3) {
      this.boxstation = {
        name: '',
        station_id: this.selecteds,
        name_work: this.selecteds,
        model: this.selectedm,
        submsg: this.selectedmsg,
        conversion_char: this.selectedc,
        layout_location: id1,
        show_detail: id3,
        priority: this.priority
      }
      await axios.post('http://localhost:4000/editboxstation', this.boxstation).then(response => {
        console.log('Edit');
        this.showboxdetail2 = response.data.result
        this.$bvModal.hide(id2)
        // console.log('getid=====Edit', this.showboxdetail2);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async delBoxstation(id1, id2, id3) {
      this.showbox = {
        layout_location: id1
      }
      await axios.post('http://localhost:4000/deleteboxstation', this.showbox).then(response => {
        console.log('Deleted');
        this.$bvModal.hide(id2)
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async selectsubmsgno() {
      await axios.get('http://localhost:4000/allconversion').then(response => {
        // console.log(response.data);
        // this.options = response.data.result
        this.submsgno = response.data.result
        console.log('mmm====', this.selectedm)
        this.submsgno = this.submsgno.filter((i) => {
          console.log('ccc====', i.model)
          // console.log('ccc====', i.submsgno)
          return i.model == this.selectedm
        })
        // console.log('allmsghno', this.submsgno)
        this.submsgno = this.submsgno.map((data, i) => {
          return {
            value: data.submsgno,
            text: data.submsgno
          }
        })
        this.submsgno.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async selectconversion() {
      await axios.get('http://localhost:4000/allconversion').then(response => {
        console.log(response.data);
        // this.options = response.data.result
        this.conversion = response.data.result
        this.conversion = this.conversion.filter((i) => {
          return i.submsgno == this.selectedmsg
        })
        this.conversion = this.conversion.map((data, i) => {
          return {
            value: data.conversioncharacter,
            text: data.conversioncharacter
          }
        })
        this.conversion.push({ "value": null, "text": "Please select an option" })
        // this.model.push({ "value": null, "text": "Please select an option" })
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    },
    async loadLayout(lworks) {
      // console.log('loadLayout', lworks)
      await axios.get('http://localhost:4000/alllayout')
        .then(response => {
          this.llayout = response.data.result
          this.llayout = this.llayout.filter((i) => {
            return i.station_id == lworks.station_id
          })
          // console.log('detail', typeof (this.llayout[0].layout_level))
          this.llevel = Number(this.llayout[0].layout_level)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    edittable(items) {
      // console.log('tei=======', items)
      let mydiv = document.getElementById('top4')
      for (let i = 0; i < 4; i++) {
        let mydiv = document.getElementById('top' + i)
        mydiv.innerHTML = 'a' + i

        // console.log('fdasfas======', mydiv)
      }
      let aa = 'top4'
      if (aa == mydiv.id) {
        mydiv.style.backgroundColor = 'lightblue'
      }
      // document.getElementById('top0') = 'ASAFASFSFSF'
    },
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown
    }
  }
}
</script>