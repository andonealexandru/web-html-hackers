<template>
  <div>
    <div class="uk-margin-bottom">
      <h2>Search for your destination</h2>
      <GmapAutocomplete
          @place_changed='setPlace'
      />
      <button class="uk-margin-small-left" @click="addMarker">Search</button>
    </div>
    <GmapMap
      :center="center"
      :zoom="15"
      style="width: 100%; height: 400px;"
    >
      <GmapMarker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        @click="center=m.position"
      />
    </GmapMap>
  </div>
</template>

<script>
    export default {
        name: "Map",
        data() {
            return {
                center: { lat: 45.508, lng: -25.752 },
                currentPlace: null,
                markers: [],
                places: []
            }
        },
        mounted() {
            this.geolocate();
        },
        methods: {
            setPlace(place) {
                this.currentPlace = place
            },
            geolocate() {
                navigator.geolocation.getCurrentPosition(position => {
                    this.center = {
                        lat: position.coords.latitude,
                        lng: position.coords.longitude
                    }
                })
            },
            addMarker() {
                if (this.currentPlace) {
                    const marker = {
                        lat: this.currentPlace.geometry.location.lat(),
                        lng: this.currentPlace.geometry.location.lng()
                    }
                    this.markers.push({ position: marker })
                    this.places.push(this.currentPlace)
                    this.$store.commit('setCountry', this.currentPlace.address_components[this.currentPlace.address_components.length - 1].long_name)
                    this.center = marker
                    this.currentPlace = null
                }
            }
        }
    }
</script>

<style scoped>

</style>