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

<script lang="ts">
import TimeRequirements from './components/TimeRequirements.vue';
import lpoc from '../json/lpoc.json';
import destination from '../json/destination.json';
import { defineComponent } from "vue";
import { LPOC } from './type/lpoc';

export default defineComponent({
  name: 'App',

  components: {
    TimeRequirements,
  },

  data: () => ({
    lpoc: undefined as LPOC[] | undefined,
    destination: undefined as LPOC[] | undefined,
  }),
  mounted() {
    this.lpoc = lpoc.map(
      item => new LPOC(item.ID, parseFloat(item.Miles.replace(/,/g, '')))
    ).sort((a, b) => a.title.localeCompare(b.title));
    this.destination = destination.map(
      item => new LPOC(item.Destination, item["Mile Marker"])
    ).sort((a, b) => a.title.localeCompare(b.title));
  }
});
</script>
