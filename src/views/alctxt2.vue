<template>
  <div>
    <div>
      <h1>
        Upload ALC Excel file
      </h1>
    </div>
    <div style="font-weight: bold;font-size: 20px;margin: 10px;">
    </div>
    <input type="file" ref="fileInput" @change="handleFileChangeTnos" />
    <br>
    <br>
    <div>
      <b-button variant="outline-success" v-on:click="biasbc">Download Excel</b-button>
    </div>
    <br>
    <br>
    <br>
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
            alctxt: data.item1
          }
        })
        this.jsondata2Tnos5 = jsonMapTnos5
        console.log('C1-4', this.jsondata2Tnos5)
        axios.post('http://localhost:4000/alctxt', this.jsondata2Tnos5).then(response => {
          console.log(response.data);
        }).catch(error => {
          console.error('Error fetching data:', error.message);
        });
      };
      reader.readAsBinaryString(file);
    },
    biasbc() {
      axios.get('http://localhost:4000/allalctxt')
        .then(response => {
          console.log('resdata', response.data.result);
          let dataexcel = response.data.result
          this.alldatabiasbc = response.data.result
          let jsonMaps = dataexcel.map((data, i) => {
            return {
              car_family_code: data.alctxt.slice(0, 4),
              new_version: data.alctxt.slice(4, 5),
              date_alc: data.alctxt.slice(5, 17),
              sequence_no: data.alctxt.slice(17, 20),
              time_alc: data.alctxt.slice(20, 24),
              shift_alc: data.alctxt.slice(24, 25),
              date_alc_2: data.alctxt.slice(25, 27),
              month_alc: data.alctxt.slice(27, 29),
              year_alc: data.alctxt.slice(29, 31),
              body: data.alctxt.slice(31, 36),
              id_no: data.alctxt.slice(36, 46),
              destination: data.alctxt.slice(46, 56),
              name_plate: data.alctxt.slice(56, 72),
              suffix: data.alctxt.slice(72, 74),
              lot_code: data.alctxt.slice(74, 76),
              frame_no: data.alctxt.slice(76, 93),
              color: data.alctxt.slice(93, 96),
              trim_code: data.alctxt.slice(96, 100),
              yom: data.alctxt.slice(100, 104),
              mom: data.alctxt.slice(104, 106),
              spare_1: data.alctxt.slice(106, 108),
              wheel_assy: data.alctxt.slice(108, 109),
              spare_2: data.alctxt.slice(109, 110),
              hood_lock: data.alctxt.slice(110, 111),
              spare_3: data.alctxt.slice(111, 112),
              inlet_air_cleaner: data.alctxt.slice(112, 113),
              abs_actuator: data.alctxt.slice(113, 115),
              spare_4: data.alctxt.slice(115, 118),
              parking_brake_cable: data.alctxt.slice(118, 119),
              back_glass_def: data.alctxt.slice(119, 120),
              spare_5: data.alctxt.slice(120, 123),
              plate_rocker_rh: data.alctxt.slice(123, 126),
              spare_6: data.alctxt.slice(126, 127),
              spare_7: data.alctxt.slice(127, 131),
              plate_rocker_lh: data.alctxt.slice(131, 134),
              spare_8: data.alctxt.slice(134, 135),
              ornament_hub: data.alctxt.slice(135, 137),
              owner_th: data.alctxt.slice(137, 139),
              fr_seat_belt: data.alctxt.slice(139, 140),
              spare_9: data.alctxt.slice(140, 141),
              clutch_mt: data.alctxt.slice(141, 142),
              ac_unit_condition: data.alctxt.slice(142, 143),
              air_condition: data.alctxt.slice(143, 144),
              spare10: data.alctxt.slice(144, 145),
              egine_cover: data.alctxt.slice(145, 146),
              hose_assy: data.alctxt.slice(146, 147),
              handle_stick_case: data.alctxt.slice(147, 148),
              spare_11: data.alctxt.slice(148, 153),
              hose_air_rh: data.alctxt.slice(153, 154),
              clamp_fuel: data.alctxt.slice(154, 155),
              spare_12: data.alctxt.slice(155, 156),
              bumper: data.alctxt.slice(156, 157),
              label_emission: data.alctxt.slice(157, 158),
              spare_13: data.alctxt.slice(158, 161),
              lamp_assy_rh_lh: data.alctxt.slice(161, 162),
              handle_sub: data.alctxt.slice(162, 163),
              spare_14: data.alctxt.slice(163, 171),
              pr_mudguard: data.alctxt.slice(171, 172),
              air_filter: data.alctxt.slice(172, 174),
              panel_fr_rh: data.alctxt.slice(174, 175),
              spare_15: data.alctxt.slice(175, 178),
              engine_type: data.alctxt.slice(178, 179),
              spare_16: data.alctxt.slice(179, 180),
              cab_type: data.alctxt.slice(180, 181),
              spare_17: data.alctxt.slice(181, 182),
              fr_seat_lh: data.alctxt.slice(182, 184),
              fr_seat_rh: data.alctxt.slice(184, 186),
              dect_type: data.alctxt.slice(186, 187),
              spare_18: data.alctxt.slice(187, 193),
              engine_type_2: data.alctxt.slice(193, 194),
              pump_assy_air: data.alctxt.slice(194, 195),
              spare_19: data.alctxt.slice(195, 197),
              panel_fr_lh: data.alctxt.slice(197, 198),
              seal_fr_up: data.alctxt.slice(198, 199),
              seal_fr_up_2: data.alctxt.slice(199, 200),
              seal_fender: data.alctxt.slice(200, 201),
              insualator_fr: data.alctxt.slice(201, 202),
              spare_20: data.alctxt.slice(202, 203),
              cooler_fuel: data.alctxt.slice(203, 204),
              connector_drain_hose: data.alctxt.slice(204, 205),
              sensor_assy_airbag: data.alctxt.slice(205, 207),
              hose_fuel_main: data.alctxt.slice(207, 208),
              spare_21: data.alctxt.slice(208, 211),
              lever_sa_con: data.alctxt.slice(211, 212),
              lever_sa_con_2: data.alctxt.slice(212, 213),
              spare_22: data.alctxt.slice(213, 218),
              filter_assy_fuel: data.alctxt.slice(218, 219),
              plate_emission: data.alctxt.slice(219, 221),
              spare_23: data.alctxt.slice(221, 225),
              link_as_power: data.alctxt.slice(225, 226),
              absorber_as_rh: data.alctxt.slice(226, 231),
              absorber_as_kh: data.alctxt.slice(231, 236),
              fr_coil_rh: data.alctxt.slice(236, 243),
              fr_foil_lh: data.alctxt.slice(243, 250),
              bar_stablizer: data.alctxt.slice(250, 253),
              spare_24: data.alctxt.slice(253, 256),
              shaft_assy: data.alctxt.slice(256, 259),
              pr_leaf_spring: data.alctxt.slice(259, 265),
              pr_shock_rh: data.alctxt.slice(265, 270),
              pr_shock_lh: data.alctxt.slice(270, 275),
              pr_axle: data.alctxt.slice(275, 278),
              band_sa_lh: data.alctxt.slice(278, 280),
              band_sa_no1: data.alctxt.slice(280, 282),
              spare_25: data.alctxt.slice(282, 284),
              wire_frame: data.alctxt.slice(284, 286),
              tank_assy_fuel: data.alctxt.slice(286, 288),
              spare_26: data.alctxt.slice(288, 290),
              hose_assy_washer: data.alctxt.slice(290, 291),
              spare_27: data.alctxt.slice(291, 292),
              guar_propelle: data.alctxt.slice(292, 294),
              trasmission: data.alctxt.slice(294, 297),
              spare_28: data.alctxt.slice(297, 298),
              fr_regula_lh: data.alctxt.slice(298, 300),
              fr_regula_rh: data.alctxt.slice(300, 302),
              spare_29: data.alctxt.slice(302, 307),
              themistor_cooler: data.alctxt.slice(307, 308),
              horn_assy_low: data.alctxt.slice(308, 309),
              clip_hose_water_no13: data.alctxt.slice(309, 310),
              clip_hose_ventilation: data.alctxt.slice(310, 311),
              spare_30: data.alctxt.slice(311, 312),
              cylinder_assy_clutch: data.alctxt.slice(312, 313),
              spare_31: data.alctxt.slice(313, 314),
              cable_sa_spiral: data.alctxt.slice(314, 315),
              spare_32: data.alctxt.slice(315, 317),
              transmission_4x2: data.alctxt.slice(317, 318),
              spare_33: data.alctxt.slice(318, 319),
              inter_cooler: data.alctxt.slice(319, 320),
              spare_34: data.alctxt.slice(320, 321),
              drl_relay: data.alctxt.slice(321, 322),
              tail_relay: data.alctxt.slice(322, 323),
              spare_35: data.alctxt.slice(323, 328),
              htr_reley: data.alctxt.slice(328, 329),
              ac_comp_relay: data.alctxt.slice(329, 330),
              spare_36: data.alctxt.slice(330, 336),
              hose_radiator_no2: data.alctxt.slice(336, 338),
              wire_earth: data.alctxt.slice(338, 341),
              cable_astm: data.alctxt.slice(341, 344),
              clam_qg_06: data.alctxt.slice(344, 345),
              bolt_qg_06: data.alctxt.slice(345, 346),
              spare_37: data.alctxt.slice(346, 347),
              clip_qg_06: data.alctxt.slice(347, 348),
              clip_hose_qg_06: data.alctxt.slice(348, 349),
              spare_38: data.alctxt.slice(349, 351),
              cushion_sa_upr: data.alctxt.slice(351, 352),
              cushion_sa_pr: data.alctxt.slice(352, 353),
              spare_39: data.alctxt.slice(353, 359),
              alternator_as: data.alctxt.slice(359, 362),
              spare_40: data.alctxt.slice(362, 364),
              clip_hose_qg_06_2: data.alctxt.slice(364, 365),
              spare_41: data.alctxt.slice(365, 368),
              insualator_eg_mount: data.alctxt.slice(368, 370),
              fan_alc: data.alctxt.slice(370, 372),
              clip_hose_qg_06_3: data.alctxt.slice(372, 373),
              clamp_hose_tr: data.alctxt.slice(373, 374),
              spare_42: data.alctxt.slice(374, 376),
              clip_hose_qg_06_4: data.alctxt.slice(376, 377),
              sensor_air_ratio: data.alctxt.slice(377, 379),
              sensor_oxygen_v6: data.alctxt.slice(379, 380),
              pipe_sa_tail: data.alctxt.slice(380, 382),
              cushion_sa_no3: data.alctxt.slice(382, 383),
              dome_cut_relay: data.alctxt.slice(383, 384),
              connect_startor: data.alctxt.slice(384, 386),
              spare_43: data.alctxt.slice(386, 393),
              louver_sub_assy_ventilator: data.alctxt.slice(393, 394),
              spare_44: data.alctxt.slice(394, 396),
              Carpet_alc: data.alctxt.slice(396, 402),
              spare_45: data.alctxt.slice(402, 403),
              ac_discharge_hose: data.alctxt.slice(403, 405),
              ac_suction_hose: data.alctxt.slice(405, 406),
              spare_46: data.alctxt.slice(406, 409),
              rea_exhaust_pipe: data.alctxt.slice(409, 411),
              spare_47: data.alctxt.slice(411, 419),
              rack_shock_pad: data.alctxt.slice(419, 421),
              fuel_inlet_pipe: data.alctxt.slice(421, 422),
              fuel_return_line: data.alctxt.slice(422, 423),
              switch_lamp: data.alctxt.slice(423, 424),
              rivet_90269: data.alctxt.slice(424, 425),
              wire_roof: data.alctxt.slice(425, 427),
              handle_assy_regulator: data.alctxt.slice(427, 428),
              shock_absorber_lhrh_4x2: data.alctxt.slice(428, 429),
              exhaust_pipe_heat: data.alctxt.slice(429, 431),
              camber_bolt: data.alctxt.slice(431, 432),
              spare_48: data.alctxt.slice(432, 431),
              battery: data.alctxt.slice(431, 436),
              spare_49: data.alctxt.slice(436, 441),
              sw_front_door_lh: data.alctxt.slice(441, 443),
              sw_front_door_rh: data.alctxt.slice(443, 445),
              ac_luquid_tube: data.alctxt.slice(445, 446),
              spare_50: data.alctxt.slice(446, 448),
              mat_assy_floor: data.alctxt.slice(448, 450),
              spare_51: data.alctxt.slice(450, 451),
              spare_wheel: data.alctxt.slice(451, 452),
              passenger_door: data.alctxt.slice(452, 455),
              passenger_door_2: data.alctxt.slice(455, 458),
              wheel_steering: data.alctxt.slice(458, 459),
              spare_52: data.alctxt.slice(459, 466),
              front_exhaust_pipe: data.alctxt.slice(466, 468),
              rear_exhuast_pipe: data.alctxt.slice(468, 470),
              clamp_clutch: data.alctxt.slice(471, 472),
              spare_53: data.alctxt.slice(472, 473),
              front_door_panel_lhrh: data.alctxt.slice(473, 474),
              spare_54: data.alctxt.slice(474, 477),
              lhd_dashboard: data.alctxt.slice(477, 478),
              tape_moulding: data.alctxt.slice(478, 479),
              mudguard_lhrh: data.alctxt.slice(479, 480),
              front_exhaust_pipe_2: data.alctxt.slice(480, 481),
              dashboard_rhd: data.alctxt.slice(481, 482),
              moter_fr_wipper: data.alctxt.slice(482, 483),
              spare_55: data.alctxt.slice(483, 484),
              console_box_mat: data.alctxt.slice(484, 485),
              spare_56: data.alctxt.slice(485, 486),
              cover_roof_headlining: data.alctxt.slice(486, 487),
              spare_57: data.alctxt.slice(487, 491),
              label_cooler: data.alctxt.slice(491, 492),
              spare_58: data.alctxt.slice(492, 497),
              jack_alc: data.alctxt.slice(497, 498),
              spare_59: data.alctxt.slice(498, 501),
              lamp_assy_rh: data.alctxt.slice(501, 502),
              lamp_assy_lh: data.alctxt.slice(502, 503),
              label_tire_rh: data.alctxt.slice(503, 505),
              label_tire_lh: data.alctxt.slice(505, 507),
              spare_60: data.alctxt.slice(507, 513),
              knob_sub_assy: data.alctxt.slice(513, 515),
              deck_type: data.alctxt.slice(515, 516),
              engine_type_3: data.alctxt.slice(516, 523),
              engine_capacity: data.alctxt.slice(523, 527),
              transmission_code: data.alctxt.slice(527, 532),
              spare_61: data.alctxt.slice(532, 536),
              gvm_kg: data.alctxt.slice(536, 540),
              spare_62: data.alctxt.slice(540, 541),
              gcvm_kg: data.alctxt.slice(541, 545),
              mpac_fr_kg: data.alctxt.slice(545, 549),
              mpac_rr_kg: data.alctxt.slice(549, 553),
              kata_1: data.alctxt.slice(553, 556),
              kata_2: data.alctxt.slice(556, 559),
              kata_3: data.alctxt.slice(559, 562),
              kata_4: data.alctxt.slice(562, 565),
              kata_5: data.alctxt.slice(565, 568),
              engine_1: data.alctxt.slice(568, 572),
              engine_2: data.alctxt.slice(572, 575),
              frame_no_1: data.alctxt.slice(575, 579),
              frame_no_2: data.alctxt.slice(579, 583),
              frame_no_3: data.alctxt.slice(583, 587),
              frame_no_4: data.alctxt.slice(587, 592),
              cleaner_assy: data.alctxt.slice(592, 593),
              spare_63: data.alctxt.slice(593, 596),
              spare_64: data.alctxt.slice(596, 599),
              switch_seat_heater: data.alctxt.slice(599, 600),
              handle_outside_rh: data.alctxt.slice(600, 601),
              handle_outside_lh: data.alctxt.slice(601, 602),
              label_emits: data.alctxt.slice(602, 603),
              spare_65: data.alctxt.slice(603, 605),
              cylinder_key_set: data.alctxt.slice(605, 608),
              wireless_door: data.alctxt.slice(608, 611),
              spare_66: data.alctxt.slice(611, 613),
              wheel_assy_steering: data.alctxt.slice(613, 615),
              gusset_sub_assy: data.alctxt.slice(615, 616),
              mudguard_body_rh: data.alctxt.slice(616, 617),
              mudguard_body_lh: data.alctxt.slice(617, 618),
              spare_67: data.alctxt.slice(618, 619),
              tire_tubeless: data.alctxt.slice(619, 621),
              wheel_assy_mix: data.alctxt.slice(621, 622),
              spare_68: data.alctxt.slice(622, 623),
              pad_assy_steering: data.alctxt.slice(623, 624),
              sw_as_head_dimmer: data.alctxt.slice(624, 625),
              spare_69: data.alctxt.slice(625, 629),
              manual_owner_mix: data.alctxt.slice(629, 631),
              spare_70: data.alctxt.slice(631, 637),
              name_label: data.alctxt.slice(637, 638),
              spare_71: data.alctxt.slice(638, 639),
              label_fuel_inform: data.alctxt.slice(639, 640),
              cover_relay_upr: data.alctxt.slice(640, 641),
              spare_72: data.alctxt.slice(641, 643),
              cylinder_assy_ir: data.alctxt.slice(643, 644),
              label_coolant: data.alctxt.slice(644, 645),
              lamp_assy_dome: data.alctxt.slice(645, 646),
              panel_sub_assy: data.alctxt.slice(646, 647),
              cover_sa_lwr: data.alctxt.slice(647, 648),
              parts_cooling_unit: data.alctxt.slice(648, 649),
              label_plate_fuel: data.alctxt.slice(649, 650),
              panel_instrum_rl: data.alctxt.slice(650, 651),
              tool_set_std_mix: data.alctxt.slice(651, 652),
              spare_wheel_brand: data.alctxt.slice(652, 655),
              sensor_speed_frrh: data.alctxt.slice(655, 656),
              shaft_sub_assy_no2: data.alctxt.slice(656, 657)
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
          XLSX.utils.book_append_sheet(workbook, worksheet, 'alctxt');

          // Save the workbook to a file
          XLSX.writeFile(workbook, 'alctxt.xlsx');
        })
        .catch(error => {
          console.error('Error fetching data:', error.message);
        });
    },
    biasbc2() {
      axios.get('http://localhost:4000/allalctxt')
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