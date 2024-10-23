<template>
  <div>
    <div>Upload ALC Excel file</div>
    <div style="font-weight: bold;font-size: 20px;margin: 10px;">
    </div>
    <input type="file" ref="fileInput" @change="handleFileChangeTnos" />
    <br>
    <br>
    <div>
      Download Data
    </div>
    <div>
      <button v-on:click="biasbc">Download Excel</button>
    </div>
    <br>
    <br>
    <br>
    <!-- <div>
      <button v-on:click="biasbc2">Detail Data</button>
    </div> -->
    <br>
  </div>

</template>
<script>
import * as XLSX from 'xlsx';
import axios from 'axios';
export default {
  data() {
    return {
      alldatabiasbc: ''
    }
  },
  methods: {
    handleFileChangeTnos(event) {
      console.log('tnos')
      const file = event.target.files[0];

      if (file) {
        this.readExcelTnos(file);
      }
    },
    async readExcelTnos(file) {
      const reader = new FileReader();

      reader.onload = (e) => {
        const data = e.target.result;
        const workbook = XLSX.read(data, { type: 'binary' });

        // Assume the first sheet is the one you want to read
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];

        // Convert the sheet data to JSON
        const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1 });

        // Set the JSON data to be displayed in the component
        this.jsonDataTnos = JSON.stringify(jsonData, null, 2);
        this.jsonDataTnos = JSON.parse(this.jsonDataTnos)
        this.jsonDataTnos.shift()
        let jsonobjectTnos = {}
        let jsonobject2Tnos = this.jsonDataTnos
        console.log('JSONTYPEOF2: ', jsonobjectTnos)
        jsonobjectTnos = jsonobject2Tnos.map(innerarray => {
          return innerarray.reduce((acc, item, index) => {
            acc[`item${index + 1}`] = item;
            return acc
          }, {})
        })
        jsonobjectTnos = jsonobjectTnos.filter((i) => {
          return i.item1 !== null && i.item1 !== undefined
        })
        console.log('JSONTYPEOF2: ', jsonobjectTnos)
        let jsonMapTnos5 = jsonobjectTnos.map((data, i) => {
          return {
            car_family_code: data.item1.slice(0, 4),
            new_version: data.item1.slice(4, 5),
            date_alc: data.item1.slice(0, 12),
            sequence_no: data.item1.slice(0, 3),
            time_alc: data.item1.slice(0, 4),
            shift_alc: data.item1.slice(0, 1),
            date_alc_2: data.item1.slice(0, 2),
            month_alc: data.item1.slice(0, 2),
            year_alc: data.item1.slice(0, 2),
            body: data.item1.slice(0, 5),
            id_no: data.item1.slice(0, 10),
            destination: data.item1.slice(0, 10),
            name_plate: data.item1.slice(0, 16),
            suffix: data.item1.slice(0, 2),
            lot_code: data.item1.slice(0, 2),
            frame_no: data.item1.slice(0, 17),
            color: data.item1.slice(0, 3),
            trim_code: data.item1.slice(0, 4),
            yom: data.item1.slice(0, 4),
            mom: data.item1.slice(0, 2),
            spare_1: data.item1.slice(0, 2),
            wheel_assy: data.item1.slice(0, 1),
            spare_2: data.item1.slice(0, 1),
            hood_lock: data.item1.slice(0, 1),
            spare_3: data.item1.slice(0, 1),
            inlet_air_cleaner: data.item1.slice(0, 1),
            abs_actuator: data.item1.slice(0, 2),
            spare_4: data.item1.slice(0, 3),
            parking_brake_cable: data.item1.slice(0, 1),
            back_glass_def: data.item1.slice(0, 1),
            spare_5: data.item1.slice(0, 3),
            plate_rocker_rh: data.item1.slice(0, 3),
            spare_6: data.item1.slice(0, 1),
            spare_7: data.item1.slice(0, 4),
            plate_rocker_lh: data.item1.slice(0, 3),
            spare_8: data.item1.slice(0, 1),
            ornament_hub: data.item1.slice(0, 2),
            owner_th: data.item1.slice(0, 2),
            fr_seat_belt: data.item1.slice(0, 1),
            spare_9: data.item1.slice(0, 1),
            clutch_mt: data.item1.slice(0, 1),
            ac_unit_condition: data.item1.slice(0, 1),
            air_condition: data.item1.slice(0, 1),
            spare10: data.item1.slice(0, 1),
            egine_cover: data.item1.slice(0, 1),
            hose_assy: data.item1.slice(0, 1),
            handle_stick_case: data.item1.slice(0, 1),
            spare_11: data.item1.slice(0, 5),
            hose_air_rh: data.item1.slice(0, 1),
            clamp_fuel: data.item1.slice(0, 1),
            spare_12: data.item1.slice(0, 1),
            bumper: data.item1.slice(0, 1),
            label_emission: data.item1.slice(0, 1),
            spare_13: data.item1.slice(0, 3),
            lamp_assy_rh_lh: data.item1.slice(0, 1),
            handle_sub: data.item1.slice(0, 1),
            spare_14: data.item1.slice(0, 8),
            pr_mudguard: data.item1.slice(0, 1),
            air_filter: data.item1.slice(0, 2),
            panel_fr_rh: data.item1.slice(0, 1),
            spare_15: data.item1.slice(0, 3),
            engine_type: data.item1.slice(0, 1),
            spare_16: data.item1.slice(0, 1),
            cab_type: data.item1.slice(0, 1),
            spare_17: data.item1.slice(0, 1),
            fr_seat_lh: data.item1.slice(0, 2),
            fr_seat_rh: data.item1.slice(0, 2),
            dect_type: data.item1.slice(0, 1),
            spare_18: data.item1.slice(0, 6),
            engine_type_2: data.item1.slice(0, 1),
            pump_assy_air: data.item1.slice(0, 1),
            spare_19: data.item1.slice(0, 2),
            panel_fr_lh: data.item1.slice(0, 1),
            seal_fr_up: data.item1.slice(0, 1),
            seal_fr_up_2: data.item1.slice(0, 1),
            seal_fender: data.item1.slice(0, 1),
            insualator_fr: data.item1.slice(0, 1),
            spare_20: data.item1.slice(0, 1),
            cooler_fuel: data.item1.slice(0, 1),
            connector_drain_hose: data.item1.slice(0, 1),
            sensor_assy_airbag: data.item1.slice(0, 2),
            hose_fuel_main: data.item1.slice(0, 1),
            spare_21: data.item1.slice(0, 3),
            lever_sa_con: data.item1.slice(0, 1),
            lever_sa_con_2: data.item1.slice(0, 1),
            spare_22: data.item1.slice(0, 5),
            filter_assy_fuel: data.item1.slice(0, 1),
            plate_emission: data.item1.slice(0, 2),
            spare_23: data.item1.slice(0, 4),
            link_as_power: data.item1.slice(0, 1),
            absorber_as_rh: data.item1.slice(0, 5),
            absorber_as_kh: data.item1.slice(0, 5),
            fr_coil_rh: data.item1.slice(0, 7),
            fr_foil_lh: data.item1.slice(0, 7),
            bar_stablizer: data.item1.slice(0, 3),
            spare_24: data.item1.slice(0, 3),
            shaft_assy: data.item1.slice(0, 3),
            pr_leaf_spring: data.item1.slice(0, 6),
            pr_shock_rh: data.item1.slice(0, 5),
            pr_shock_lh: data.item1.slice(0, 5),
            pr_axle: data.item1.slice(0, 3),
            band_sa_lh: data.item1.slice(0, 2),
            band_sa_no1: data.item1.slice(0, 2),
            spare_25: data.item1.slice(0, 2),
            wire_frame: data.item1.slice(0, 2),
            tank_assy_fuel: data.item1.slice(0, 2),
            spare_26: data.item1.slice(0, 2),
            hose_assy_washer: data.item1.slice(0, 1),
            spare_27: data.item1.slice(0, 1),
            guar_propelle: data.item1.slice(0, 2),
            trasmission: data.item1.slice(0, 3),
            spare_28: data.item1.slice(0, 1),
            fr_regula_lh: data.item1.slice(0, 2),
            fr_regula_rh: data.item1.slice(0, 2),
            spare_29: data.item1.slice(0, 5),
            themistor_cooler: data.item1.slice(0, 1),
            horn_assy_low: data.item1.slice(0, 1),
            clip_hose_water_no13: data.item1.slice(0, 1),
            clip_hose_ventilation: data.item1.slice(0, 1),
            spare_30: data.item1.slice(0, 1),
            cylinder_assy_clutch: data.item1.slice(0, 1),
            spare_31: data.item1.slice(0, 1),
            cable_sa_spiral: data.item1.slice(0, 1),
            spare_32: data.item1.slice(0, 2),
            transmission_4x2: data.item1.slice(0, 1),
            spare_33: data.item1.slice(0, 1),
            inter_cooler: data.item1.slice(0, 1),
            spare_34: data.item1.slice(0, 1),
            drl_relay: data.item1.slice(0, 1),
            tail_relay: data.item1.slice(0, 1),
            spare_35: data.item1.slice(0, 5),
            htr_reley: data.item1.slice(0, 1),
            ac_comp_relay: data.item1.slice(0, 1),
            spare_36: data.item1.slice(0, 6),
            hose_radiator_no2: data.item1.slice(0, 2),
            wire_earth: data.item1.slice(0, 3),
            cable_astm: data.item1.slice(0, 3),
            clam_qg_06: data.item1.slice(0, 1),
            bolt_qg_06: data.item1.slice(0, 1),
            spare_37: data.item1.slice(0, 1),
            clip_qg_06: data.item1.slice(0, 1),
            clip_hose_qg_06: data.item1.slice(0, 1),
            spare_38: data.item1.slice(0, 2),
            cushion_sa_upr: data.item1.slice(0, 1),
            cushion_sa_pr: data.item1.slice(0, 1),
            spare_39: data.item1.slice(0, 6),
            alternator_as: data.item1.slice(0, 3),
            spare_40: data.item1.slice(0, 2),
            clip_hose_qg_06_2: data.item1.slice(0, 1),
            spare_41: data.item1.slice(0, 3),
            insualator_eg_mount: data.item1.slice(0, 2),
            fan_alc: data.item1.slice(0, 2),
            clip_hose_qg_06_3: data.item1.slice(0, 1),
            clamp_hose_tr: data.item1.slice(0, 1),
            spare_42: data.item1.slice(0, 2),
            clip_hose_qg_06_4: data.item1.slice(0, 1),
            sensor_air_ratio: data.item1.slice(0, 2),
            sensor_oxygen_v6: data.item1.slice(0, 1),
            pipe_sa_tail: data.item1.slice(0, 2),
            cushion_sa_no3: data.item1.slice(0, 1),
            dome_cut_relay: data.item1.slice(0, 1),
            connect_startor: data.item1.slice(0, 2),
            spare_43: data.item1.slice(0, 7),
            louver_sub_assy_ventilator: data.item1.slice(0, 1),
            spare_44: data.item1.slice(0, 2),
            Carpet_alc: data.item1.slice(0, 6),
            spare_45: data.item1.slice(0, 1),
            ac_discharge_hose: data.item1.slice(0, 2),
            ac_suction_hose: data.item1.slice(0, 1),
            spare_46: data.item1.slice(0, 3),
            rea_exhaust_pipe: data.item1.slice(0, 2),
            spare_47: data.item1.slice(0, 8),
            rack_shock_pad: data.item1.slice(0, 2),
            fuel_inlet_pipe: data.item1.slice(0, 1),
            fuel_return_line: data.item1.slice(0, 1),
            switch_lamp: data.item1.slice(0, 1),
            rivet_90269: data.item1.slice(0, 1),
            wire_roof: data.item1.slice(0, 2),
            handle_assy_regulator: data.item1.slice(0, 1),
            shock_absorber_lhrh_4x2: data.item1.slice(0, 1),
            exhaust_pipe_heat: data.item1.slice(0, 2),
            camber_bolt: data.item1.slice(0, 1),
            spare_48: data.item1.slice(0, 1),
            battery: data.item1.slice(0, 5),
            spare_49: data.item1.slice(0, 5),
            sw_front_door_lh: data.item1.slice(0, 2),
            sw_front_door_rh: data.item1.slice(0, 2),
            ac_luquid_tube: data.item1.slice(0, 1),
            spare_50: data.item1.slice(0, 2),
            mat_assy_floor: data.item1.slice(0, 1),
            spare_51: data.item1.slice(0, 1),
            spare_wheel: data.item1.slice(0, 1),
            passenger_door: data.item1.slice(0, 3),
            passenger_door_2: data.item1.slice(0, 3),
            wheel_steering: data.item1.slice(0, 1),
            spare_52: data.item1.slice(0, 7),
            front_exhaust_pipe: data.item1.slice(0, 2),
            rear_exhuast_pipe: data.item1.slice(0, 2),
            clamp_clutch: data.item1.slice(0, 1),
            spare_53: data.item1.slice(0, 1),
            front_door_panel_lhrh: data.item1.slice(0, 1),
            spare_54: data.item1.slice(0, 3),
            lhd_dashboard: data.item1.slice(0, 1),
            tape_moulding: data.item1.slice(0, 1),
            mudguard_lhrh: data.item1.slice(0, 1),
            front_exhaust_pipe_2: data.item1.slice(0, 1),
            dashboard_rhd: data.item1.slice(0, 1),
            moter_fr_wipper: data.item1.slice(0, 1),
            spare_55: data.item1.slice(0, 1),
            console_box_mat: data.item1.slice(0, 1),
            spare_56: data.item1.slice(0, 1),
            cover_roof_headlining: data.item1.slice(0, 1),
            spare_57: data.item1.slice(0, 4),
            label_cooler: data.item1.slice(0, 1),
            spare_58: data.item1.slice(0, 5),
            jack_alc: data.item1.slice(0, 1),
            spare_59: data.item1.slice(0, 3),
            lamp_assy_rh: data.item1.slice(0, 1),
            lamp_assy_lh: data.item1.slice(0, 1),
            label_tire_rh: data.item1.slice(0, 2),
            label_tire_lh: data.item1.slice(0, 2),
            spare_60: data.item1.slice(0, 6),
            knob_sub_assy: data.item1.slice(0, 2),
            deck_type: data.item1.slice(0, 1),
            engine_type_3: data.item1.slice(0, 7),
            engine_capacity: data.item1.slice(0, 4),
            transmission_code: data.item1.slice(0, 5),
            spare_61: data.item1.slice(0, 4),
            gvm_kg: data.item1.slice(0, 4),
            spare_62: data.item1.slice(0, 1),
            gcvm_kg: data.item1.slice(0, 4),
            mpac_fr_kg: data.item1.slice(0, 4),
            mpac_rr_kg: data.item1.slice(0, 4),
            kata_1: data.item1.slice(0, 3),
            kata_2: data.item1.slice(0, 3),
            kata_3: data.item1.slice(0, 3),
            kata_4: data.item1.slice(0, 3),
            kata_5: data.item1.slice(0, 3),
            engine_1: data.item1.slice(0, 4),
            engine_2: data.item1.slice(0, 3),
            frame_no_1: data.item1.slice(0, 4),
            frame_no_2: data.item1.slice(0, 4),
            frame_no_3: data.item1.slice(0, 4),
            frame_no_4: data.item1.slice(0, 5),
            cleaner_assy: data.item1.slice(0, 1),
            spare_63: data.item1.slice(0, 3),
            spare_64: data.item1.slice(0, 3),
            switch_seat_heater: data.item1.slice(0, 1),
            handle_outside_rh: data.item1.slice(0, 1),
            handle_outside_lh: data.item1.slice(0, 1),
            label_emits: data.item1.slice(0, 1),
            spare_65: data.item1.slice(0, 2),
            cylinder_key_set: data.item1.slice(0, 3),
            wireless_door: data.item1.slice(0, 3),
            spare_66: data.item1.slice(0, 2),
            wheel_assy_steering: data.item1.slice(0, 2),
            gusset_sub_assy: data.item1.slice(0, 1),
            mudguard_body_rh: data.item1.slice(0, 1),
            mudguard_body_lh: data.item1.slice(0, 1),
            spare_67: data.item1.slice(0, 1),
            tire_tubeless: data.item1.slice(0, 2),
            wheel_assy_mix: data.item1.slice(0, 1),
            spare_68: data.item1.slice(0, 1),
            pad_assy_steering: data.item1.slice(0, 1),
            sw_as_head_dimmer: data.item1.slice(0, 1),
            spare_69: data.item1.slice(0, 4),
            manual_owner_mix: data.item1.slice(0, 2),
            spare_70: data.item1.slice(0, 6),
            name_label: data.item1.slice(0, 1),
            spare_71: data.item1.slice(0, 1),
            label_fuel_inform: data.item1.slice(0, 1),
            cover_relay_upr: data.item1.slice(0, 1),
            spare_72: data.item1.slice(0, 2),
            cylinder_assy_ir: data.item1.slice(0, 1),
            label_coolant: data.item1.slice(0, 1),
            lamp_assy_dome: data.item1.slice(0, 1),
            panel_sub_assy: data.item1.slice(0, 1),
            cover_sa_lwr: data.item1.slice(0, 1),
            parts_cooling_unit: data.item1.slice(0, 1),
            label_plate_fuel: data.item1.slice(0, 1),
            panel_instrum_rl: data.item1.slice(0, 1),
            tool_set_std_mix: data.item1.slice(0, 1),
            spare_wheel_brand: data.item1.slice(0, 3),
            sensor_speed_frrh: data.item1.slice(0, 1),
            shaft_sub_assy_no2: data.item1.slice(0, 1)
          }
        })
        this.jsondata2Tnos5 = jsonMapTnos5
        console.log('C1-4', this.jsondata2Tnos5)
        // axios.post('http://localhost:4000/conversiondias', this.jsondata2Tnos5).then(response => {
        //   console.log(response.data);
        // }).catch(error => {
        //   console.error('Error fetching data:', error.message);
        // });
      };
      reader.readAsBinaryString(file);
    },
    biasbc() {
      axios.get('http://localhost:4000/allconverdiasbc')
        .then(response => {
          console.log('resdata', response.data.result);
          let dataexcel = response.data.result
          this.alldatabiasbc = response.data.result
          let jsonMaps = dataexcel.map((data, i) => {
            return {
              "idmsgno": data.msgno,
              "msgno": data.submsgno,
              "conversioncharacter": data.conversioncharacter,
              "description": data.description,
              "mainpic": data.mainpic,
              "timeadd": data.timeadd,
              "og": data.og,
              "updates": data.updates
            }
          })
          // console.log('resdataExcel', jsonMaps);
          this.excelarray = Object.values(jsonMaps);
          // console.log('JSONTYPEOF2Aftermap23', this.excelarray)

          //export to excell
          const workbook = XLSX.utils.book_new();

          // Convert the JSON data to a worksheet
          const worksheet = XLSX.utils.json_to_sheet(this.excelarray);

          // Add the worksheet to the workbook
          XLSX.utils.book_append_sheet(workbook, worksheet, 'converbiasbc');

          // Save the workbook to a file
          XLSX.writeFile(workbook, 'exported_data.xlsx');
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    biasbc2() {
      axios.get('http://localhost:4000/allconverdiasbc')
        .then(response => {
          // console.log('resdata', response.data.result);
          let dataexcel = response.data.result
          this.alldatabiasbc = response.data.result
          console.log('detail', this.alldatabiasbc)
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    edittable(items) {
      console.log('edittable', items)
      axios.post('http://localhost:4000/updateconverbiasbc', items).then(response => {
        console.log(response.data);
      }).catch(error => {
        console.error('Error fetching data:', error.message);
      });
    }
  }
}
</script>