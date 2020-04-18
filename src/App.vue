<template>
  <div id="app">
    <AsteroidGrid @remove="remove" :asteroids="asteroids" header="Near-Earth Objects" />
  </div>
</template>

<script>
  import AsteroidGrid from "./components/AsteroidGrid";
  import axios from 'axios'

  export default {
    name: 'App',
    components: {
      AsteroidGrid
    },
    data() {
      return {
        asteroids: []
      }
    },
    created() {
      this.fetchAsteroids();
    },
    methods: {
      fetchAsteroids: function () {
        var apiKey = 'ay7ydhbNUYBeBTfvbJ97eDyr9P1gKqBaVXJkBnDo';
        var url = 'https://api.nasa.gov/neo/rest/v1/neo/browse?api_key=' + apiKey;
        axios.get(url)
                .then(response => {
                  this.asteroids = response.data.near_earth_objects;
                })
      },
      remove(index) {
        this.asteroids.splice(index, 1);
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
