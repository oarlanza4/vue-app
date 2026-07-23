<template>
  <h1>Earthquakes (Past 24h, Mag 2.5+)</h1>
  <br>
  <div v-if="magnitude.length === 0">
    Loading...
  </div>
  <div v-else>
    <p>{{ magnitude.length }} earthquakes found.</p>
  </div>
  <table>
    <thead>
      <tr>
        <th>Magnitude</th>
        <th>Place</th>
        <th>Time</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(mag, index) in magnitude" :key="index">
        <td>{{ mag }}</td>
        <td>{{ place[index] }}</td>
        <td>{{ new Date(time[index]).toLocaleString() }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',


  data () {
    return {
      magnitude: [],
      place: [],
      time: [],
    }
  },
  async mounted() {
    let response = await axios.get('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson')
    this.magnitude = response.data.features.map(feature => feature.properties.mag)
    this.place = response.data.features.map(feature => feature.properties.place)
    this.time = response.data.features.map(feature => feature.properties.time)
  },
  components: {
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
table {
  text-align: left;
  margin: 0;
  border-collapse: collapse;
}
th, td {
  border: 1px solid black;
  padding: 8px;
}
th {
  background-color: #f2f2f2;
  font-weight: bold;
}
</style>
