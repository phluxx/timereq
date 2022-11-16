<template>
  <v-container>
    <table>
      <tr>LPOC: <v-select name="LPOC" id="lpoc" :items="lpoc" item-value="Miles" item-text="ID"
          v-on:change="changeLPOC"></v-select>
      </tr>
      <tr>LPOC MM: <input type="number" step="0.01" id="lpocmm" name="lpocmm" v-model="lpocmm"></tr>
      <tr>Destination: <v-select name="Destination" :items="destination" item-value="Mile Marker"
          item-text="Destination" v-on:change="changeDest"></v-select>
      </tr>
      <tr>Destination MM: <input type="number" step="0.01" id="destmm" name="destmm" v-model="destmm"></tr>
      <tr>Time Submitted: <input type="datetime-local" id="subtime" name="subtime" v-model="subtime"></tr>
      <tr>Minimum Time: {{minimumTime}}</tr>
      <tr>Cleared Time: {{clearedTime}}</tr>
      <!--
      <tr>Minimum Time: <input readonly="readonly" type="datetime-local" id="mintime" name="mintime" v-model="minimumTime"></tr>
      <tr>Cleared Time: <input readonly="readonly" type="datetime-local" id="clrdtime" name="clrdtime" v-model="clearedTime"></tr>
      -->
    </table>
  </v-container>
</template>

<script>
import lpoc from '../../json/lpoc.json';
import destination from '../../json/destination.json';

Date.prototype.addHours = function(h) {
  this.setTime(this.getTime() + (h*60*60*1000));
  return this;
}

export default {
  name: 'TimeRequirements',
  data: () => {
    return {
      publicPath: process.env.BASE_URL,
      subtime: new Date().toISOString().replace('Z', ''),
      destmm: 0.00,
      lpocmm: 0.00,
      lpoc: lpoc.sort((a, b) => a.ID.localeCompare(b.ID)),
      destination: destination.sort((a, b) => a.Destination.localeCompare(b.Destination))
    }
  },
  methods: {
    changeLPOC(lpocmm) {
      this.lpocmm = parseFloat(lpocmm.replace(/,/g, ''));
    },
    changeDest(destmm) {
      this.destmm = destmm;
    },
  },
  computed: {
    lpochrs() {
      return this.lpocmm / 14;
    },
    desthrs() {
      return this.destmm / 10
    },
    subtimehrs() {
      return this.lpochrs + this.desthrs;
    },
    clearedTime() {
      return new Date(this.subtime).addHours(this.subtimehrs).toISOString().replace('Z', '')
    },
    minimumTime() {
      return new Date(this.subtime).addHours(this.lpochrs).toISOString().replace('Z', '')
    }
  }
}
</script>

<style>

</style>
