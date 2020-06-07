<template>
  <MglMap :accessToken="accessToken" :mapStyle="mapStyle">
    <MglMarker v-for="(user, index) in users" :key="index" :coordinates="user.coords"
               :color="getColor(user)">
      <MglPopup>
        <v-card>
          <v-card-title class="headline"> {{ user.store.name }} </v-card-title>

          <v-card-text>
            This place currently has <strong> {{ user.store.count }} / {{ user.store.max }}</strong> people.
          </v-card-text>

          <v-input v-model="email"/>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
                    color="green darken-1"
                    text
                    @click="console.log('subscribing email: ' + email)"
            >
              Subscribe
            </v-btn>
          </v-card-actions>
        </v-card>
      </MglPopup>
    </MglMarker>
  </MglMap>
</template>

<script>
  import Mapbox from "mapbox-gl";
  import { MglMap, MglMarker, MglPopup } from "vue-mapbox";

  export default {
    name: 'Map',
    components: {
      MglMap,
      MglMarker,
      MglPopup
    },
    data: () => {
      const accessToken = 'pk.eyJ1IjoibXVzMjAwMy1hYmR1bCIsImEiOiJjazh0aHZwYWwwMDlqM250MXNseXhzOTZqIn0.wn4nYNkApTXegqQ0nb2gZQ'
      const mapStyle = 'mapbox://styles/mapbox/streets-v11'

      const users = [
        {
          coords: [-111.549668, 39.014],
          store: {
            name: 'Nofirlls',
            max: 100,
            count: 51
          }
        },
        {
          coords: [111.549668, -39.014],
          store: {
            name: 'Nofirlls',
            max: 100,
            count: 38
          }
        }
      ]

      return {
        accessToken, // your access token. Needed if you using Mapbox maps
        mapStyle,
        users,
        dialogue: false,
        email: ''// your map style
      };
    },
    methods: {
      getColor(user) {
        const cap = (user.store.count / user.store.max) * 100
        return cap >= 50 ? 'red' : 'blue'
      }
    },
    created() {
      this.mapbox = Mapbox;
      setInterval(async () => {
        this.users = []
        const res = await fetch('http://localhost:4567/api/store/list')
        const body = JSON.parse(await res.body)
        for(const s in body) {
          this.users.push({
            coords: [s.latitude, s.longitude],
            store: {
              name: s.store_name,
              max: s.max,
              count: s.count
            }
          })
        }
      }, 5000)
    }
  }
</script>
