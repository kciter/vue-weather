<template>
  <div>
    <div class="location">{{ location }}</div>
    <div class="weather">{{ weather }}</div>
    <div class="temp">{{ temp }}℃</div>
  </div>
</template>

<script>
  export default {
    name: 'current-weather',
    data () {
      return {
        location: '',
        weather: '',
        temp: '',
        lat: 37.566535,
        lon: 126.977969
      }
    },
    methods: {
      setCoordinate (coordinate) {
        this.lat = coordinate.lat
        this.lon = coordinate.lng

        this.axios.get('http://api.openweathermap.org/data/2.5/weather', {
          params: {
            lat: this.lat,
            lon: this.lon,
            APPID: 'c5d698f455ddeab167e90c6f9a276f3e'
          }
        })
        .then(response => {
          let data = response.data
          this.location = data.name
          this.weather = data.weather[0].main
          this.temp = (data.main.temp - 273.15).toFixed(0)
        })
      }
    },
    mounted () {
      this.$bus.$on('sendCoordinate', this.setCoordinate)
    }
  }
</script>

<style scoped>
  .location {
    text-align: center;
    font-size: 40pt;
    color: white;
  }
  .weather {
    text-align: center;
    font-size: 20pt;
    color: white;
  }
  .temp {
    text-align: center;
    font-size: 50pt;
    color: white;
  }
</style>