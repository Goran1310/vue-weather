<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
      <div class="search-box">
        
        <input
          type="text"
          placeholder="Please input name of city ..."
          class="search-bar" 
          v-model="query"
          @keypress="fetchWeather"
          @keyup.enter="fetchWeather"
        />
        
    </div>
    <div class="wrap">Current weather data.
    Access current weather data for any location on Earth including over 200,000 cities.
    Weather data from different sources such as global and local weather models, satellites, radars and a vast network of weather stations. Data is available with API call in JSON
    </div>
    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
        <div class="date">{{dateBuilder()}}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}} °C</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
        <div class="humidity">Humidity: {{weather.main.humidity}} %</div>
        <div class="wind">Wind speed: {{weather.wind.speed}} meter/sec</div>
      </div>
    </div>
  </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
      return {
        api_key: '91d3def5bf5b9c78fb78208e7dc39db0',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {}
      }
  },
  methods: {
    fetchWeather(e) {
      if(e.key=='Enter') {
        // To make an API call 
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res=> {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
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
  margin:0;
  padding: 0;
  box-sizing:border-box;
}
body {
  font-family: sans-serif, Arial, Helvetica;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.35), rgba(0,0,0,0.85));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: darkgrey;
  font-size: 18px;
  appearance: none;
  border: none;
  outline: none;
  background: transparent;
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.75);
  background-color: rgba(255,255,255,0.5);
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius:  16px 0px 16px 0px;
}
.location-box .location {
  margin-top: 2vh;
  color: #FFFF;
  font-size: 29px;
  font-weight: bold;
  text-shadow: rgba(0,0,0,0.3);
}
.location-box .date {
  color: #FFFF;
  font-size: 21px;
  font-weight: bold;
  text-shadow: rgba(0,0,0,0.3);
}
.location-box .time {
    color: #FFFF;
  font-size: 21px;
  font-weight: bold;
  text-shadow: 3px 6px rgba(0,0,0,0.3);
}
.weather-box{
  color: #FFFF;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  font-size: 30px;
  font-weight: bolder;
  text-shadow: 3px 6px rgba(0,0,0,0.3);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.3);;
}
.weather-box .weather .wrap{
  color: #FFFF;
  font-size: 41px;
  font-weight: bold;
  text-shadow: 3px 6px rgba(0,0,0,0.3);
}
.humidity, .wind {
    color: #FFFF;
  font-size: 21px;
  font-weight: bold;
  text-shadow: 2px 4px rgba(0,0,0,0.3);
}
</style>
