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
            date_alc: data.item1.slice(5, 17),
            sequence_no: data.item1.slice(17, 20),
            time_alc: data.item1.slice(20, 24),
            shift_alc: data.item1.slice(24, 25),
            date_alc_2: data.item1.slice(25, 27),
            month_alc: data.item1.slice(27, 29),
            year_alc: data.item1.slice(29, 31),
            body: data.item1.slice(31, 36),
            id_no: data.item1.slice(36, 46),
            destination: data.item1.slice(46, 56),
            name_plate: data.item1.slice(56, 72),
            suffix: data.item1.slice(72, 74),
            lot_code: data.item1.slice(74, 76),
            frame_no: data.item1.slice(76, 93),
            color: data.item1.slice(93, 96),
            trim_code: data.item1.slice(96, 100),
            yom: data.item1.slice(100, 104),
            mom: data.item1.slice(104, 106),
            spare_1: data.item1.slice(106, 108),
            wheel_assy: data.item1.slice(108, 109),
            spare_2: data.item1.slice(109, 110),
            hood_lock: data.item1.slice(110, 111),
            spare_3: data.item1.slice(111, 112),
            inlet_air_cleaner: data.item1.slice(112, 113),
            abs_actuator: data.item1.slice(113, 115),
            spare_4: data.item1.slice(115, 118),
            parking_brake_cable: data.item1.slice(118, 119),
            back_glass_def: data.item1.slice(119, 120),
            spare_5: data.item1.slice(120, 123),
            plate_rocker_rh: data.item1.slice(123, 126),
            spare_6: data.item1.slice(126, 127),
            spare_7: data.item1.slice(127, 131),
            plate_rocker_lh: data.item1.slice(131, 134),
            spare_8: data.item1.slice(134, 135),
            ornament_hub: data.item1.slice(135, 137),
            owner_th: data.item1.slice(137, 139),
            fr_seat_belt: data.item1.slice(139, 140),
            spare_9: data.item1.slice(140, 141),
            clutch_mt: data.item1.slice(141, 142),
            ac_unit_condition: data.item1.slice(142, 143),
            air_condition: data.item1.slice(143, 144),
            spare10: data.item1.slice(144, 145),
            egine_cover: data.item1.slice(145, 146),
            hose_assy: data.item1.slice(146, 147),
            handle_stick_case: data.item1.slice(147, 148),
            spare_11: data.item1.slice(148, 153),
            hose_air_rh: data.item1.slice(153, 154),
            clamp_fuel: data.item1.slice(154, 155),
            spare_12: data.item1.slice(155, 156),
            bumper: data.item1.slice(156, 157),
            label_emission: data.item1.slice(157, 158),
            spare_13: data.item1.slice(158, 161),
            lamp_assy_rh_lh: data.item1.slice(161, 162),
            handle_sub: data.item1.slice(162, 163),
            spare_14: data.item1.slice(163, 171),
            pr_mudguard: data.item1.slice(171, 172),
            air_filter: data.item1.slice(172, 174),
            panel_fr_rh: data.item1.slice(174, 175),
            spare_15: data.item1.slice(175, 178),
            engine_type: data.item1.slice(178, 179),
            spare_16: data.item1.slice(179, 180),
            cab_type: data.item1.slice(180, 181),
            spare_17: data.item1.slice(181, 182),
            fr_seat_lh: data.item1.slice(182, 184),
            fr_seat_rh: data.item1.slice(184, 186),
            dect_type: data.item1.slice(186, 187),
            spare_18: data.item1.slice(187, 193),
            engine_type_2: data.item1.slice(193, 194),
            pump_assy_air: data.item1.slice(194, 195),
            spare_19: data.item1.slice(195, 197),
            panel_fr_lh: data.item1.slice(197, 198),
            seal_fr_up: data.item1.slice(198, 199),
            seal_fr_up_2: data.item1.slice(199, 200),
            seal_fender: data.item1.slice(200, 201),
            insualator_fr: data.item1.slice(201, 202),
            spare_20: data.item1.slice(202, 203),
            cooler_fuel: data.item1.slice(203, 204),
            connector_drain_hose: data.item1.slice(204, 205),
            sensor_assy_airbag: data.item1.slice(205, 207),
            hose_fuel_main: data.item1.slice(207, 208),
            spare_21: data.item1.slice(208, 211),
            lever_sa_con: data.item1.slice(211, 212),
            lever_sa_con_2: data.item1.slice(212, 213),
            spare_22: data.item1.slice(213, 218),
            filter_assy_fuel: data.item1.slice(218, 219),
            plate_emission: data.item1.slice(219, 221),
            spare_23: data.item1.slice(221, 225),
            link_as_power: data.item1.slice(225, 226),
            absorber_as_rh: data.item1.slice(226, 231),
            absorber_as_kh: data.item1.slice(231, 236),
            fr_coil_rh: data.item1.slice(236, 243),
            fr_foil_lh: data.item1.slice(243, 250),
            bar_stablizer: data.item1.slice(250, 253),
            spare_24: data.item1.slice(253, 256),
            shaft_assy: data.item1.slice(256, 259),
            pr_leaf_spring: data.item1.slice(259, 265),
            pr_shock_rh: data.item1.slice(265, 270),
            pr_shock_lh: data.item1.slice(270, 275),
            pr_axle: data.item1.slice(275, 278),
            band_sa_lh: data.item1.slice(278, 280),
            band_sa_no1: data.item1.slice(280, 282),
            spare_25: data.item1.slice(282, 284),
            wire_frame: data.item1.slice(284, 286),
            tank_assy_fuel: data.item1.slice(286, 288),
            spare_26: data.item1.slice(288, 290),
            hose_assy_washer: data.item1.slice(290, 291),
            spare_27: data.item1.slice(291, 292),
            guar_propelle: data.item1.slice(292, 294),
            trasmission: data.item1.slice(294, 297),
            spare_28: data.item1.slice(297, 298),
            fr_regula_lh: data.item1.slice(298, 300),
            fr_regula_rh: data.item1.slice(300, 302),
            spare_29: data.item1.slice(302, 307),
            themistor_cooler: data.item1.slice(307, 308),
            horn_assy_low: data.item1.slice(308, 309),
            clip_hose_water_no13: data.item1.slice(309, 310),
            clip_hose_ventilation: data.item1.slice(310, 311),
            spare_30: data.item1.slice(311, 312),
            cylinder_assy_clutch: data.item1.slice(312, 313),
            spare_31: data.item1.slice(313, 314),
            cable_sa_spiral: data.item1.slice(314, 315),
            spare_32: data.item1.slice(315, 317),
            transmission_4x2: data.item1.slice(317, 318),
            spare_33: data.item1.slice(318, 319),
            inter_cooler: data.item1.slice(319, 320),
            spare_34: data.item1.slice(320, 321),
            drl_relay: data.item1.slice(321, 322),
            tail_relay: data.item1.slice(322, 323),
            spare_35: data.item1.slice(323, 328),
            htr_reley: data.item1.slice(328, 329),
            ac_comp_relay: data.item1.slice(329, 330),
            spare_36: data.item1.slice(330, 336),
            hose_radiator_no2: data.item1.slice(336, 338),
            wire_earth: data.item1.slice(338, 341),
            cable_astm: data.item1.slice(341, 344),
            clam_qg_06: data.item1.slice(344, 345),
            bolt_qg_06: data.item1.slice(345, 346),
            spare_37: data.item1.slice(346, 347),
            clip_qg_06: data.item1.slice(347, 348),
            clip_hose_qg_06: data.item1.slice(348, 349),
            spare_38: data.item1.slice(349, 351),
            cushion_sa_upr: data.item1.slice(351, 352),
            cushion_sa_pr: data.item1.slice(352, 353),
            spare_39: data.item1.slice(353, 359),
            alternator_as: data.item1.slice(359, 362),
            spare_40: data.item1.slice(362, 364),
            clip_hose_qg_06_2: data.item1.slice(364, 365),
            spare_41: data.item1.slice(365, 368),
            insualator_eg_mount: data.item1.slice(368, 370),
            fan_alc: data.item1.slice(370, 372),
            clip_hose_qg_06_3: data.item1.slice(372, 373),
            clamp_hose_tr: data.item1.slice(373, 374),
            spare_42: data.item1.slice(374, 376),
            clip_hose_qg_06_4: data.item1.slice(376, 377),
            sensor_air_ratio: data.item1.slice(377, 379),
            sensor_oxygen_v6: data.item1.slice(379, 380),
            pipe_sa_tail: data.item1.slice(380, 382),
            cushion_sa_no3: data.item1.slice(382, 383),
            dome_cut_relay: data.item1.slice(383, 384),
            connect_startor: data.item1.slice(384, 386),
            spare_43: data.item1.slice(386, 393),
            louver_sub_assy_ventilator: data.item1.slice(393, 394),
            spare_44: data.item1.slice(394, 396),
            Carpet_alc: data.item1.slice(396, 402),
            spare_45: data.item1.slice(402, 403),
            ac_discharge_hose: data.item1.slice(403, 405),
            ac_suction_hose: data.item1.slice(405, 406),
            spare_46: data.item1.slice(406, 409),
            rea_exhaust_pipe: data.item1.slice(409, 411),
            spare_47: data.item1.slice(411, 419),
            rack_shock_pad: data.item1.slice(419, 421),
            fuel_inlet_pipe: data.item1.slice(421, 422),
            fuel_return_line: data.item1.slice(422, 423),
            switch_lamp: data.item1.slice(423, 424),
            rivet_90269: data.item1.slice(424, 425),
            wire_roof: data.item1.slice(425, 427),
            handle_assy_regulator: data.item1.slice(427, 428),
            shock_absorber_lhrh_4x2: data.item1.slice(428, 429),
            exhaust_pipe_heat: data.item1.slice(429, 431),
            camber_bolt: data.item1.slice(431, 432),
            spare_48: data.item1.slice(432, 431),
            battery: data.item1.slice(431, 436),
            spare_49: data.item1.slice(436, 441),
            sw_front_door_lh: data.item1.slice(441, 443),
            sw_front_door_rh: data.item1.slice(443, 445),
            ac_luquid_tube: data.item1.slice(445, 446),
            spare_50: data.item1.slice(446, 448),
            mat_assy_floor: data.item1.slice(448, 450),
            spare_51: data.item1.slice(450, 451),
            spare_wheel: data.item1.slice(451, 452),
            passenger_door: data.item1.slice(452, 455),
            passenger_door_2: data.item1.slice(455, 458),
            wheel_steering: data.item1.slice(458, 459),
            spare_52: data.item1.slice(459, 466),
            front_exhaust_pipe: data.item1.slice(466, 468),
            rear_exhuast_pipe: data.item1.slice(468, 470),
            clamp_clutch: data.item1.slice(471, 472),
            spare_53: data.item1.slice(472, 473),
            front_door_panel_lhrh: data.item1.slice(473, 474),
            spare_54: data.item1.slice(474, 477),
            lhd_dashboard: data.item1.slice(477, 478),
            tape_moulding: data.item1.slice(478, 479),
            mudguard_lhrh: data.item1.slice(479, 480),
            front_exhaust_pipe_2: data.item1.slice(480, 481),
            dashboard_rhd: data.item1.slice(481, 482),
            moter_fr_wipper: data.item1.slice(482, 483),
            spare_55: data.item1.slice(483, 484),
            console_box_mat: data.item1.slice(484, 485),
            spare_56: data.item1.slice(485, 486),
            cover_roof_headlining: data.item1.slice(486, 487),
            spare_57: data.item1.slice(487, 491),
            label_cooler: data.item1.slice(491, 492),
            spare_58: data.item1.slice(492, 497),
            jack_alc: data.item1.slice(497, 498),
            spare_59: data.item1.slice(498, 501),
            lamp_assy_rh: data.item1.slice(501, 502),
            lamp_assy_lh: data.item1.slice(502, 503),
            label_tire_rh: data.item1.slice(503, 505),
            label_tire_lh: data.item1.slice(505, 507),
            spare_60: data.item1.slice(507, 513),
            knob_sub_assy: data.item1.slice(513, 515),
            deck_type: data.item1.slice(515, 516),
            engine_type_3: data.item1.slice(516, 523),
            engine_capacity: data.item1.slice(523, 527),
            transmission_code: data.item1.slice(527, 532),
            spare_61: data.item1.slice(532, 536),
            gvm_kg: data.item1.slice(536, 540),
            spare_62: data.item1.slice(540, 541),
            gcvm_kg: data.item1.slice(541, 545),
            mpac_fr_kg: data.item1.slice(545, 549),
            mpac_rr_kg: data.item1.slice(549, 553),
            kata_1: data.item1.slice(553, 556),
            kata_2: data.item1.slice(556, 559),
            kata_3: data.item1.slice(559, 562),
            kata_4: data.item1.slice(562, 565),
            kata_5: data.item1.slice(565, 568),
            engine_1: data.item1.slice(568, 572),
            engine_2: data.item1.slice(572, 575),
            frame_no_1: data.item1.slice(575, 579),
            frame_no_2: data.item1.slice(579, 583),
            frame_no_3: data.item1.slice(583, 587),
            frame_no_4: data.item1.slice(587, 592),
            cleaner_assy: data.item1.slice(592, 593),
            spare_63: data.item1.slice(593, 596),
            spare_64: data.item1.slice(596, 599),
            switch_seat_heater: data.item1.slice(599, 600),
            handle_outside_rh: data.item1.slice(600, 601),
            handle_outside_lh: data.item1.slice(601, 602),
            label_emits: data.item1.slice(602, 603),
            spare_65: data.item1.slice(603, 605),
            cylinder_key_set: data.item1.slice(605, 608),
            wireless_door: data.item1.slice(608, 611),
            spare_66: data.item1.slice(611, 613),
            wheel_assy_steering: data.item1.slice(613, 615),
            gusset_sub_assy: data.item1.slice(615, 616),
            mudguard_body_rh: data.item1.slice(616, 617),
            mudguard_body_lh: data.item1.slice(617, 618),
            spare_67: data.item1.slice(618, 619),
            tire_tubeless: data.item1.slice(619, 621),
            wheel_assy_mix: data.item1.slice(621, 622),
            spare_68: data.item1.slice(622, 623),
            pad_assy_steering: data.item1.slice(623, 624),
            sw_as_head_dimmer: data.item1.slice(624, 625),
            spare_69: data.item1.slice(625, 629),
            manual_owner_mix: data.item1.slice(629, 631),
            spare_70: data.item1.slice(631, 637),
            name_label: data.item1.slice(637, 638),
            spare_71: data.item1.slice(638, 639),
            label_fuel_inform: data.item1.slice(639, 640),
            cover_relay_upr: data.item1.slice(640, 641),
            spare_72: data.item1.slice(641, 643),
            cylinder_assy_ir: data.item1.slice(643, 644),
            label_coolant: data.item1.slice(644, 645),
            lamp_assy_dome: data.item1.slice(645, 646),
            panel_sub_assy: data.item1.slice(646, 647),
            cover_sa_lwr: data.item1.slice(647, 648),
            parts_cooling_unit: data.item1.slice(648, 649),
            label_plate_fuel: data.item1.slice(649, 650),
            panel_instrum_rl: data.item1.slice(650, 651),
            tool_set_std_mix: data.item1.slice(651, 652),
            spare_wheel_brand: data.item1.slice(652, 655),
            sensor_speed_frrh: data.item1.slice(655, 656),
            shaft_sub_assy_no2: data.item1.slice(656, 657)
          }
        })
        this.jsondata2Tnos5 = jsonMapTnos5
        console.log('C1-4', this.jsondata2Tnos5)
        axios.post('http://localhost:4000/alctxt').then(response => {
          console.log(response.data);
        }).catch(error => {
          console.error('Error fetching data:', error.message);
        });
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