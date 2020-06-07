<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.discordapp.com/attachments/718245186798485525/719149394766856202/logo.png"
          transition="scale-transition"
          width="40"
        />
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <Map/>
    </v-content>
  </v-app>
</template>

<script>
import { defineComponent } from '@vue/composition-api'
import Map from './components/Map';

export default defineComponent({
  name: 'App',
  components: {
    Map,
  },
  setup () {}
});
</script>
