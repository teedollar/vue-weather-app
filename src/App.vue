<template>
  <div id="app" :class="typeof weather.list != 'undefined' && weather.list[0].main.temp > 16 ? 'warm' : ''">
    <main>

      <div class="row text-light">
        <div class="col-12 text-center">
          <h4>Check Weather Forecast For Other Cities</h4>
        </div>
      </div>
      <div class="row mt-2 text-light mb-4  text-center bg-dark py-3 mx-auto justify-content-center">
        <div class="col-2 city" @click="cityWeather('lagos')">Lagos</div>
        <div class="col-2 city" @click="cityWeather('abuja')">Abuja</div>
        <div class="col-2 city" @click="cityWeather('london')">London</div>
        <div class="col-2 city" @click="cityWeather('los angeles')">Los Angeles</div>
        <div class="col-2 city" @click="cityWeather('paris')">Paris</div>
      </div>

      <div class="weather-wrap" v-if="typeof weather.list != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.city.name }}, {{ weather.city.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.list[0].main.temp) }}°c</div>
          <div class="weather">{{ weather.list[0].weather[0].main }}</div>
        </div>

        <div class="row text-light mt-4">
            <div class="col text-center">
              <h6>Tomorrow</h6>
              <b class="daily">{{ Math.round(weather.list[8].main.temp) }}°c </b> <br>
              <b class="weather-type">{{ weather.list[8].weather[0].main }}</b>
            </div>
            <div class="col text-center">
              <h6>Next Tomorrow</h6>
              <b class="daily">{{ Math.round(weather.list[16].main.temp) }}°c </b> <br>
              <b class="weather-type">{{ weather.list[16].weather[0].main }}</b>
            </div>
            <div class="col text-center">
              <h6>3 days time</h6>
              <b class="daily">{{ Math.round(weather.list[23].main.temp) }}°c </b> <br>
              <b class="weather-type">{{ weather.list[23].weather[0].main }}</b>
            </div>
            <div class="col text-center">
              <h6>4 days time</h6>
              <b class="daily">{{ Math.round(weather.list[30].main.temp) }}°c </b> <br>
              <b class="weather-type">{{ weather.list[30].weather[0].main }}</b>
            </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import 'bootstrap'
import 'bootstrap/dist/css/bootstrap.min.css'

export default {
  name: 'app',
  data () {
    return {
      api_key: '08a9c800d7947a9033c9bdbf76f4585f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: 'Lagos',
      weather: {},
      location: null,
      coord: {
        lat: null,
        lng: null
      }
    }
  },
  created() {
    this.loadByDefault()
  },

  methods: {

    async loadByDefault() {
      await this.getLocation()
      //console.log('Lat'+ this.lng)
      if(this.coord.lat != null && this.coord.lng != null){
        console.log('Default')
        this.getWeatherByDefault();
      }
      else{
        console.log('Founded')
        this.getWeather()
      }
    },

    fetchWeather (e) {
      if (e.key == "Enter") {
        this.getWeather()
      }
    },

    getWeather() {
      fetch(`${this.url_base}forecast?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
    },

    getWeatherByDefault() {
      fetch(`${this.url_base}forecast?lat=${this.coord.lat}&lon=${this.coord.lng}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
    },

    setResults (results) {
      this.weather = results;
      //console.log(this.weather)
    },

    cityWeather(city) {
      this.query = city
      this.getWeather()
    },

    async getLocation() {
      await this.$getLocation({})
        .then(coordinates => {
          this.coord = coordinates
        })
        .catch( error => alert(error))

    },

    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
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
  font-family: 'montserrat', sans-serif;
}

.city {
  cursor: pointer;
}

.city:hover {
  font-size: 17px;
  font-weight: bolder;
}

.daily {
  font-size: 42px;
  font-weight: bolder;
  font-family: roboto sans-serif;
  padding-bottom: -10px;
}

.weather-type {
  font-size: 22px;
  font-weight: bolder;
}

#app {
  background-image: url('./assets/cold.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border:none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
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

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}


</style>