<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <TimeRequirements/>
    </v-main>
  </v-app>
</template>

<script>
import TimeRequirements from './components/TimeRequirements';

export default {
  name: 'App',

  components: {
    TimeRequirements,
  },

  data: () => ({
    //
  }),
};
</script>
