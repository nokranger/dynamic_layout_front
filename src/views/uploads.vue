<template>
  <div>
    <div>
      <b-container>
        <b-tabs content-class="mt-3">
          <br>
          <b-tab title="Main" active>
            <br>
            <mainPage></mainPage>
          </b-tab>
          <b-tab title="BC">
            <br>
            <upload></upload>
          </b-tab>
          <b-tab title="Conversion">
            <br>
            <upload2></upload2>
          </b-tab>
          <b-tab title="Setting Page">
            <br>
            <b-tabs content-class="mt-3">
              <b-tab title="Model">
                <setting></setting>
              </b-tab>
              <b-tab title="Set Working Station">
                <setting2></setting2>
              </b-tab>
              <!-- <b-tab title="Set Layout">
                <setting3></setting3>
              </b-tab> -->
              <!-- <b-tab title="Set Picture">
                <setting4></setting4>
              </b-tab> -->
            </b-tabs>
          </b-tab>
          <!-- <b-tab title="Text File">
            <br>
            <textfile></textfile>
          </b-tab> -->
        </b-tabs>
      </b-container>
    </div>
  </div>
</template>
<script>
import upload from '../views/upload.vue';
import upload2 from '../views/upload2.vue';
import setting from '../views/settingpage.vue';
import setting2 from '../views/settingpage2.vue';
import setting3 from '../views/setting3.vue';
import setting4 from '../views/setting4.vue';
import textfile from '../views/alctxt.vue';
import mainPage from '../views/dlayout.vue';
export default {
  components: {
    upload,
    upload2,
    setting,
    setting2,
    setting3,
    setting4,
    textfile,
    mainPage
  },
  data() {
    return {
    }
  }
}
</script>