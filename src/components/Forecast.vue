<template>
  <div>
    <ul class="forecasts">
      <li class="item" v-for="forecast in forecasts" :key="forecast.dt">
        <div class="weekday">{{ convertToDate(forecast.dt) | weekday }}</div>
        <div class="icon">
          <img 
            :src="`http://openweathermap.org/img/w/${forecast.weather[0].icon}.png`">
        </div>
        <div class="temp">{{ (forecast.temp.day - 273.15).toFixed(0) }}℃</div>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: 'forecast',
    data () {
      return {
        lat: 37.566535,
        lon: 126.977969,
        forecasts: []
      }
    },
    methods: {
      convertToDate (timestamp) {
        return new Date(timestamp * 1000)
      },
      setCoordinate (coordinate) {
        this.lat = coordinate.lat
        this.lon = coordinate.lng

        this.axios.get('http://api.openweathermap.org/data/2.5/forecast/daily', {
          params: {
            lat: this.lat,
            lon: this.lon,
            APPID: 'c5d698f455ddeab167e90c6f9a276f3e'
          }
        })
        .then(response => {
          const data = response.data
          this.forecasts = data.list
        })
      }
    },
    filters: {
      weekday (date) {
        let weekdayNames = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
        return weekdayNames[date.getDay()]
      }
    },
    mounted () {
      this.$bus.$on('sendCoordinate', this.setCoordinate)
    }
  }
</script>

<style scoped>
  .forecasts {
    display: flex;
    width: 80%;
    padding: 0;
    margin: 0 auto;
    align-items: center;
    justify-content: center;
  }
  .forecasts > li.item {
    position: relative;
    flex: 1;
    color: white;
    text-align: center;
    list-style-type: none;
  }
  .forecasts > li.item:first-child:before {
    content: '';
    position: absolute;
    top: -10px;
    left: 50%;
    display: block;
    width: 6px;
    height: 6px;
    margin-left: -3px;
    border-radius: 50%;
    background-color: red;
  }
  .forecasts > li.item > .temp {
    margin-top: -8px;
  }
</style>












