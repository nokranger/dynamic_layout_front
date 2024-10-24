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
            car_family_code,
            new_version,
            date_alc,
            sequence_no,
            time_alc,
            shift_alc,
            date_alc_2,
            month_alc,
            year_alc,
            body,
            id_no,
            destination,
            name_plate,
            suffix,
            lot_code,
            frame_no,
            color,
            trim_code,
            yom,
            mom,
            spare_1,
            wheel_assy,
            spare_2,
            hood_lock,
            spare_3,
            inlet_air_cleaner,
            abs_actuator,
            spare_4,
            parking_brake_cable,
            back_glass_def,
            spare_5,
            plate_rocker_rh,
            spare_6,
            spare_7,
            plate_rocker_lh,
            spare_8,
            ornament_hub,
            owner_th,
            fr_seat_belt,
            spare_9,
            clutch_mt,
            ac_unit_condition,
            air_condition,
            spare10,
            egine_cover,
            hose_assy,
            handle_stick_case,
            spare_11,
            hose_air_rh,
            clamp_fuel,
            spare_12,
            bumper,
            label_emission,
            spare_13,
            lamp_assy_rh_lh,
            handle_sub,
            spare_14,
            pr_mudguard,
            air_filter,
            panel_fr_rh,
            spare_15,
            engine_type,
            spare_16,
            cab_type,
            spare_17,
            fr_seat_lh,
            fr_seat_rh,
            dect_type,
            spare_18,
            engine_type_2,
            pump_assy_air,
            spare_19,
            panel_fr_lh,
            seal_fr_up,
            seal_fr_up_2,
            seal_fender,
            insualator_fr,
            spare_20,
            cooler_fuel,
            connector_drain_hose,
            sensor_assy_airbag,
            hose_fuel_main,
            spare_21,
            lever_sa_con,
            lever_sa_con_2,
            spare_22,
            filter_assy_fuel,
            plate_emission,
            spare_23,
            link_as_power,
            absorber_as_rh,
            absorber_as_kh,
            fr_coil_rh,
            fr_foil_lh,
            bar_stablizer,
            spare_24,
            shaft_assy,
            pr_leaf_spring,
            pr_shock_rh,
            pr_shock_lh,
            pr_axle,
            band_sa_lh,
            band_sa_no1,
            spare_25,
            wire_frame,
            tank_assy_fuel,
            spare_26,
            hose_assy_washer,
            spare_27,
            guar_propelle,
            trasmission,
            spare_28,
            fr_regula_lh,
            fr_regula_rh,
            spare_29,
            themistor_cooler,
            horn_assy_low,
            clip_hose_water_no13,
            clip_hose_ventilation,
            spare_30,
            cylinder_assy_clutch,
            spare_31,
            cable_sa_spiral,
            spare_32,
            transmission_4x2,
            spare_33,
            inter_cooler,
            spare_34,
            drl_relay,
            tail_relay,
            spare_35,
            htr_reley,
            ac_comp_relay,
            spare_36,
            hose_radiator_no2,
            wire_earth,
            cable_astm,
            clam_qg_06,
            bolt_qg_06,
            spare_37,
            clip_qg_06,
            clip_hose_qg_06,
            spare_38,
            cushion_sa_upr,
            cushion_sa_pr,
            spare_39,
            alternator_as,
            spare_40,
            clip_hose_qg_06_2,
            spare_41,
            insualator_eg_mount,
            fan_alc,
            clip_hose_qg_06_3,
            clamp_hose_tr,
            spare_42,
            clip_hose_qg_06_4,
            sensor_air_ratio,
            sensor_oxygen_v6,
            pipe_sa_tail,
            cushion_sa_no3,
            dome_cut_relay,
            connect_startor,
            spare_43,
            louver_sub_assy_ventilator,
            spare_44,
            Carpet_alc,
            spare_45,
            ac_discharge_hose,
            ac_suction_hose,
            spare_46,
            rea_exhaust_pipe,
            spare_47,
            rack_shock_pad,
            fuel_inlet_pipe,
            fuel_return_line,
            switch_lamp,
            rivet_90269,
            wire_roof,
            handle_assy_regulator,
            shock_absorber_lhrh_4x2,
            exhaust_pipe_heat,
            camber_bolt,
            spare_48,
            battery,
            spare_49,
            sw_front_door_lh,
            sw_front_door_rh,
            ac_luquid_tube,
            spare_50,
            mat_assy_floor,
            spare_51,
            spare_wheel,
            passenger_door,
            passenger_door_2,
            wheel_steering,
            spare_52,
            front_exhaust_pipe,
            rear_exhuast_pipe,
            clamp_clutch,
            spare_53,
            front_door_panel_lhrh,
            spare_54,
            lhd_dashboard,
            tape_moulding,
            mudguard_lhrh,
            front_exhaust_pipe_2,
            dashboard_rhd,
            moter_fr_wipper,
            spare_55,
            console_box_mat,
            spare_56,
            cover_roof_headlining,
            spare_57,
            label_cooler,
            spare_58,
            jack_alc,
            spare_59,
            lamp_assy_rh,
            lamp_assy_lh,
            label_tire_rh,
            label_tire_lh,
            spare_60,
            knob_sub_assy,
            deck_type,
            engine_type_3,
            engine_capacity,
            transmission_code,
            spare_61,
            gvm_kg,
            spare_62,
            gcvm_kg,
            mpac_fr_kg,
            mpac_rr_kg,
            kata_1,
            kata_2,
            kata_3,
            kata_4,
            kata_5,
            engine_1,
            engine_2,
            frame_no_1,
            frame_no_2,
            frame_no_3,
            frame_no_4,
            cleaner_assy,
            spare_63,
            spare_64,
            switch_seat_heater,
            handle_outside_rh,
            handle_outside_lh,
            label_emits,
            spare_65,
            cylinder_key_set,
            wireless_door,
            spare_66,
            wheel_assy_steering,
            gusset_sub_assy,
            mudguard_body_rh,
            mudguard_body_lh,
            spare_67,
            tire_tubeless,
            wheel_assy_mix,
            spare_68,
            pad_assy_steering,
            sw_as_head_dimmer,
            spare_69,
            manual_owner_mix,
            spare_70,
            name_label,
            spare_71,
            label_fuel_inform,
            cover_relay_upr,
            spare_72,
            cylinder_assy_ir,
            label_coolant,
            lamp_assy_dome,
            panel_sub_assy,
            cover_sa_lwr,
            parts_cooling_unit,
            label_plate_fuel,
            panel_instrum_rl,
            tool_set_std_mix,
            spare_wheel_brand,
            sensor_speed_frrh,
            shaft_sub_assy_no2
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