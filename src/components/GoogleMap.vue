<template>
  <div>
    <div id="map-canvas" ref="map"></div>
  </div>
</template>

<script>
  export default {
    name: 'google-map',
    data () {
      return {
        lat: 37.566535,
        lng: 126.97769
      }
    },
    methods: {
      sendCoordinate () {
        this.$bus.$emit('sendCoordinate', {
          lat: this.lat,
          lng: this.lng
        })
      }
    },
    mounted () {
      /* global google */
      var center = new google.maps.LatLng(37.566535, 126.97769) // 서울
      var map = new google.maps.Map(this.$refs.map, {
        center: center,
        zoom: 12
      })

      var marker = new google.maps.Marker({
        position: center,
        map: map
      })

      map.addListener('center_changed', () => {
        marker.setPosition(map.getCenter())
      })

      map.addListener('dragend', () => {
        this.lat = map.getCenter().lat()
        this.lng = map.getCenter().lng()
        this.sendCoordinate()
      })

      this.sendCoordinate()
    }
  }
</script>

<style>
  #map-canvas {
    width: 400px;
    height: 600px;
  }
</style>