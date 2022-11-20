<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <TimeRequirements :lpoc="lpoc" :destination="destination" />
    </v-main>
  </v-app>
</template>

<script>
import TimeRequirements from './components/TimeRequirements';
import lpoc from '../json/lpoc.json';
import destination from '../json/destination.json';
import { LPOC } from './type/lpoc';

export default {
  name: 'App',

  components: {
    TimeRequirements,
  },

  data: () => ({
    lpoc: undefined,
    destination: undefined,
  }),
  mounted() {
    this.lpoc = lpoc.map(
      item => new LPOC(item.ID, parseFloat(item.Miles.replace(/,/g, '')))
    ).sort((a, b) => a.label.localeCompare(b.label));
    this.destination = destination.map(
      item => new LPOC(item.Destination, parseFloat(item["Mile Marker"]))
    ).sort((a, b) => a.label.localeCompare(b.label));
  }
};
</script>
