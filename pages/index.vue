<template>
  <main>
    <div v-if="loaded">
      <MainData :city="city" :temp="temperature" :overall="weatherSummary"/>
      <Toggle @fhValue="updateValue"/>
      <WeatherDetails :weather="weatherDetails"/>
    </div>
    <div v-else>
      <img src="@/assets/loading.gif" alt="loading.." class="loading">
    </div>
  </main>
</template>

<script>
export default {
  name: 'home',
  data() {
    return {
      city: '',
      country: '',
      lng: '',
      lat: '',
      showFh: false,
      temperature: 0,
      celsius: 0,
      weatherDetails: null,
      weatherSummary: '',
      loaded: false
    }
  },
  async mounted() {
    await this.fetchLocation()
    await this.checkLocalDataAndFetch()
  },
  methods: {
    async fetchLocation() {
      const request = await fetch("https://ipinfo.io/json?token=6eedbd96267ebb")
      const response = await request.json()
      this.city = response.city
      const loc = response.loc.split(',')
      this.lng = loc[0]
      this.lat = loc[1]
    },
    async fetchWeather() {
      const request = await fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${this.lng}&lon=${this.lat}&units=metric&appid=9ac31eb3e11f701b954ab3deef69b47d`)
      const response = await request.json()
      const time = Math.floor(Date.now() / 1000);
      this.weatherDetails = response
      this.weatherSummary = response.weather[0]?.main
      this.celsius = response.main.temp
      localStorage.setItem('lastLoadedTime', time)
      localStorage.setItem('weatherDetails', JSON.stringify(this.weatherDetails))
      localStorage.setItem('weatherSummary', this.weatherSummary)
      localStorage.setItem('celsius', this.celsius)
      this.showFh = localStorage.getItem('showFh')
      this.updateValue(this.showFh)
      this.loaded = true
    },
    updateValue(val) {
      this.showFh = val
      localStorage.setItem('showFh', this.showFh)
      this.temperature = (this.showFh) ? parseInt(((this.celsius * 9/5) + 32)).toFixed(0) : parseInt(this.celsius).toFixed(0)
    },
    async checkLocalDataAndFetch() {
      if (localStorage.getItem('location') === null) {
        localStorage.setItem('location', this.city)
        localStorage.setItem('showFh', this.showFh)
        await this.fetchWeather()
      } else {
        const lastUsedCity = localStorage.getItem('location')
        if (lastUsedCity === this.city) {
          const time = localStorage.getItem('lastLoadedTime')
          const timeNow = Math.floor(Date.now() / 1000);
          const diff = timeNow - time
          if (diff > 50) {
            await this.fetchWeather()
          } else {
            this.weatherDetails = JSON.parse(localStorage.getItem('weatherDetails'))
            this.weatherSummary = localStorage.getItem('weatherSummary')
            this.celsius = localStorage.getItem('celsius')
            this.showFh = localStorage.getItem('showFh')
            this.updateValue(this.showFh)
            this.loaded = true
          }
        } else {
          localStorage.setItem('location', this.city)
          await this.fetchWeather()
        }
      }
    }
  }
}
</script>
<style lang="scss">
* {
  padding: 0;
  margin: 0;
  user-select: none;
}

main {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100dvh;
}

.bold {
  font-weight: 800;
}

.loading {
  width: 64px;
}
</style>