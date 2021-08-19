<template>
  <div id="app" :class="classWeather">
    <main>
      <div class="title-box">
        <h1 class="title">Weather App</h1>
      </div>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar"
          placeholder="Search a city..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="btn-box">
        <div class="btn" @click="fetchWeather"><i class="fas fa-search-location fa-3x"></i></div>
      </div>
      <div class="warning-box" v-if="warning !== ''">
        <p class="warning-text">{{warning}}</p>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined' && warning === ''">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateFormat()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: '47ce24b37fe76e70a39f554e4e8e45a8',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      warning: ''
    }
  },
  methods: {
    setResults ( data ) {
      this.weather = data
    },
    async fetchWeather (e) {
      if (this.query !== '') {
        if (e.type === 'click') {
          try {
            const res = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)  
            if (!res.ok) {
              this.warning = 'The indicated city cannot be found'
              return
            }
            const data = await res.json()
            this.setResults( data )
            this.query = ''
            this.warning = ''
          } catch (error) {
            console.log(error)
          }
        } else {
          if (e.key === 'Enter') {
            try {
              const res = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)  
              if (!res.ok) {
                this.warning = 'The indicated city cannot be found'
                return
              }
              const data = await res.json()
              this.setResults( data )
              this.query = ''
              this.warning = ''
            } catch (error) {
              console.log(error)
            }
          }
        }
      }
    },
    dateFormat () {
      const d = new Date()
      const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']

      const day = days[d.getDay()]
      const date = d.getDate()
      const month = months[d.getMonth()]
      const year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  },
  computed: {
    classWeather () {
      if (typeof this.weather.main !== 'undefined' && this.warning === '') {
        if (this.weather.weather[0].main === 'Clouds') {
          return 'clouds'
        } else if (this.weather.weather[0].main === 'Rain' ) {
          return 'rain'
        } else if(this.weather.weather[0].main === 'Drizzle'){
          return 'drizzle'
        } else if(this.weather.weather[0].main === 'Mist'){
          return 'mist'
        } else {
          return 'clear'
        }
      }
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-size: cover;
  background-position: bottom;
  transition: .4s;
}

#app.clouds {
  background-image: url('./assets/clouds.jpg');
}

#app.rain {
  background-image: url('./assets/rain.jpg');
}

#app.clear {
  background-image: url('./assets/clear.jpg');
}

#app.drizzle {
  background-image: url('./assets/drizzle.jpg');
}

#app.mist {
  background-image: url('./assets/mist.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5));
}

.title-box {
  width: 100%;
  text-align: center;
  margin: 20px;
}

.title-box .title {
  color: #FFF;
  text-shadow: 1px 3px rgba(0, 0, 0, .25);
}

.search-box {
  width: 100%;
  margin-bottom: 25px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);  
  border-radius: 0 16px 0 16px;
  box-shadow: 0 0 8px rgba(0, 0, 0, .25);
  transition: .4s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0 0 16px rgba(0, 0, 0, .25);
  border-radius: 16px 0 16px 0;
}

.btn-box {
  text-align: center;
  margin-bottom: 15px;
}

.btn-box .btn {
  color: #FFF;
  cursor: pointer;
  transition: .4s;
}

.btn-box .btn:hover {
  color: #BBB;
}

.warning-box {
  width: 100%;
  margin: 15px;
  text-align: center;
}

.warning-box .warning-text {
  color: #FFF;
  font-size: 24px;
  font-style: italic;
  font-weight: 700;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, .25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  margin-top: 10px;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
}
</style>
