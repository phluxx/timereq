<template>
  <v-container>
    <v-select label="LPOC" :items="lpoc" item-title="title" item-value="value" v-model="lpocmm" />
    <v-text-field label="LPOC Mile Marker" type="number" step="0.01" v-model="lpocmm" />
    <v-select label="Destination" :items="destination"  v-model="destmm" />
    <v-text-field label="Destination Mile Marker" type="number" step="0.01" id="destmm" name="destmm"
      v-model="destmm" />
    <v-text-field label="Time Submitted" type="datetime-local" v-model="subtime" />
    <v-text-field label="ETA Time" type="datetime-local" v-model="etatime" />
    <v-text-field label="Minimum Time" v-model="minimumTime" readonly />
    <v-text-field label="Cleared Time" v-model="clearedTime" readonly />
    <!--
      <tr>Minimum Time: <input readonly="readonly" type="datetime-local" id="mintime" name="mintime" v-model="minimumTime"></tr>
      <tr>Cleared Time: <input readonly="readonly" type="datetime-local" id="clrdtime" name="clrdtime" v-model="clearedTime"></tr>
      -->
  </v-container>
</template>

<script lang="ts">
declare global {
  interface Date {
    addHours: (h: number) => Date;
  }
}

Date.prototype.addHours = function (h: number) {
  this.setTime(this.getTime() + (h * 60 * 60 * 1000));
  return this;
}

export default {
  name: 'TimeRequirements',
  props: {
    lpoc: {
      type: Array,
      default: () => [],
    },
    destination: {
      type: Array,
      default: () => [],
    },
  },
  data: () => {
    return {
      publicPath: process.env.BASE_URL,
      subtime: new Date().toISOString().replace('Z', ''),
      etatime: new Date().toISOString().replace('Z', ''),
      destmm: 0.00,
      lpocmm: 0.00,
    }
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
      return new Date(this.etatime).addHours(-this.desthrs).toISOString().replace('Z', '')
    },
    minimumTime() {
      return new Date(this.subtime).addHours(this.lpochrs - this.desthrs).toISOString().replace('Z', '')
    }
  }
}
</script>
