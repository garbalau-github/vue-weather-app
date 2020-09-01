<template>
  <div id="app" v-bind:class="typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input @keypress="fetchWeather" v-model="query" placeholder="Search..." type="text" class="search-bar">
      </div>

      <div v-if="typeof weather.main != 'undefined'" class="weather-wrap">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ getDate()}} </div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp)}}°с</div>
          <div class="weather">{{ weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: 'ffbf599a3e731bb10b766d1399a3048e',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather: function (e) {
      if (e.key === 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(data => data.json())
          .then(this.setResults)
      }
    },
    setResults: function (results) {
      this.weather = results;
      console.log(results);
    },
    getDate: function () {
      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`
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
  font-family: sans-serif;
}
#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: all 0.4s;
  width: 350px;
  margin: 0 auto;
  border-radius: 10px;
}
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
  transition: all 0.4s;
  border-radius: 10px;
}
main {
  min-height: 60vh;
  padding: 25px;
  margin-top: 50px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.7));
  border-radius: 10px;
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 10px;
  color: #313131;
  font-size: 18px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.35);
  transition: 0.4s;
  cursor: pointer;
  box-shadow: 0 0 10px rgba(0, 0, 0, .1);
}
.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0 0 10px rgba(0, 0, 0, .3);
}
.location-box .location {
  color: #fff;
  font-size: 30px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.15);
}
.location-box .date {
  margin-top: 5px;
  color: #fff;
  font-size: 18px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 15px 25px;
  color: #fff;
  font-size: 70px;
  font-weight: 900;
  border-radius: 15px;
  margin: 35px 0;
  background-color: rgba(255, 255, 255, 0.25);
  text-shadow: 2px 4px rgba(0, 0, 0, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 40px;
  font-weight: 700;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>